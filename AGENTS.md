# AGENTS.md - Contexto Universal para Agentes de IA e LLMs

Bem-vindo ao repositório do **Rendash**. Este arquivo é o ponto de entrada canônico para qualquer agente inteligente, assistente de IA ou LLM (Cursor, Claude Code, GitHub Copilot, Windsurf, ChatGPT, Antigravity) que trabalhe nesta base de código.

---

## 1. O Que é o Rendash?

O **Rendash** é uma aplicação completa de inteligência e gestão financeira patrimonial voltada para os contextos **Pessoal** e **Empresarial (PJ - Lumina Studio)**, composta por:
- **Painel de Patrimônio & Rendimentos:** Gráficos sparkline, performance diária/mensal e comparativos com CDI/Selic/IPCA.
- **Ecossistema Open Finance Brasil:** Conexão multi-bancos (Itaú, Nubank, BB, Inter), consentimentos granulares e conciliação em tempo real.
- **Cartão Rendash Platinum:** Gestão de limites dinâmicos via slider nativo iOS, faturas detalhadas, cartões virtuais com CVV dinâmico e controles de segurança.
- **Investimentos & Mercado:** Portfólio diversificado (Renda Fixa, Ações, Fundos, Cripto) com simulação e liquidação D+0 / D+1.
- **Gestão Empresarial PJ:** Alternância rápida entre visão Pessoal e PJ, fluxo de caixa projetado para 30 dias, contas a pagar/receber e folha de pagamento.

---

## 2. Pilha Tecnológica Central

| Camada | Tecnologia | Descrição |
|---|---|---|
| **Arquitetura** | Monorepo Turborepo | Gerenciamento de múltiplos apps e pacotes compartilhados |
| **Mobile** | React Native + Expo SDK 52 | Expo Router v4 (file-based routing), iOS Native Style |
| **Estilização Mobile**| NativeWind v4 (Tailwind CSS) | Design tokens Dark Glassmorphism |
| **Backend / API** | Next.js App Router | Turbopack ativado, hospedado na Vercel |
| **Execução API** | Vercel Serverless Functions | Endpoints leves e escaláveis em `/app/api/...` |
| **Banco de Dados** | PostgreSQL | Vercel Postgres / Neon com suporte a pooling |
| **ORM** | Prisma ORM | Modelagem tipada, migrations e transações atômicas |
| **Validação** | Zod | Schemas estritos para validação de formulários e APIs |

---

## 3. Catálogo de Documentação Técnica

Consulte sempre os documentos na pasta `docs/` antes de implementar qualquer funcionalidade:

1. [**`docs/ARCHITECTURE.md`**](file:///c:/Users/Guicker/Desktop/Rendash/docs/ARCHITECTURE.md): Arquitetura completa, divisão de pacotes, monorepo e padrões.
2. [**`docs/SCREENS_ANALYSIS.md`**](file:///c:/Users/Guicker/Desktop/Rendash/docs/SCREENS_ANALYSIS.md): Mapeamento exaustivo de **todas as 46 telas do design**, incluindo rotas Expo Router, componentes visuais e comportamentos.
3. [**`docs/DATABASE_SCHEMA.prisma`**](file:///c:/Users/Guicker/Desktop/Rendash/docs/DATABASE_SCHEMA.prisma): Modelagem relacional do Prisma cobrindo Usuários, Contas, Transações, Pix, Investimentos, Cartões e PJ.
4. [**`docs/API_ROUTES.md`**](file:///c:/Users/Guicker/Desktop/Rendash/docs/API_ROUTES.md): Especificação dos endpoints Serverless do Next.js.
5. [**`docs/SETUP_GUIDE.md`**](file:///c:/Users/Guicker/Desktop/Rendash/docs/SETUP_GUIDE.md): Passo a passo para execução local com Expo, Turbopack e deploy na Vercel.

---

## 4. Skills Especializadas para LLMs

Na pasta [`.agents/skills/`](file:///c:/Users/Guicker/Desktop/Rendash/.agents/skills), você encontrará instruções direcionadas para cada domínio:
- [**`ios-design-system.md`**](file:///c:/Users/Guicker/Desktop/Rendash/.agents/skills/ios-design-system.md): Tokens de cor, botões pill, glassmorphism, haptics e estados visuais (PAGO, EM PROCESSO, BLOQUEADO, etc.).
- [**`prisma-postgres-vercel.md`**](file:///c:/Users/Guicker/Desktop/Rendash/.agents/skills/prisma-postgres-vercel.md): Singleton do Prisma, connection pooling com PgBouncer e transações financeiras com `$transaction`.
- [**`open-finance-engine.md`**](file:///c:/Users/Guicker/Desktop/Rendash/.agents/skills/open-finance-engine.md): Fluxo de consentimento OAuth mTLS, sincronização assíncrona e inteligência de categorização de Pix.
- [**`turbo-monorepo-setup.md`**](file:///c:/Users/Guicker/Desktop/Rendash/.agents/skills/turbo-monorepo-setup.md): Configuração do `turbo.json`, dependências internas e scripts.

---

## 5. Regras de Ouro para Agentes de Código

1. **Fidelidade ao iOS:** Os componentes mobile devem se parecer e se comportar como aplicativos nativos do iOS (Apple Human Interface Guidelines). Use cantos arredondados generosos (`rounded-3xl`), botões estilo pill, dark mode elegante com `#0B0E14` e haptic feedback em ações importantes.
2. **Sem Tipos `any`:** Utilize tipagem rigorosa em TypeScript. Compartilhe tipos através de `@rendash/shared` ou `@rendash/database`.
3. **Valores Monetários Seguros:** Use sempre o padrão BRL (`R$ 0,00`) e preserve a precisão de decimais em operações bancárias.
4. **Serverless Friendly:** Nunca mantenha conexões de banco de dados abertas indefinidamente; sempre use o Prisma singleton com pooling configurado.
