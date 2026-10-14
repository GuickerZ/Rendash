# Rendash - Gestão Patrimonial & Inteligência Financeira

> Plataforma integrada de inteligência e controle patrimonial com ecossistema Open Finance Brasil, cartões de crédito premium, acompanhamento de investimentos em tempo real e visão híbrida Pessoal & Empresarial (PJ).

---

## 📱 Pilha Tecnológica (Stack)

- **Mobile:** [React Native](https://reactnative.dev/) + [Expo SDK 52](https://expo.dev/) (Expo Router v4)
- **Estilização Mobile:** [NativeWind v4](https://www.nativewind.dev/) (Tailwind CSS)
- **Design System:** Inspirado nas diretrizes do **Apple iOS Human Interface Guidelines** (Dark Theme com Glassmorphism e Haptic Feedback)
- **Backend / Web API:** [Next.js](https://nextjs.org/) (App Router, Turbopack) executando como **Vercel Serverless Functions**
- **Banco de Dados:** [PostgreSQL](https://www.postgresql.org/) ([Vercel Postgres](https://vercel.com/docs/storage/vercel-postgres) / Neon)
- **ORM:** [Prisma ORM](https://www.prisma.io/) com pooling via PgBouncer
- **Monorepo:** [Turborepo](https://turbo.build/repo)

---

## 🧭 Catálogo de Documentação Técnica

Toda a arquitetura e especificações de telas foram meticulosamente documentadas:

- 📐 [**Arquitetura do Sistema (`docs/ARCHITECTURE.md`)**](file:///c:/Users/Guicker/Desktop/Rendash/docs/ARCHITECTURE.md): Visão detalhada da estrutura de pacotes, monorepo e padrões arquiteturais.
- 📱 [**Análise Completa das 46 Telas (`docs/SCREENS_ANALYSIS.md`)**](file:///c:/Users/Guicker/Desktop/Rendash/docs/SCREENS_ANALYSIS.md): Mapeamento tela a tela (Páginas 1 a 46 do PDF), componentes iOS, fluxos de navegação e contratos de API.
- 🗄️ [**Schema do Banco de Dados (`docs/DATABASE_SCHEMA.prisma`)**](file:///c:/Users/Guicker/Desktop/Rendash/docs/DATABASE_SCHEMA.prisma): Modelagem relacional para Open Finance, investimentos, transações, Pix com IA, PJ e cartões.
- ⚡ [**Especificação de Serverless API Routes (`docs/API_ROUTES.md`)**](file:///c:/Users/Guicker/Desktop/Rendash/docs/API_ROUTES.md): Endpoints Next.js com payloads e validação Zod.
- 🚀 [**Guia de Setup e Deploy (`docs/SETUP_GUIDE.md`)**](file:///c:/Users/Guicker/Desktop/Rendash/docs/SETUP_GUIDE.md): Passo a passo para rodar localmente no simulador/emulador e subir para Vercel e EAS.

---

## 🤖 Contexto para Agentes de IA & LLMs

Este projeto possui suporte nativo para ferramentas de inteligência artificial (Cursor, Claude Code, GitHub Copilot, Windsurf, Antigravity):

- [**`AGENTS.md`**](file:///c:/Users/Guicker/Desktop/Rendash/AGENTS.md): Manifesto central com instruções universais para assistentes de IA.
- [**`.agents/SYSTEM_PROMPT.md`**](file:///c:/Users/Guicker/Desktop/Rendash/.agents/SYSTEM_PROMPT.md): Regras mestras de desenvolvimento e padrões de qualidade de código.
- [**`.agents/skills/`**](file:///c:/Users/Guicker/Desktop/Rendash/.agents/skills): Skills modulares:
  - 🎨 [`ios-design-system.md`](file:///c:/Users/Guicker/Desktop/Rendash/.agents/skills/ios-design-system.md): Tokens Tailwind e componentes com estilo nativo Apple.
  - 💾 [`prisma-postgres-vercel.md`](file:///c:/Users/Guicker/Desktop/Rendash/.agents/skills/prisma-postgres-vercel.md): Boas práticas para Vercel Serverless e PostgreSQL.
  - 🏦 [`open-finance-engine.md`](file:///c:/Users/Guicker/Desktop/Rendash/.agents/skills/open-finance-engine.md): Fluxo de consentimentos e classificação inteligente de Pix.
  - 📦 [`turbo-monorepo-setup.md`](file:///c:/Users/Guicker/Desktop/Rendash/.agents/skills/turbo-monorepo-setup.md): Orquestração do Turborepo e dependências.

---

## 🚀 Como Executar

```bash
# 1. Instalar dependências
pnpm install

# 2. Executar migrações do banco de dados
pnpm db:migrate

# 3. Iniciar o ambiente completo de desenvolvimento (API + Mobile)
pnpm dev
```
