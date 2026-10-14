# Rendash - Arquitetura de Sistema & Monorepo

## 1. Visão Geral da Arquitetura

O **Rendash** é uma plataforma de inteligência financeira de alta performance para gestão de patrimônio pessoal e empresarial (PJ), com integração de Open Finance, cartões de crédito premium, acompanhamento de investimentos em tempo real e automação inteligente de despesas e proventos.

O ecossistema é projetado no formato **Monorepo Turborepo** com **TypeScript strict**:
- **Mobile (Android & iOS):** React Native + Expo SDK 52 (Expo Router v4), NativeWind (Tailwind CSS para React Native), Reanimated v3, Haptic Feedback e componentes fiéis às diretrizes de Human Interface Guidelines (estilo iOS).
- **Backend / Web API:** Next.js (App Router, Turbopack) hospedado na Vercel como **Vercel Serverless Functions & Edge API Routes**.
- **Camada de Dados & ORM:** Prisma ORM com PostgreSQL (Vercel Postgres / Neon / Supabase), com suporte nativo a pooling via PgBouncer / Prisma Accelerate.
- **Design System:** Baseado em tons escuros premium (#0B0E14, #121824, #1B2234), cantos arredondados generosos (`rounded-2xl` e `rounded-3xl`), tipografia refinada (SF Pro / Inter) e acentos em verde esmeralda ciano (`#00E599` e `#10B981`) para ações primárias e confirmações financeiras.

---

## 2. Estrutura do Monorepo Turborepo

```
rendash/
├── .agents/                          # Diretrizes e skills especializadas para IAs/LLMs
│   ├── SYSTEM_PROMPT.md              # Regras mestras de desenvolvimento
│   └── skills/                       # Skills modulares carregadas por assistentes
│       ├── ios-design-system.md      # Regras de componentes, tokens e UX iOS
│       ├── prisma-postgres-vercel.md # Boas práticas de modelagem e serverless
│       ├── open-finance-engine.md    # Especificações de integração Open Finance
│       └── turbo-monorepo-setup.md   # Setup do Turborepo e dependências
├── .github/
│   └── copilot-instructions.md       # Instruções para GitHub Copilot
├── docs/                             # Documentação técnica central
│   ├── ARCHITECTURE.md               # Este documento
│   ├── SCREENS_ANALYSIS.md           # Análise detalhada das 46 telas do design
│   ├── DATABASE_SCHEMA.prisma        # Schema Prisma completo
│   ├── API_ROUTES.md                 # Especificação dos endpoints Serverless
│   └── SETUP_GUIDE.md                # Guia de instalação e deploy
├── apps/
│   ├── mobile/                       # App Expo (React Native para iOS e Android)
│   │   ├── app/                      # Expo Router (Rotas baseadas em arquivos)
│   │   │   ├── (auth)/               # Login, Cadastro, 2FA, Biometria
│   │   │   ├── (tabs)/               # Início, Carteira, Rendimentos, Mercado, Perfil
│   │   │   ├── open-finance/         # Fluxo completo de conexão bancária
│   │   │   ├── cards/                # Cartão Rendash Platinum, faturas e limites
│   │   │   └── business/             # Visão empresarial (Lumina Studio PJ)
│   │   ├── components/               # Componentes UI (iOS Native style)
│   │   ├── hooks/                    # Hooks de estado, biometria e cache
│   │   ├── tailwind.config.js        # Configuração do NativeWind
│   │   └── package.json
│   └── web-api/                      # Next.js App Router (Serveless API + Dashboard Web)
│       ├── src/
│       │   ├── app/
│       │   │   └── api/              # Serverless Functions na Vercel
│       │   │       ├── auth/         # Autenticação JWT e WebAuthn / Biometria
│       │   │       ├── banking/      # Contas, Extrato e Pix
│       │   │       ├── investments/  # Carteira, Cotações e Rendimentos
│       │   │       ├── cards/        # Cartões, Faturas e Controles de Limite
│       │   │       └── open-finance/ # Integração com instituições bancárias
│       │   └── lib/                  # Prisma client, autenticação e validação Zod
│       ├── next.config.mjs           # Next.js com Turbopack ativado
│       └── package.json
├── packages/
│   ├── database/                     # Pacote Prisma compartilhado
│   │   ├── prisma/
│   │   │   └── schema.prisma         # Schema do Prisma central
│   │   ├── src/index.ts              # Export do PrismaClient singleton
│   │   └── package.json
│   ├── shared/                       # Tipos TypeScript, Schemas Zod e Constantes
│   │   ├── src/
│   │   │   ├── types/
│   │   │   ├── schemas/
│   │   │   └── utils/
│   │   └── package.json
│   └── ui/                           # Componentes de Design System agnósticos
│       ├── src/
│       └── package.json
├── AGENTS.md                         # Prompt global unificado para ferramentas de IA
├── CLAUDE.md                         # Instruções para Claude Code
├── .cursorrules                      # Regras para Cursor IDE
├── turbo.json                        # Pipeline Turborepo
├── package.json                      # Workspaces Root
└── README.md
```

---

## 3. Pilha Tecnológica Detalhada

### 3.1. Frontend Mobile (Expo & React Native)
- **Framework:** Expo SDK 52+ com **Expo Router v4** (Typed routes).
- **Estilização:** **NativeWind v4** (Tailwind CSS nativo) com design tokens customizados para Dark Mode de alto contraste e Glassmorphism.
- **Animações & Gestos:** `react-native-reanimated` v3, `react-native-gesture-handler` e `react-native-bottom-sheet` para sheets com fricção e física idênticas ao iOS.
- **Gráficos e Charts:** `react-native-wagmi-charts` / `victory-native` para gráficos de linha com sparkline dinâmica, curvas Bézier e gráficos de barras com tracking tátil.
- **Biometria & Segurança:** `expo-local-authentication` (Touch ID / Face ID) e `expo-secure-store` para armazenamento seguro de tokens de acesso e tokens biométricos.
- **Ícones & Design Tokens:** `lucide-react-native` estilizados para combinar perfeitamente com os ícones SF Symbols do ecossistema Apple.

### 3.2. Backend & Serverless API (Next.js na Vercel)
- **Framework:** Next.js (Turbopack, App Router).
- **Execução:** Vercel Serverless Functions (`nodejs20.x`) e Edge Functions para validações ultra-rápidas.
- **Validação de Entrada:** Zod para validação rigorosa de payloads de requisição.
- **Autenticação:** JWT seguro em cookies `HttpOnly` com refresh token rotation e suporte a autenticação biométrica via tokens de dispositivo vinculados.
- **Documentação de Rotas:** Tipos TypeScript ponta a ponta compartilhados entre o backend Next.js e o frontend Expo.

### 3.3. Banco de Dados & Camada de Persistência
- **Banco de Dados:** PostgreSQL hospedado na Vercel (Vercel Postgres / Neon Serverless Postgres).
- **ORM:** Prisma ORM com suporte a:
  - **Connection Pooling:** Prisma Accelerate ou PgBouncer no endpoint transacional (`?pgbouncer=true&connection_limit=10`) para evitar esgotamento de conexões em ambiente serverless.
  - **Índices Estruturados:** Índices em chaves estrangeiras, `userId`, timestamps e identificadores de transações Open Finance.
  - **Precisão Financeira:** Campos monetários mapeados estritamente como `Decimal(12, 2)` para evitar erros de ponto flutuante.

---

## 4. Padrões de Qualidade de Código & Diretrizes de IA
1. **Tipagem Estrita:** Nenhum tipo `any` é permitido. Todos os objetos de dados devem derivar de tipos gerados pelo Prisma ou schemas Zod do `@rendash/shared`.
2. **Componentes Orientados a Estados:** Todos os componentes de lista e cards suportam nativamente estados: `loading`, `empty`, `error`, `success` e `disabled` (conforme documentado na tela 45 do design).
3. **Internacionalização e Moeda:** Toda exibição de moeda segue o padrão BRL (`Intl.NumberFormat('pt-BR', { style: 'currency', currency: 'BRL' })`).
4. **Resiliência Serverless:** Tratamento de timeout em chamadas de Open Finance com retentativas idempotentes e logs estruturados em JSON.
