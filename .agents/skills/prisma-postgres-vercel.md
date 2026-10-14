# Skill: Prisma ORM + PostgreSQL no Vercel Serverless

Esta skill orienta o desenvolvimento da camada de banco de dados do **Rendash**, garantindo estabilidade e escalabilidade em ambientes Serverless na **Vercel** usando **Vercel Postgres (Neon)** e **Prisma ORM**.

---

## 1. Padrão Singleton do Prisma Client

Em Serverless Functions do Next.js, novas instâncias de funções lambda podem ser criadas sob demanda. Para evitar estourar o limite de conexões com o banco de dados, utilize sempre o padrão de cliente global com pooling:

```typescript
// packages/database/src/index.ts
import { PrismaClient } from '@prisma/client';

const globalForPrisma = globalThis as unknown as {
  prisma: PrismaClient | undefined;
};

export const prisma =
  globalForPrisma.prisma ??
  new PrismaClient({
    log: process.env.NODE_ENV === 'development' ? ['query', 'error', 'warn'] : ['error'],
  });

if (process.env.NODE_ENV !== 'production') globalForPrisma.prisma = prisma;

export * from '@prisma/client';
```

---

## 2. Configuração de Variáveis de Ambiente no Vercel Postgres

No arquivo `schema.prisma`:
```prisma
datasource db {
  provider  = "postgresql"
  url       = env("POSTGRES_PRISMA_URL")        // Utiliza pooling (porta 5432 / 6543 via PgBouncer)
  directUrl = env("POSTGRES_URL_NON_POOLING")    // Conexão direta necessária para executar migrations
}
```

---

## 3. Padrão de Transações Atômicas Financeiras

Ao executar operações que envolvem transferência de fundos, resgates ou aplicações de investimentos, **sempre utilize `prisma.$transaction`**:

```typescript
// Exemplo de Aplicação em Ativo de Investimento
export async function executeInvestment(
  userId: string,
  bankAccountId: string,
  assetId: string,
  amount: number
) {
  return await prisma.$transaction(async (tx) => {
    // 1. Verificar saldo disponível
    const account = await tx.bankAccount.findUniqueOrThrow({
      where: { id: bankAccountId, userId },
    });

    if (account.balance.toNumber() < amount) {
      throw new Error('Saldo insuficiente para esta aplicação');
    }

    // 2. Debitar o valor da conta
    await tx.bankAccount.update({
      where: { id: bankAccountId },
      data: { balance: { decrement: amount } },
    });

    // 3. Registrar ou atualizar a posição de investimento
    const asset = await tx.asset.findUniqueOrThrow({ where: { id: assetId } });
    const quantity = amount / asset.currentPrice.toNumber();

    const position = await tx.investmentPosition.upsert({
      where: {
        bankAccountId_assetId: { bankAccountId, assetId },
      },
      create: {
        bankAccountId,
        assetId,
        quantity,
        averagePrice: asset.currentPrice,
        totalInvested: amount,
        currentValue: amount,
        grossReturn: 0,
      },
      update: {
        quantity: { increment: quantity },
        totalInvested: { increment: amount },
        currentValue: { increment: amount },
      },
    });

    // 4. Registrar a transação no extrato
    const transaction = await tx.transaction.create({
      data: {
        bankAccountId,
        title: `Investimento ${asset.name}`,
        amount,
        type: 'INVESTMENT',
        status: 'PAGO',
        occurredAt: new Date(),
      },
    });

    return { position, transaction };
  });
}
```

---

## 4. Diretrizes de Performance
- Utilize sempre seleções restritas (`select: { id: true, balance: true }`) em consultas do dashboard para minimizar o tráfego de rede e o uso de memória das funções serverless.
- Sempre crie índices compostos em colunas frequentemente filtradas em conjunto, como `[bankAccountId, occurredAt]`.
