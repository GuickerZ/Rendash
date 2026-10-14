# Skill: Turborepo Monorepo Setup & Pipelines

Esta skill orienta a estrutura do monorepo, gerenciamento de dependências compartilhadas e pipelines de cache do **Turborepo** para o ecossistema Rendash.

---

## 1. Configuração do `turbo.json`

O arquivo `turbo.json` na raiz orquestra as tarefas entre a API Next.js e o App Expo:

```json
{
  "$schema": "https://turbo.build/schema.json",
  "globalDependencies": [
    "**/.env.*local",
    "**/.env"
  ],
  "tasks": {
    "build": {
      "dependsOn": ["^build", "^db:generate"],
      "outputs": [".next/**", "!.next/cache/**", "dist/**"]
    },
    "db:generate": {
      "cache": false
    },
    "db:migrate": {
      "cache": false
    },
    "lint": {
      "dependsOn": ["^lint"]
    },
    "dev": {
      "cache": false,
      "persistent": true
    }
  }
}
```

---

## 2. Pacotes Compartilhados Internos

1. **`@rendash/database`:** Exporta o cliente Prisma e os tipos gerados a partir do schema central.
2. **`@rendash/shared`:** Exporta constantes de domínio (categorias bancárias, taxas de conversão), schemas de validação Zod e utilitários de formatação de moeda.
3. **`@rendash/ui`:** Componentes de design tokens agnósticos ou temas comuns compartilhados.

---

## 3. Scripts no `package.json` Root

```json
{
  "name": "rendash",
  "private": true,
  "workspaces": [
    "apps/*",
    "packages/*"
  ],
  "scripts": {
    "dev": "turbo dev",
    "build": "turbo build",
    "lint": "turbo lint",
    "format": "prettier --write \"**/*.{ts,tsx,md}\"",
    "db:generate": "turbo run db:generate",
    "db:migrate": "pnpm --filter @rendash/database prisma migrate dev"
  }
}
```
