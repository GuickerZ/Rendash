# Rendash - Análise Completa de Telas & Especificação de UI (Páginas 1 a 46)

Este documento contém o mapeamento detalhado e exaustivo de todas as 46 interfaces visuais do aplicativo **Rendash**, organizadas por módulos funcionais, com indicação de componentes nativos iOS, rotas do Expo Router, comportamentos interativos e contratos de dados.

---

## Índice de Módulos

1. [Módulo 1: Onboarding, Autenticação & Recuperação de Acesso (Páginas 1 - 6)](#modulo-1)
2. [Módulo 2: Visão Geral, Dashboard & Navegação Principal (Página 7)](#modulo-2)
3. [Módulo 3: Carteira, Rendimentos & Detalhes de Ativos (Páginas 8 - 13)](#modulo-3)
4. [Módulo 4: Operações de Investimento & Resgate (Páginas 14 - 16)](#modulo-4)
5. [Módulo 5: Mercado Financeiro & Cotações em Tempo Real (Página 17)](#modulo-5)
6. [Módulo 6: Planejamento Financeiro, Metas & Extrato (Páginas 18 - 19)](#modulo-6)
7. [Módulo 7: Notificações, Perfil & Segurança Avançada (Páginas 20 - 22)](#modulo-7)
8. [Módulo 8: Open Finance Brasil - Conexão, Permissões & Sincronização (Páginas 23 - 31)](#modulo-8)
9. [Módulo 9: Visão Empresarial PJ & Fluxo de Caixa (Páginas 32 - 33)](#modulo-9)
10. [Módulo 10: Extrato Unificado, Pix Inteligente & Categorias (Páginas 34 - 38)](#modulo-10)
11. [Módulo 11: Cartão Rendash Platinum, Faturas & Controles (Páginas 39 - 46)](#modulo-11)

---

<a id="modulo-1"></a>
## Módulo 1: Onboarding, Autenticação & Recuperação de Acesso

### Página 1: Onboarding Splash & Boas-Vindas
- **Rota Expo:** `apps/mobile/app/(auth)/index.tsx`
- **Componentes Visuais:**
  - Logo "Rendash" no topo centralizado.
  - Anéis concêntricos com efeito de luz esférica e gradiente escuro.
  - Card flutuante de indicador: ícone de tendência ascendente + badge `RENTABILIDADE +12,8% a.a.`.
  - Título com tipografia premium: *"Invista com clareza. Cresça com confiança."*.
  - Subtítulo: *"Acompanhe seu patrimônio, descubra oportunidades e alcance seus objetivos em um só lugar."*.
  - Indicador de paginação (3 dots).
  - Botão Primário: Pill arredondado em tom cinza claro/branco com texto `➔ Começar`.
- **Interações:** Toque em "Começar" navega para a tela de Login ou Cadastro.

### Página 2: Login com E-mail e Senha
- **Rota Expo:** `apps/mobile/app/(auth)/login.tsx`
- **Componentes Visuais:**
  - Header com logo "Rendash".
  - Título H1 *"Bem-vindo de volta"* com subtítulo de apoio.
  - Inputs em cartões brancos/claros com cantos arredondados (`rounded-2xl`): Campo de E-mail e Campo de Senha mascarada (`••••••••`).
  - Link de ação secundária: *"Esqueceu a senha?"*.
  - Botão Primário: `Entrar` (estilo pill elevado).
  - Botão Secundário: `Criar minha conta` (estilo outlined escuro com borda translúcida).
  - Termos de uso e rodapé de privacidade.
- **Interações:** Validação Zod em tempo real; feedback tátil ao submeter.

### Página 3: Login Rápido com Biometria (Touch ID / Face ID)
- **Rota Expo:** `apps/mobile/app/(auth)/biometrics.tsx`
- **Componentes Visuais:**
  - Subtítulo com dot indicador: `• SEU PATRIMÔNIO, EM MOVIMENTO`.
  - Avatar circular com iniciais `GM` (Guilherme Matias).
  - Saudação personalizada: *"Olá, Guilherme"*.
  - Ícone vetorial estilizado de impressão digital (Touch ID / Face ID) com círculos de foco concêntricos.
  - Botão Primário com ícone de biometria: `Entrar com biometria`.
  - Botão Secundário com ícone de chave: `Usar minha senha`.
  - Link de alternância de usuário: *"Não é Lucas? Trocar de conta"*.
- **Interações:** Acionamento automático da API nativa `LocalAuthentication.authenticateAsync()`.

### Página 4: Cadastro - Etapa 1 de 3 (Dados Básicos)
- **Rota Expo:** `apps/mobile/app/(auth)/register-step1.tsx`
- **Componentes Visuais:**
  - Header de navegação com botão Voltar (`chevron-left`) e indicador `Etapa 1 de 3`.
  - Título: *"Vamos começar pelos seus dados"*.
  - Badge informativa: *"Leva menos de 3 minutos. Seus dados são protegidos de ponta a ponta."*.
  - Inputs com labels flutuantes e botões de limpeza (clear icon `✕`):
    - Nome completo
    - CPF (com máscara automática `000.000.000-00` e validação de dígito verificador)
    - E-mail
    - Celular (com máscara `(00) 00000-0000`)
  - Checkbox de consentimento legal: *"Li e aceito os Termos de Uso e a Política de Privacidade"*.
  - Botão de avanço: `➔ Continuar`.

### Página 5: Verificação de Celular - Etapa 2 de 3 (2FA SMS / WhatsApp)
- **Rota Expo:** `apps/mobile/app/(auth)/register-step2-otp.tsx`
- **Componentes Visuais:**
  - Ícone de segurança de mensagem SMS dentro de círculo translúcido.
  - Texto de destino: *"Enviamos um código de 6 dígitos para o número +55 11 •••••-4821"*.
  - Grid de 6 caixas individuais para digitação de PIN com auto-focus sequencial.
  - Timer regressivo de reenvio: *"Reenviar código em 00:42"*.
  - Link alternativo: *"Receber por ligação"*.
  - Botão de validação: `Verificar código`.
  - Card de segurança anti-fraude: ícone de escudo + aviso *"Nunca compartilhe este código. A Rendash não solicita sua senha por telefone."*.

### Página 6: Recuperação de Acesso e Redefinição de Senha
- **Rota Expo:** `apps/mobile/app/(auth)/forgot-password.tsx`
- **Componentes Visuais:**
  - Ícone de chave destacada.
  - Título: *"Vamos ajudar você a voltar"*.
  - Campo para e-mail cadastrado com validação instantânea (`E-mail encontrado`).
  - Stepper de processo com 3 etapas:
    1. Confirme o link enviado por e-mail
    2. Valide sua identidade com biometria
    3. Crie uma nova senha de acesso
  - Botão Primário: `Enviar link seguro` (ícone de envio).
  - Botão Secundário: `Voltar ao login`.

---

<a id="modulo-2"></a>
## Módulo 2: Visão Geral, Dashboard & Navegação Principal

### Página 7: Home Dashboard Pessoal
- **Rota Expo:** `apps/mobile/app/(tabs)/index.tsx`
- **Componentes Visuais:**
  - Top Bar: Saudação personalizada (*"Boa tarde, Guilherme Matias"*), botão de notificações com badge de contador.
  - Card Principal de Patrimônio:
    - Label `PATRIMÔNIO TOTAL` com ícone de alternância de visibilidade (olho fechado/aberto).
    - Valor principal em destaque: `R$ 124.850,32`.
    - Badge de variação diária: `+2,41%  + R$ 2.930,12 hoje`.
    - Gráfico sparkline com curva suave renderizado via SVG.
  - Barra de Ações Rápidas em Pills Horizontais:
    - `+ Investir`
    - `↓ Resgatar`
    - `❖ Pix`
    - `➔ Transferir`
  - Mini Cards de Destaque (Grid 2 colunas):
    - Card 1: Progresso da Reserva (`75% Reserva - R$ 15 mil`).
    - Card 2: Performance no Mês (`+ R$ 2.340 - 112% do CDI`).
  - Grid de Acessos Rápidos (4 botões em cards com ícones):
    - Extrato
    - Bancos
    - Investimentos
    - Cartão
  - Tab Bar Inferior Fixa com Efeito Glassmorphism:
    - Início | Carteira | Rendimentos | Mercado | Perfil

---

<a id="modulo-3"></a>
## Módulo 3: Carteira, Rendimentos & Detalhes de Ativos

### Página 8: Minha Carteira & Alocação de Ativos
- **Rota Expo:** `apps/mobile/app/(tabs)/wallet.tsx`
- **Componentes Visuais:**
  - Header com status *"Atualizado agora"* e botão de filtros.
  - Donut Chart SVG de Alocação Patrimonial:
    - Renda Fixa: 45% (Dot azul/ciano)
    - Ações: 30% (Dot branco)
    - Fundos: 15% (Dot cinza médio)
    - Cripto: 10% (Dot cinza escuro)
    - Centro com valor consolidado: `100% | R$ 121.920,20`.
  - Seção `ATIVOS NA CARTEIRA` com link `Ver todos`:
    - Tesouro Selic 2029 (Renda fixa · 112% CDI): `R$ 35.200`
    - PETR4 · Petrobras (42 ações · +6,4%): `R$ 18.450`
    - Bitcoin (0,043 BTC · +18,2%): `R$ 12.500`
    - Fundo Atlas Macro (Cota D+1 · +8,1%): `R$ 28.770`
  - Banner de Inteligência Financeira: Ícone de estrela + *"Sua carteira está bem diversificada. Risco atual: moderado."*.

### Página 9: Performance de Rendimentos & Proventos
- **Rota Expo:** `apps/mobile/app/(tabs)/returns.tsx`
- **Componentes Visuais:**
  - Header com botão de informação (`info`).
  - Segmented Control de Períodos: `7D | 1M | 3M | 6M | 1A | Tudo`.
  - Indicador principal: `Rendimento em Setembro - R$ 2.340,50 (+1,92%)`.
  - Gráfico de barras verticais com histórico diário.
  - Mini Cards com KPIs:
    - Acumulado 1M: `R$ 2.340`
    - Yield Médio: `102% CDI`
    - Melhor Dia: `+ R$ 180`
  - Histórico Recente de Entradas:
    - Tesouro Selic 2029: `+ R$ 45,20` (Hoje, 09:12)
    - Dividendos PETR4: `+ R$ 145,20` (02 set, 14:30)
    - Fundo Atlas Macro: `+ R$ 92,80` (01 set, 18:00)

### Página 10: Resumo Rápido de Ações Secundárias
- **Componente:** Bottom Sheet / Card de Confirmação Rápida.
- **Elementos:** Exibição sintética de valores de liquidação (`Valor a investir: R$ 5.000,00 - Hoje, até 17h` e `Valor líquido: R$ 2.482,50 - D+0`).

### Página 11: Detalhes do Ativo Individual (Ex: Tesouro Selic 2029)
- **Rota Expo:** `apps/mobile/app/investments/[id].tsx`
- **Componentes Visuais:**
  - Título do ativo e saldo atual: `R$ 35.200,00` (+8,2% acumulado).
  - Card de evolução gráfica de 3 meses (`+ R$ 2.700`).
  - Grid de Indicadores-Chave (4 quadrantes):
    - Rentabilidade Real: `+ 8,2%`
    - Desempenho CDI: `112% CDI`
    - Valor Investido: `R$ 32.500`
    - Rendimento Bruto: `R$ 2.700`
  - Linhas de liquidez e vencimento: `Liquidez: D+0 | Vencimento: 01 mar 2029`.
  - Barra de Botões Dupla: `Resgatar` (secundário) e `Investir mais` (primário).

### Página 12: Gráficos de Evolução e Comparação
- **Componente:** Visão analítica avançada com sobreposição gráfica de histórico do ativo versus benchmarks de mercado (CDI, Selic e IBOV).

### Página 13: Comparativo de Indicadores & Projeções
- **Componente:** Matriz comparativa entre períodos (7D, 1M, 3M) e indexadores econômicos (CDI 112%, Selic 102%, IPCA 88%) com simulador de projeção líquida futura (`R$ 5.000,00 ➔ R$ 6.274,90`).

---

<a id="modulo-4"></a>
## Módulo 4: Operações de Investimento & Resgate

### Páginas 14 & 15: Fluxo de Aplicação / Investir
- **Rota Expo:** `apps/mobile/app/investments/apply.tsx`
- **Componentes Visuais:**
  - Top Bar com botão Voltar e ativo selecionado.
  - Indicador de saldo em conta corrente: `Saldo disponível R$ 18.420,80`.
  - Input monetário com máscara BRL e nota de aplicação mínima (`R$ 100,00`).
  - Quick-Pills de Valores Rápidos: `R$ 500 | R$ 1.000 | R$ 5.000 | Máximo`.
  - Card de Projeção até o Vencimento: Valor aplicado versus estimativa líquida calculada.
  - Card "Pronto para confirmar" com regras de liquidação (`Hoje, até 17h`).
  - CTA Fixo: `Confirmar investimento`.

### Página 16: Fluxo de Resgate de Investimento
- **Rota Expo:** `apps/mobile/app/investments/redeem.tsx`
- **Componentes Visuais:**
  - Indicador de saldo no ativo: `R$ 35.200,00` com disponibilidade `100% · D+0`.
  - Input monetário de resgate com cálculo de saldo remanescente em tempo real.
  - Quick-Pills de Resgate: `R$ 500 | R$ 1.000 | R$ 2.500 | Total`.
  - Detalhamento Fiscal Transparente:
    - Valor Bruto: `R$ 2.500,00`
    - Imposto de Renda Retido: `− R$ 17,50`
  - Card de Resumo: Valor Líquido a creditar (`R$ 2.482,50`) e liquidação (`Hoje · D+0`).
  - CTA Fixo: `Confirmar resgate`.

---

<a id="modulo-5"></a>
## Módulo 5: Mercado Financeiro & Cotações em Tempo Real

### Página 17: Cotações & Tickers do Mercado
- **Rota Expo:** `apps/mobile/app/(tabs)/market.tsx`
- **Componentes Visuais:**
  - Campo de busca com ícone de lupa: *"Buscar ativo, empresa ou fundo"*.
  - Filtros em Carrossel Horizontal: `Ações | FIIs | Cripto | Renda fixa`.
  - Mini Cards de Índices Globais:
    - IBOVESPA: `128.450 (+0,8%)`
    - IFIX: `3.245 (+0,3%)`
    - BITCOIN: `R$ 320 mil (-1,2%)`
  - Card de Destaque Diário (Ex: PETR4 `R$ 36,82 (+2,41%)`) com gráfico interativo de linha.
  - Seção `MAIS NEGOCIADOS · 24H` com link `Ver ranking`:
    - VALE3: `+3,18%`
    - WEGE3: `+2,74%`
    - ITUB4: `+1,42%`

---

<a id="modulo-6"></a>
## Módulo 6: Planejamento Financeiro, Metas & Extrato

### Página 18: Minhas Metas & Objetivos Financeiros
- **Rota Expo:** `apps/mobile/app/goals/index.tsx`
- **Componentes Visuais:**
  - Header com botão de adicionar nova meta (`+`).
  - Card Consolidado: `Progresso Total R$ 68.200 em 3 objetivos` com barra de progresso linear.
  - Cards Individuais de Objetivos com Gráfico Circular de Conclusão:
    - **Reserva de Emergência:** `75% | R$ 15.000 de R$ 20.000 · Dez 2026` + Dica de IA: *"Aporte R$ 500/mês para atingir no prazo."*
    - **Viagem para a Europa:** `33% | R$ 8.200 de R$ 25.000 · Jul 2027` + Dica de IA: *"Economize R$ 200 extras em jantares."*
    - **Aposentadoria:** `9% | R$ 45.000 de R$ 500.000 · Mai 2045` + Dica de IA: *"Aumente os aportes em 2% ao ano para acelerar."*

### Página 19: Extrato Financeiro
- **Rota Expo:** `apps/mobile/app/statement/index.tsx`
- **Componentes Visuais:**
  - Seletor de Mês e Ano com ícone de calendário.
  - Card de Balanço do Mês: `Entradas: R$ 5.200 | Saídas: R$ 1.850 | Saldo Líquido: R$ 3.350`.
  - Lista de Transações Agrupadas por Data:
    - Ícones temáticos para cada categoria (Mercado, Streaming, Transferências, Investimentos).
    - Valores positivos em verde e negativos em branco/cinza.

---

<a id="modulo-7"></a>
## Módulo 7: Notificações, Perfil & Segurança Avançada

### Página 20: Notificações Inteligentes
- **Rota Expo:** `apps/mobile/app/notifications/index.tsx`
- **Componentes Visuais:**
  - Header com contador de novas atualizações e botão de marcar todas como lidas (`double-check`).
  - Filtros: `Todas | Rendimentos | Alertas | Sistema`.
  - Cards com ícones específicos de evento e timestamps relativos (`há 2h`, `ontem`, `3 dias`).

### Página 21: Perfil do Usuário
- **Rota Expo:** `apps/mobile/app/(tabs)/profile.tsx`
- **Componentes Visuais:**
  - Header com botão de configurações (`engrenagem`).
  - Card de Identidade: Avatar, Nome completo, Número da conta e Badge `Rendash Prime | Perfil verificado`.
  - Seção Planejamento: Minhas metas, Extrato, Perfil de investidor (`Moderado`).
  - Seção Conta: Dados pessoais, Segurança e privacidade, Central de ajuda.
  - Versão do app no rodapé: `Rendash para iOS · versão 4.8.0`.

### Página 22: Central de Configurações, Biometria & Privacidade
- **Rota Expo:** `apps/mobile/app/settings/security.tsx`
- **Componentes Visuais:**
  - Indicador circular de nível de segurança: `92% Conta muito segura`.
  - Toggles nativos de iOS (Switch):
    - Acesso por biometria (Touch ID / Face ID)
    - Confirmação biométrica para movimentações acima de R$ 1.000
    - Alertas de rendimentos e de mercado
    - Ocultar saldo ao abrir o aplicativo
  - Acessos a alteração de senha e lista de dispositivos autorizados conectados.

---

<a id="modulo-8"></a>
## Módulo 8: Open Finance Brasil - Conexão, Permissões & Sincronização

### Página 23: Open Finance Hub
- **Rota Expo:** `apps/mobile/app/open-finance/index.tsx`
- **Componentes Visuais:**
  - Card de Saldo Consolidado Multi-bancos: `R$ 186.420,90` (+3,8% no mês).
  - Ações Principais: `Conectar | Sincronizar | Gerenciar`.
  - Lista de Instituições Conectadas com latência e status:
    - Rendash (Conta Principal): `R$ 84.230,50` (Ao vivo)
    - Itaú (Conta Corrente): `R$ 56.740,20` (Atualizado há 2 min)
    - Nubank (Conta + Cartão): `R$ 45.450,20` (Atualizado há 5 min)

### Página 24: Conectar Instituição / Busca Open Finance
- **Rota Expo:** `apps/mobile/app/open-finance/connect.tsx`
- **Componentes Visuais:**
  - Barra de busca de instituições autorizadas pelo Banco Central do Brasil.
  - Grid de Bancos Populares: Nubank, Itaú, Banco do Brasil, C6 Bank.
  - Lista de resultados com botão `Selecionar`: Itaú Unibanco, iti, Inter, InfinitePay.

### Página 25: Consentimento & Permissões LGPD
- **Rota Expo:** `apps/mobile/app/open-finance/permissions.tsx`
- **Componentes Visuais:**
  - Card de identificação da instituição com selo `SEGURO`.
  - Seleção granular de permissões com checkboxes nativos:
    - Contas e saldos
    - Extratos e transações (12 meses)
    - Cartões e faturas
    - Investimentos (apenas consulta)
  - Validade do consentimento: Dropdown `12 meses`.
  - CTA: `➔ Continuar no [Banco]` (redirecionamento OAuth 2.0 mTLS).

### Páginas 26 a 30: Máquina de Estados da Sincronização
- **Página 26 (Sincronizando):** Loader circular com percentual (`80%`) e checklist em tempo real com contagem de transações importadas. Opção *"Continuar em segundo plano"*.
- **Página 27 (Conexão Interrompida):** Estado de falha transitória com CTA de retentativa.
- **Página 28 (Conectado com Sucesso):** Ícone de check esmeralda com saldo consolidado recalculado.
- **Página 29 (Tudo Pronto):** Resumo da importação (contas encontradas, volume de transações e agendamento da sincronização em 6h).
- **Página 30 (Alerta Modal Nativo iOS):** Dialog de erro com código de diagnóstico técnico (`Código OF-408`).

### Página 31: Minhas Contas / Visão Multi-banco
- **Rota Expo:** `apps/mobile/app/open-finance/accounts.tsx`
- **Componentes Visuais:**
  - Barra proporcional colorida demonstrando a distribuição patrimonial por banco: Rendash 62%, Itaú 27%, Nubank 11%.
  - Detalhamento por conta corrente e aplicação de cada instituição financeira.

---

<a id="modulo-9"></a>
## Módulo 9: Visão Empresarial PJ & Fluxo de Caixa

### Página 32: Alternador de Contexto Pessoal vs Empresa (Lumina Studio Ltda.)
- **Rota Expo:** `apps/mobile/app/business/index.tsx`
- **Componentes Visuais:**
  - Segmented Control no topo: `Pessoal | Empresa`.
  - Card PJ: Razão Social, CNPJ (`48.290.118/0001-72`) e Saldo Empresarial (`R$ 286.310,80`).
  - Indicadores de Fluxo Operacional:
    - A Receber: `R$ 48,2 mil` (próximos 7 dias)
    - A Pagar: `R$ 31,8 mil` (12 vencimentos programados)
  - Menu de Gestão PJ: Dashboard empresarial, Cobranças e recebíveis, Sócios e acessos autorizados, Contas conectadas PJ.

### Página 33: Dashboard Empresarial & Fluxo Projetado
- **Rota Expo:** `apps/mobile/app/business/cashflow.tsx`
- **Componentes Visuais:**
  - Gráfico de projeção de fluxo de caixa (30 dias) com barras de volume e linha de tendência.
  - Indicadores de Entradas, Saídas e Rendimentos de caixa.
  - Próximos compromissos: Notas Fiscais emitidas a receber e Folha de Pagamento programada.

---

<a id="modulo-10"></a>
## Módulo 10: Extrato Unificado, Pix Inteligente & Categorias

### Página 34: Extrato Unificado Multi-bancos
- **Rota Expo:** `apps/mobile/app/statement/unified.tsx`
- **Componentes Visuais:**
  - Filtros combinados em pills: `Todos | Contas | Categorias | Pix`.
  - Contador de transações consolidadas (`2.847 movimentações · 3 bancos`).

### Páginas 35 & 37: Detalhe do Pix & Comprovante com IA
- **Rota Expo:** `apps/mobile/app/pix/receipt/[id].tsx`
- **Componentes Visuais:**
  - Avatar do pagador/recebedor e valor em destaque (`R$ 3.200,00`).
  - Metadados completos do Pix: Data, hora, instituição de origem/destino, ID da transação (End-to-End Id) e descrição.
  - Seção de Enriquecimento por IA: Classificação automática do contato e categoria contábil com botão de edição.
  - Ações Rápidas: `Compartilhar | Copiar ID | Mensagem`.

### Página 36: Bottom Sheet de Edição de Classificação
- **Componente:** Bottom Sheet deslizante nativo iOS com busca de categorias e switch inteligente: *"Aplicar 'Serviços' automaticamente aos próximos Pix deste contato"*.

### Página 38: Gestão de Categorias, Orçamento e Automação
- **Rota Expo:** `apps/mobile/app/categories/index.tsx`
- **Componentes Visuais:**
  - Total de despesas mensais (`R$ 12.684,20`).
  - Barras de progresso com porcentagem de consumo do orçamento:
    - Alimentação: 31% (`R$ 3.842`)
    - Moradia: 25% (`R$ 3.180`)
    - Mobilidade: 16% (`R$ 1.986`)
    - Saúde: 10% (`R$ 1.210`)
  - Card de Automação: `18 regras automáticas ativas - 94% das transações classificadas sem intervenção humana`.
  - Botão Flutuante (FAB) verde esmeralda: `+ Nova categoria`.

---

<a id="modulo-11"></a>
## Módulo 11: Cartão Rendash Platinum, Faturas & Controles

### Páginas 39 & 42: Hub do Cartão de Crédito Rendash Platinum
- **Rota Expo:** `apps/mobile/app/cards/index.tsx`
- **Componentes Visuais:**
  - Mockup do Cartão Físico/Virtual com acabamento escuro metálico, chip EMV, logo Visa Platinum e numeração mascarada (`•••• 4821`).
  - Grade de Ações Rápidas do Cartão:
    - `Bloquear` (ícone de trava)
    - `Senha` (ícone de teclado PIN)
    - `Virtual` (gerador de cartão temporário)
    - `Controles` (limites e preferências)
  - Card da Fatura Atual:
    - Valor fechamento: `R$ 4.286,90` com contagem regressiva (*"Fecha em 3 dias"*).
    - Barra de limite: `43% utilizado` (`R$ 5.713,10 disponíveis de R$ 10.000`).
  - CTA Primário: Botão verde esmeralda `Ver fatura`.
  - Banner de Fidelidade: `1.842 pontos Rendash · 2 acessos lounge disponíveis`.

### Páginas 40 & 43: Fatura Detalhada & Parcelamentos
- **Rota Expo:** `apps/mobile/app/cards/invoice.tsx`
- **Componentes Visuais:**
  - Status da fatura em badge: `ABERTA`.
  - Datas de fechamento e vencimento.
  - CTA Primário destacado: `Pagar fatura`.
  - Segmented Control de Filtragem: `Todos | Titular Secundário | Cartão Virtual`.
  - Lista de Compras Agrupadas por Dia com indicador de parcelamento (ex: `Azul Linhas Aéreas - 2/6 parcelas`).

### Páginas 41 & 46: Controles do Cartão & Ajuste de Limite Dinâmico
- **Rota Expo:** `apps/mobile/app/cards/controls.tsx`
- **Componentes Visuais:**
  - Status do cartão: `Cartão desbloqueado - Pronto para uso` com switch rápido.
  - **Slider de Limite Dinâmico com Feedback Tátil:**
    - Slider suave de R$ 500 a R$ 10.000, atualizando o limite instantaneamente via API.
  - Gestão de Cartão Virtual (CVV dinâmico).
  - Toggles de Segurança Avançados:
    - Compras por aproximação (Contactless) com limite de R$ 200 sem senha.
    - Compras internacionais (Ativo em 42 países).
    - Proteção para compras online com confirmação biométrica obrigatória.
  - Card informativo: *"Face ID confirma mudanças sensíveis e dados do cartão"*.
  - Ação de emergência: *"Contestar compra ou informar perda"*.

### Página 44: Modal / Bottom Sheet de Detalhe da Compra
- **Componentes Visuais:**
  - Badge de status: `PAGO`.
  - Metadados: Categoria Viagens, Cartão utilizado, Estabelecimento físico/online, Status do pagamento e parcelamento.
  - Botão de ação: `Ver comprovante`.

### Página 45: Design System de Estados Visuais & Microinterações
- **Componente:** Guia canônico de componentes de status para toda a aplicação:
  - `PAGO` (Verde esmeralda / Teal)
  - `EM PROCESSO` (Azul acinzentado)
  - `BLOQUEADO` (Cinza neutro escuro com ícone de cadeado)
  - `CARREGANDO` (Skeleton animado com gradiente suave)
  - `VAZIO` (Estado neutro com ilustração sutil e ação de início)
  - `ERRO` (Vermelho carmesim suave com ação de retentativa)
