# Rendash - Guia de Setup, Execução Local e Deploy

Este guia detalha o passo a passo para configurar o ambiente de desenvolvimento, executar o monorepo localmente e realizar o deploy em produção na Vercel e no Expo.dev.

---

## 1. Pré-Requisitos

- **Node.js:** Versão `>= 20.10.0` (LTS recomendada).
- **Gerenciador de Pacotes:** `pnpm` (versão `>= 9.0.0`) ou `npm` com suporte a workspaces.
- **CLIs Globais:**
  ```bash
  npm install -g turbo eas-cli vercel
  ```
- **Mobile Development:**
  - **macOS (para iOS):** Xcode instalado com CocoaPods e iOS Simulator.
  - **Windows / Linux / macOS (para Android):** Android Studio com SDK configurado e Android Virtual Device (AVD).
  - **Aplicativo Expo Go:** Para testar diretamente em dispositivo físico.

---

## 2. Variáveis de Ambiente

Crie um arquivo `.env` na raiz do projeto ou configure nos respectivos workspaces:

```env
# ----------------------------------------------------
# BANCO DE DADOS (Vercel Postgres / Neon / Supabase)
# ----------------------------------------------------
POSTGRES_PRISMA_URL="postgres://default:secret@ep-cool-db.us-east-1.postgres.vercel-storage.com:5432/verceldb?pgbouncer=true&connect_timeout=15"
POSTGRES_URL_NON_POOLING="postgres://default:secret@ep-cool-db.us-east-1.postgres.vercel-storage.com:5432/verceldb"

# ----------------------------------------------------
# AUTENTICAÇÃO & SEGURANÇA
# ----------------------------------------------------
JWT_SECRET="sua_chave_secreta_jwt_super_longa_e_aleatoria"
REFRESH_TOKEN_SECRET="sua_chave_refresh_token_super_longa"
ENCRYPTION_KEY="chave_de_32_bytes_para_dados_sensiveis"

# ----------------------------------------------------
# NEXT.JS & VERCEL
# ----------------------------------------------------
NEXT_PUBLIC_API_URL="http://localhost:3000/api"
NEXTAUTH_URL="http://localhost:3000"

# ----------------------------------------------------
# EXPO / MOBILE
# ----------------------------------------------------
EXPO_PUBLIC_API_URL="http://10.0.2.2:3000/api" # Para emulador Android
# EXPO_PUBLIC_API_URL="http://localhost:3000/api" # Para simulador iOS
EXPO_PUBLIC_APP_ENV="development"
```

---

## 3. Instalação e Inicialização Local

### 3.1. Instalar Dependências do Monorepo
```bash
pnpm install
# ou npm install
```

### 3.2. Gerar Prisma Client e Rodar Migrations
```bash
# Executa as migrações no banco de dados PostgreSQL
pnpm --filter @rendash/database prisma migrate dev --name init

# Gera os tipos TypeScript tipados do Prisma
pnpm --filter @rendash/database prisma generate
```

### 3.3. Iniciar o Ambiente de Desenvolvimento Completo com Turbo
```bash
turbo dev
```
Isso iniciará concorrentemente:
- **`apps/web-api`:** Servidor Next.js com Turbopack em `http://localhost:3000`.
- **`apps/mobile`:** Metro bundler do Expo em `http://localhost:8081`.

---

## 4. Executando o App Mobile

Dentro de `apps/mobile`:
- **iOS Simulator:**
  ```bash
  pnpm --filter mobile ios
  ```
- **Android Emulator:**
  ```bash
  pnpm --filter mobile android
  ```
- **Dispositivo Físico:** Abra o aplicativo Expo Go e leia o QR Code gerado pelo terminal do Metro.

---

## 5. Deploy em Produção

### 5.1. Deploy da API e Web na Vercel
1. Conecte o repositório GitHub à Vercel:
   ```bash
   vercel link
   ```
2. Adicione a integração do **Vercel Postgres** pelo painel da Vercel. As variáveis `POSTGRES_PRISMA_URL` e `POSTGRES_URL_NON_POOLING` serão injetadas automaticamente.
3. Configure o Build Command no painel do projeto:
   - **Framework Preset:** Next.js
   - **Root Directory:** `apps/web-api` (ou configure via `turbo build`)
   - **Build Command:** `pnpm turbo build`
4. Dispare o deploy:
   ```bash
   vercel --prod
   ```

### 5.2. Build do App Mobile com EAS Build (Expo.dev)
1. Configure o login no EAS:
   ```bash
   eas login
   ```
2. Inicialize o projeto no Expo Application Services:
   ```bash
   eas project:init
   ```
3. Gere as builds de produção para lojas (App Store e Google Play):
   ```bash
   # Build iOS (IPA para TestFlight e App Store)
   eas build --platform ios --profile production

   # Build Android (AAB para Google Play)
   eas build --platform android --profile production
   ```
