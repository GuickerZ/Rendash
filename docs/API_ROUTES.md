# Rendash - Especificação de Serverless API Routes (Next.js na Vercel)

Esta documentação define todos os endpoints Serverless (App Router `app/api/...`) que alimentam a aplicação mobile e web do **Rendash**.

Todas as rotas utilizam validação rigorosa de entrada com **Zod**, retorno padronizado em JSON e autenticação via cabeçalho `Authorization: Bearer <token>` ou cookies de sessão `HttpOnly`.

---

## 1. Módulo de Autenticação & Biometria (`/api/auth`)

### `POST /api/auth/register`
- **Descrição:** Inicia o cadastro do usuário (Etapa 1).
- **Body:**
  ```json
  {
    "name": "Guilherme Matias",
    "cpf": "12345678900",
    "email": "seuemail@exemplo.com",
    "phone": "11999998888",
    "password": "SenhaSegura123!"
  }
  ```
- **Retorno (201 Created):**
  ```json
  {
    "userId": "uuid",
    "status": "AWAITING_PHONE_VERIFICATION"
  }
  ```

### `POST /api/auth/otp/verify`
- **Descrição:** Valida o código PIN de 6 dígitos recebido por SMS/WhatsApp (Etapa 2).
- **Body:** `{ "userId": "uuid", "code": "481269" }`
- **Retorno (200 OK):** `{ "success": true, "token": "jwt_token", "refreshToken": "refresh_token" }`

### `POST /api/auth/login`
- **Descrição:** Autenticação tradicional por e-mail e senha.
- **Body:** `{ "email": "lucas.mendes@email.com", "password": "••••••••" }`
- **Retorno (200 OK):** Dados do usuário, tokens JWT e score de segurança.

### `POST /api/auth/biometrics`
- **Descrição:** Autentica o usuário pelo Touch ID / Face ID a partir de chave de dispositivo previamente vinculada.
- **Body:** `{ "userId": "uuid", "biometricToken": "device_signed_payload" }`
- **Retorno (200 OK):** Nova sessão autenticada.

---

## 2. Módulo Bancário, Extratos & Pix (`/api/banking`)

### `GET /api/banking/summary`
- **Descrição:** Retorna os dados consolidados do dashboard principal (Tela 7):
  - Patrimônio total acumulado (`R$ 124.850,32`).
  - Variação percentual e monetária do dia (`+2,41%`, `+ R$ 2.930,12`).
  - Mini cards: Reserva de emergência (`75%`, `R$ 15 mil`) e Rendimento no mês (`+ R$ 2.340`, `112% CDI`).
  - Histórico de pontos para o gráfico sparkline.

### `GET /api/banking/transactions`
- **Descrição:** Extrato unificado de transações (Telas 19 e 34).
- **Query Params:**
  - `month`: Mês de competência (Ex: `2026-09`)
  - `context`: `PESSOAL` ou `EMPRESA`
  - `category`: ID da categoria (opcional)
  - `bankId`: ID do banco conectado (opcional)
- **Retorno (200 OK):**
  ```json
  {
    "period": "Setembro 2026",
    "totalIncome": 12680.00,
    "totalExpense": 4259.00,
    "netBalance": 8420.30,
    "transactions": [
      {
        "id": "tx_1",
        "title": "Pix recebido · Marina Costa",
        "amount": 3200.00,
        "type": "INCOME",
        "category": "Receita · Serviços",
        "occurredAt": "2026-09-30T14:32:00Z",
        "bankName": "Rendash"
      }
    ]
  }
  ```

### `GET /api/pix/details/:id`
- **Descrição:** Retorna os metadados ricos do comprovante Pix e sugestão de categorização inteligente (Telas 35 e 37).

### `POST /api/pix/classify`
- **Descrição:** Atualiza a classificação do Pix e opcionalmente cria uma regra automática para transações futuras daquele contato (Tela 36).
- **Body:**
  ```json
  {
    "transactionId": "tx_1",
    "categoryId": "cat_servicos",
    "createAutoRule": true,
    "contactCpfMasked": "•••.312.780-••"
  }
  ```

---

## 3. Módulo de Investimentos & Mercado (`/api/investments`)

### `GET /api/investments/portfolio`
- **Descrição:** Dados da tela de Carteira (Tela 8):
  - Alocação percentual: Renda Fixa (45%), Ações (30%), Fundos (15%), Cripto (10%).
  - Total investido (`R$ 121.920,20`).
  - Lista de ativos com valor atual e rentabilidade.
  - Análise de risco da carteira (`Moderado`).

### `GET /api/investments/returns`
- **Descrição:** Performance consolidada de rendimentos com séries temporais (`7D`, `1M`, `3M`, `6M`, `1A`, `Tudo`) (Tela 9).

### `GET /api/investments/:id`
- **Descrição:** Detalhes de um ativo específico (Tela 11), incluindo gráfico de evolução de 3 meses, indicadores reais vs CDI e liquidez.

### `POST /api/investments/apply`
- **Descrição:** Execução de aplicação/investimento em ativo (Telas 14 e 15).
- **Body:** `{ "assetId": "selic_2029", "amount": 5000.00 }`
- **Retorno (200 OK):** Confirmação com status `EM_PROCESSO` e liquidação agendada para até 17h.

### `POST /api/investments/redeem`
- **Descrição:** Resgate parcial ou total de ativo com apuração de imposto de renda (Tela 16).
- **Body:** `{ "assetId": "selic_2029", "amount": 2500.00 }`
- **Retorno (200 OK):**
  ```json
  {
    "grossAmount": 2500.00,
    "incomeTax": 17.50,
    "netAmount": 2482.50,
    "settlement": "Hoje · D+0"
  }
  ```

---

## 4. Módulo Open Finance Brasil (`/api/open-finance`)

### `GET /api/open-finance/institutions`
- **Descrição:** Lista de instituições participantes para busca e conexão (Tela 24).

### `POST /api/open-finance/consent/init`
- **Descrição:** Gera o token de consentimento com as permissões selecionadas (Tela 25) e retorna o link seguro de redirecionamento OAuth 2.0.

### `POST /api/open-finance/sync`
- **Descrição:** Dispara a sincronização de contas, cartões e transações em tempo real (Tela 26).
- **Retorno via Server-Sent Events (SSE) ou Polling:** Notifica o progresso percentual (`80%`) e quantidade de itens importados.

---

## 5. Módulo de Cartão Rendash Platinum (`/api/cards`)

### `GET /api/cards/platinum`
- **Descrição:** Dados do cartão físico e virtual, limite disponível, pontos do programa de fidelidade e acessos a salas VIP (Tela 39).

### `GET /api/cards/invoice/current`
- **Descrição:** Fatura detalhada do mês com lançamentos agrupados por data e status das compras (Telas 40 e 43).

### `PATCH /api/cards/controls`
- **Descrição:** Atualização instantânea dos controles de segurança e limite do cartão (Telas 41 e 46):
- **Body:**
  ```json
  {
    "adjustedLimit": 8500.00,
    "allowContactless": true,
    "contactlessLimit": 200.00,
    "allowInternational": true,
    "requireBiometricOnline": true,
    "isBlocked": false
  }
  ```

---

## 6. Módulo Empresarial PJ (`/api/business`)

### `GET /api/business/dashboard`
- **Descrição:** Dados da empresa vinculada (Lumina Studio Ltda.):
  - Saldo empresarial consolidado (`R$ 286.310,80`).
  - Total a receber nos próximos 7 dias (`R$ 48,2 mil`).
  - Total a pagar em 12 vencimentos (`R$ 31,8 mil`).
  - Gráfico de fluxo de caixa projetado para 30 dias.
  - Próximas notas fiscais e folha de colaboradores.
