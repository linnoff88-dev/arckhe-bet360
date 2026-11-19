# Arquitetura Tecnica
# Bet360 - Plataforma White-Label de iGaming

**Versao:** 1.0
**Data:** 2026-01-20
**Status:** Draft

---

## 1. Visao Geral da Arquitetura

### 1.1 Principios Arquiteturais

1. **Multi-Tenancy First**: Toda decisao considera isolamento e customizacao por tenant
2. **Compliance by Design**: Requisitos regulatorios sao requisitos de primeira classe
3. **Scalability Horizontal**: Stateless services, auto-scaling
4. **Security in Depth**: Multiplas camadas de protecao
5. **Observability**: Logs, metricas e traces em todos os componentes

### 1.2 Diagrama de Alto Nivel

```
                                    ┌─────────────────────────────────────────────────┐
                                    │                   CLIENTS                        │
                                    ├────────────────┬────────────────┬───────────────┤
                                    │   Web (PWA)    │  Mobile App    │  Admin Panel  │
                                    │   Next.js 14   │  React Native  │   Next.js 14  │
                                    └───────┬────────┴───────┬────────┴───────┬───────┘
                                            │                │                │
                                            ▼                ▼                ▼
┌───────────────────────────────────────────────────────────────────────────────────────┐
│                                    EDGE LAYER                                          │
├───────────────────────────────────────────────────────────────────────────────────────┤
│   CloudFlare (CDN + WAF + DDoS)  │  Rate Limiting  │  Geo Blocking  │  SSL/TLS 1.3   │
└───────────────────────────────────────────────────────────────────────────────────────┘
                                            │
                                            ▼
┌───────────────────────────────────────────────────────────────────────────────────────┐
│                                   API GATEWAY                                          │
├───────────────────────────────────────────────────────────────────────────────────────┤
│   Kong / AWS API Gateway                                                               │
│   - Authentication (JWT validation)                                                    │
│   - Rate Limiting (per tenant, per user)                                              │
│   - Request/Response transformation                                                    │
│   - Routing to microservices                                                          │
│   - API versioning                                                                     │
└───────────────────────────────────────────────────────────────────────────────────────┘
                                            │
                    ┌───────────────────────┼───────────────────────┐
                    ▼                       ▼                       ▼
┌─────────────────────────────────────────────────────────────────────────────────────────┐
│                                  MICROSERVICES LAYER                                     │
├──────────────────┬──────────────────┬──────────────────┬──────────────────┬─────────────┤
│   Auth Service   │  Wallet Service  │   Game Service   │ Payment Service  │ Admin Svc   │
│                  │                  │                  │                  │             │
│  - Registration  │  - Balance mgmt  │  - Game catalog  │  - PIX (Efi)     │  - Dashboard│
│  - Login/Logout  │  - Transactions  │  - Game sessions │  - Tigo Money    │  - Users    │
│  - JWT tokens    │  - Multi-currency│  - Bet placement │  - Crypto        │  - Reports  │
│  - 2FA           │  - Bonus wallet  │  - RNG/Provably  │  - Withdrawals   │  - Config   │
│  - KYC/Biometria │  - Audit trail   │  - RTP control   │  - Reconciliation│  - Logs     │
├──────────────────┼──────────────────┼──────────────────┼──────────────────┼─────────────┤
│  Compliance Svc  │  Notification Svc│   Bonus Service  │  Tenant Service  │ Analytics   │
│                  │                  │                  │                  │             │
│  - AML detection │  - Push notif    │  - Promotions    │  - Onboarding    │  - GGR      │
│  - PEP checking  │  - Email         │  - Bonus rules   │  - Config mgmt   │  - Player   │
│  - SISCOAF       │  - SMS           │  - Wagering      │  - Branding      │  - Cohorts  │
│  - Responsible   │  - In-app        │  - Expiration    │  - Isolation     │  - Funnels  │
└──────────────────┴──────────────────┴──────────────────┴──────────────────┴─────────────┘
                                            │
                                            ▼
┌───────────────────────────────────────────────────────────────────────────────────────┐
│                                    MESSAGE BROKER                                      │
├───────────────────────────────────────────────────────────────────────────────────────┤
│   Redis Streams / RabbitMQ / AWS SQS                                                   │
│   - Event-driven communication between services                                        │
│   - Async processing (withdrawals, notifications, AML)                                │
│   - Dead letter queues for failed messages                                            │
└───────────────────────────────────────────────────────────────────────────────────────┘
                                            │
                                            ▼
┌───────────────────────────────────────────────────────────────────────────────────────┐
│                                     DATA LAYER                                         │
├──────────────────┬──────────────────┬──────────────────┬──────────────────────────────┤
│   PostgreSQL     │      Redis       │     MongoDB      │        ClickHouse            │
│   (Primary DB)   │   (Cache/Queue)  │   (Events/Logs)  │       (Analytics)            │
│                  │                  │                  │                              │
│  - Users         │  - Sessions      │  - Game events   │  - Aggregated metrics        │
│  - Wallets       │  - Rate limits   │  - Audit logs    │  - Player analytics          │
│  - Transactions  │  - Leaderboards  │  - AML alerts    │  - Game statistics           │
│  - Games config  │  - Real-time     │  - Notifications │  - Financial reports         │
│  - Tenants       │  - Pub/Sub       │  - Compliance    │  - Business intelligence     │
└──────────────────┴──────────────────┴──────────────────┴──────────────────────────────┘
```

---

## 2. Stack Tecnologico

### 2.1 Backend

| Componente | Tecnologia | Justificativa |
|------------|------------|---------------|
| Linguagem | TypeScript (Node.js 20 LTS) | Type safety, ecossistema npm, performance |
| Framework | NestJS 10 | Arquitetura modular, DI, decorators, extensivel |
| ORM | Prisma | Type-safe queries, migrations, multi-database |
| Validation | class-validator + Zod | Schema validation, DTO validation |
| API Docs | Swagger/OpenAPI | Auto-geracao de documentacao |

**Alternativa**: Laravel 11 (PHP 8.3) - Se equipe tiver mais experiencia PHP

### 2.2 Frontend

| Componente | Tecnologia | Justificativa |
|------------|------------|---------------|
| Framework | Next.js 14 (App Router) | SSR, RSC, performance, SEO |
| Styling | Tailwind CSS 3 | Utility-first, customizavel, responsivo |
| State | Zustand | Simples, performatico, sem boilerplate |
| Forms | React Hook Form + Zod | Performance, validacao integrada |
| UI Components | shadcn/ui | Acessivel, customizavel, sem vendor lock |
| i18n | next-intl | Internacionalizacao nativa Next.js |

### 2.3 Mobile

| Componente | Tecnologia | Justificativa |
|------------|------------|---------------|
| Framework | React Native 0.73+ | Codigo compartilhado com web |
| Navigation | React Navigation 6 | Padrao da industria |
| Alternativa | Flutter | Se precisar performance nativa superior |

### 2.4 Databases

| Tipo | Tecnologia | Uso |
|------|------------|-----|
| Relacional | PostgreSQL 16 | Dados transacionais, ACID |
| Cache | Redis 7 | Sessions, rate limiting, real-time |
| Document | MongoDB 7 | Logs, eventos, dados nao-estruturados |
| Analytics | ClickHouse | OLAP, metricas agregadas, BI |

### 2.5 Infraestrutura

| Servico | Provider | Regiao |
|---------|----------|--------|
| Cloud | AWS | sa-east-1 (Sao Paulo) |
| Kubernetes | EKS | Auto-scaling, HA |
| CDN | CloudFlare | Global, WAF incluso |
| DNS | CloudFlare / Route53 | Failover DNS |
| Secrets | AWS Secrets Manager | Rotacao automatica |
| Logs | CloudWatch + DataDog | Observabilidade |

---

## 3. Arquitetura Multi-Tenant

### 3.1 Estrategia de Isolamento

```
┌─────────────────────────────────────────────────────────────────────────────────┐
│                           MULTI-TENANT ARCHITECTURE                              │
├─────────────────────────────────────────────────────────────────────────────────┤
│                                                                                  │
│   ┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐            │
│   │   Tenant A      │    │   Tenant B      │    │   Tenant C      │            │
│   │   (Starter)     │    │   (Business)    │    │   (Enterprise)  │            │
│   ├─────────────────┤    ├─────────────────┤    ├─────────────────┤            │
│   │                 │    │                 │    │                 │            │
│   │  Shared DB      │    │  Shared DB      │    │  Dedicated DB   │            │
│   │  Schema: A      │    │  Schema: B      │    │  Instance       │            │
│   │                 │    │                 │    │                 │            │
│   │  Shared Redis   │    │  Shared Redis   │    │  Dedicated Redis│            │
│   │  Prefix: A:*    │    │  Prefix: B:*    │    │  Instance       │            │
│   │                 │    │                 │    │                 │            │
│   └─────────────────┘    └─────────────────┘    └─────────────────┘            │
│                                                                                  │
│   ┌─────────────────────────────────────────────────────────────────────────┐  │
│   │                      SHARED APPLICATION LAYER                            │  │
│   │   ┌───────────────┐  ┌───────────────┐  ┌───────────────┐               │  │
│   │   │  Auth Service │  │ Wallet Service│  │  Game Service │  ...          │  │
│   │   │ (tenant-aware)│  │ (tenant-aware)│  │ (tenant-aware)│               │  │
│   │   └───────────────┘  └───────────────┘  └───────────────┘               │  │
│   └─────────────────────────────────────────────────────────────────────────┘  │
│                                                                                  │
└─────────────────────────────────────────────────────────────────────────────────┘
```

### 3.2 Identificacao de Tenant

O tenant e identificado por:

1. **Subdomain**: `tenant-a.bet360.com.br`
2. **Custom Domain**: `www.operador-a.bet.br`
3. **Header**: `X-Tenant-ID` (para APIs)
4. **JWT Claim**: `tenant_id` no token

```typescript
// middleware/tenant.middleware.ts
@Injectable()
export class TenantMiddleware implements NestMiddleware {
  async use(req: Request, res: Response, next: NextFunction) {
    const tenantId = this.extractTenantId(req);

    if (!tenantId) {
      throw new UnauthorizedException('Tenant not identified');
    }

    const tenant = await this.tenantService.findById(tenantId);

    if (!tenant || !tenant.isActive) {
      throw new ForbiddenException('Tenant not found or inactive');
    }

    req.tenant = tenant;
    next();
  }

  private extractTenantId(req: Request): string | null {
    // 1. Try custom domain mapping
    const host = req.get('host');
    const domainTenant = await this.tenantService.findByDomain(host);
    if (domainTenant) return domainTenant.id;

    // 2. Try subdomain
    const subdomain = host?.split('.')[0];
    if (subdomain && subdomain !== 'www') {
      return subdomain;
    }

    // 3. Try header (for API calls)
    return req.get('X-Tenant-ID') || null;
  }
}
```

### 3.3 Database por Tenant (Prisma)

```typescript
// prisma/schema.prisma
generator client {
  provider        = "prisma-client-js"
  previewFeatures = ["multiSchema"]
}

datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
  schemas  = ["public", "tenant_a", "tenant_b", "tenant_c"]
}

// Ao criar tenant, criar schema dinamicamente
async function createTenantSchema(tenantId: string) {
  await prisma.$executeRawUnsafe(`CREATE SCHEMA IF NOT EXISTS ${tenantId}`);

  // Run migrations for tenant schema
  await prisma.$executeRaw`
    SET search_path TO ${tenantId};
  `;

  // Apply all migrations to new schema
  await runMigrations(tenantId);
}
```

---

## 4. Microservices Detalhados

### 4.1 Auth Service

**Responsabilidades:**
- Cadastro e autenticacao de usuarios
- Emissao e validacao de JWT
- KYC e verificacao biometrica
- 2FA (TOTP, SMS)
- Gerenciamento de sessoes

**Endpoints principais:**

```yaml
POST /auth/register
  - Input: email, password, cpf, phone, birthdate, terms_accepted
  - Output: { user_id, requires_kyc: true }
  - Validacoes: email unico, CPF valido, idade >= 18

POST /auth/login
  - Input: email, password, device_fingerprint
  - Output: { access_token, refresh_token, expires_in }
  - Rate limit: 5 tentativas/15 min

POST /auth/kyc/biometric
  - Input: selfie_image (base64), document_type, document_images[]
  - Output: { status: pending|approved|rejected, kyc_id }
  - Integracao: Legitimuz API

POST /auth/refresh
  - Input: refresh_token
  - Output: { access_token, refresh_token }

POST /auth/2fa/enable
  - Input: type (totp|sms), verification_code
  - Output: { secret?, backup_codes[] }

GET /auth/session/validate
  - Headers: Authorization: Bearer {token}
  - Output: { valid: boolean, user_id, tenant_id, permissions[] }
```

**Modelo de dados:**

```sql
-- users table
CREATE TABLE users (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  tenant_id UUID NOT NULL REFERENCES tenants(id),
  email VARCHAR(255) NOT NULL,
  password_hash VARCHAR(255) NOT NULL,
  cpf VARCHAR(11) UNIQUE,
  phone VARCHAR(20),
  birthdate DATE NOT NULL,
  status VARCHAR(20) DEFAULT 'pending_kyc',
  kyc_status VARCHAR(20) DEFAULT 'pending',
  kyc_verified_at TIMESTAMP,
  risk_level VARCHAR(20) DEFAULT 'low',
  is_pep BOOLEAN DEFAULT false,
  two_factor_enabled BOOLEAN DEFAULT false,
  two_factor_secret VARCHAR(255),
  last_login_at TIMESTAMP,
  last_biometric_at TIMESTAMP,
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW(),

  UNIQUE(tenant_id, email),
  CHECK (birthdate <= CURRENT_DATE - INTERVAL '18 years')
);

-- sessions table
CREATE TABLE sessions (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID NOT NULL REFERENCES users(id),
  token_hash VARCHAR(255) NOT NULL,
  device_fingerprint VARCHAR(255),
  ip_address INET,
  user_agent TEXT,
  geolocation JSONB,
  expires_at TIMESTAMP NOT NULL,
  created_at TIMESTAMP DEFAULT NOW()
);

-- kyc_verifications table
CREATE TABLE kyc_verifications (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID NOT NULL REFERENCES users(id),
  provider VARCHAR(50) NOT NULL,
  provider_reference VARCHAR(255),
  document_type VARCHAR(50),
  selfie_match_score DECIMAL(5,2),
  document_verified BOOLEAN,
  liveness_verified BOOLEAN,
  status VARCHAR(20) NOT NULL,
  rejection_reason TEXT,
  raw_response JSONB,
  created_at TIMESTAMP DEFAULT NOW()
);
```

### 4.2 Wallet Service

**Responsabilidades:**
- Gerenciamento de saldos
- Transacoes (deposito, saque, aposta, premio)
- Multi-moeda com conversao
- Bonus wallet separado
- Audit trail completo

**Endpoints principais:**

```yaml
GET /wallet/balance
  - Output: {
      balances: [
        { currency: 'BRL', available: 1000.00, bonus: 50.00, locked: 0 }
      ],
      total_brl: 1050.00
    }

POST /wallet/deposit
  - Input: { amount, currency, payment_method, payment_data }
  - Output: { transaction_id, payment_url?, qr_code?, status }

POST /wallet/withdraw
  - Input: { amount, currency, bank_account }
  - Output: { transaction_id, status, estimated_time }
  - Validacoes: KYC completo, saldo suficiente, rollover bonus

GET /wallet/transactions
  - Query: ?type=&status=&from=&to=&page=&limit=
  - Output: { transactions[], total, page, pages }

POST /wallet/convert
  - Input: { from_currency, to_currency, amount }
  - Output: { converted_amount, exchange_rate, fee }
```

**Modelo de dados:**

```sql
-- wallets table
CREATE TABLE wallets (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID NOT NULL REFERENCES users(id),
  currency VARCHAR(10) NOT NULL,
  balance DECIMAL(18,8) NOT NULL DEFAULT 0,
  bonus_balance DECIMAL(18,8) NOT NULL DEFAULT 0,
  locked_balance DECIMAL(18,8) NOT NULL DEFAULT 0,
  created_at TIMESTAMP DEFAULT NOW(),
  updated_at TIMESTAMP DEFAULT NOW(),

  UNIQUE(user_id, currency),
  CHECK (balance >= 0),
  CHECK (bonus_balance >= 0)
);

-- transactions table
CREATE TABLE transactions (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  wallet_id UUID NOT NULL REFERENCES wallets(id),
  type VARCHAR(20) NOT NULL, -- deposit, withdraw, bet, win, bonus, transfer
  amount DECIMAL(18,8) NOT NULL,
  balance_before DECIMAL(18,8) NOT NULL,
  balance_after DECIMAL(18,8) NOT NULL,
  currency VARCHAR(10) NOT NULL,
  reference_type VARCHAR(50), -- payment, game_round, bonus_id
  reference_id UUID,
  status VARCHAR(20) NOT NULL DEFAULT 'pending',
  metadata JSONB,
  created_at TIMESTAMP DEFAULT NOW(),
  completed_at TIMESTAMP
);

-- Create index for fast balance lookups
CREATE INDEX idx_transactions_wallet_created ON transactions(wallet_id, created_at DESC);
```

### 4.3 Game Service

**Responsabilidades:**
- Catalogo de jogos
- Sessoes de jogo
- RNG (Provably Fair)
- Processamento de apostas
- Calculo de premios

**Endpoints principais:**

```yaml
GET /games
  - Query: ?category=&search=&page=&limit=
  - Output: { games[], total }

GET /games/:id
  - Output: { id, name, category, rtp, min_bet, max_bet, thumbnail, demo_url }

POST /games/:id/session
  - Input: { mode: 'demo'|'real', currency }
  - Output: { session_id, game_url, client_seed }

POST /games/:id/bet
  - Input: { session_id, amount, bet_data, client_seed? }
  - Output: { round_id, result, payout, server_seed_hash }

POST /games/:id/verify
  - Input: { round_id }
  - Output: { server_seed, client_seed, nonce, result_proof }
```

**Provably Fair Implementation:**

```typescript
// services/provably-fair.service.ts
import { createHash, randomBytes } from 'crypto';

@Injectable()
export class ProvablyFairService {

  // Gerar server seed antes da rodada
  generateServerSeed(): { seed: string; hash: string } {
    const seed = randomBytes(32).toString('hex');
    const hash = this.hashSeed(seed);
    return { seed, hash };
  }

  // Hash do seed (exposto ao jogador antes da aposta)
  hashSeed(seed: string): string {
    return createHash('sha256').update(seed).digest('hex');
  }

  // Combinar seeds para gerar resultado
  generateResult(
    serverSeed: string,
    clientSeed: string,
    nonce: number
  ): string {
    const combined = `${serverSeed}:${clientSeed}:${nonce}`;
    return createHash('sha256').update(combined).digest('hex');
  }

  // Derivar numero do hash (0-1)
  hashToNumber(hash: string): number {
    // Usar primeiros 8 caracteres (32 bits)
    const hex = hash.slice(0, 8);
    const int = parseInt(hex, 16);
    return int / 0xFFFFFFFF; // Normalizar para 0-1
  }

  // Verificar resultado
  verifyResult(
    serverSeed: string,
    serverSeedHash: string,
    clientSeed: string,
    nonce: number,
    expectedResult: string
  ): boolean {
    // Verificar que o hash bate com o seed revelado
    if (this.hashSeed(serverSeed) !== serverSeedHash) {
      return false;
    }

    // Verificar que o resultado e consistente
    const actualResult = this.generateResult(serverSeed, clientSeed, nonce);
    return actualResult === expectedResult;
  }
}
```

**Modelo de dados:**

```sql
-- games table
CREATE TABLE games (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  tenant_id UUID REFERENCES tenants(id), -- NULL = available to all
  name VARCHAR(100) NOT NULL,
  slug VARCHAR(100) NOT NULL UNIQUE,
  category VARCHAR(50) NOT NULL,
  provider VARCHAR(50) NOT NULL, -- 'internal', 'open-source', etc
  rtp DECIMAL(5,2) NOT NULL,
  house_edge DECIMAL(5,2) NOT NULL,
  min_bet DECIMAL(18,2) NOT NULL,
  max_bet DECIMAL(18,2) NOT NULL,
  thumbnail_url TEXT,
  game_url TEXT NOT NULL,
  is_active BOOLEAN DEFAULT true,
  is_demo_available BOOLEAN DEFAULT true,
  metadata JSONB,
  created_at TIMESTAMP DEFAULT NOW()
);

-- game_sessions table
CREATE TABLE game_sessions (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID REFERENCES users(id), -- NULL for demo
  game_id UUID NOT NULL REFERENCES games(id),
  mode VARCHAR(10) NOT NULL, -- 'demo', 'real'
  currency VARCHAR(10),
  client_seed VARCHAR(64),
  server_seed VARCHAR(64),
  server_seed_hash VARCHAR(64) NOT NULL,
  nonce INTEGER DEFAULT 0,
  status VARCHAR(20) DEFAULT 'active',
  created_at TIMESTAMP DEFAULT NOW(),
  ended_at TIMESTAMP
);

-- game_rounds table
CREATE TABLE game_rounds (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  session_id UUID NOT NULL REFERENCES game_sessions(id),
  bet_amount DECIMAL(18,8) NOT NULL,
  bet_data JSONB NOT NULL,
  result_hash VARCHAR(64) NOT NULL,
  result_data JSONB NOT NULL,
  payout DECIMAL(18,8) NOT NULL DEFAULT 0,
  nonce INTEGER NOT NULL,
  created_at TIMESTAMP DEFAULT NOW()
);
```

### 4.4 Payment Service

**Responsabilidades:**
- Integracao com gateways de pagamento
- Processamento de depositos
- Processamento de saques
- Conciliacao financeira
- Webhooks de confirmacao

**Integracoes:**

```typescript
// adapters/payment-adapter.interface.ts
interface PaymentAdapter {
  createDeposit(data: DepositRequest): Promise<DepositResponse>;
  getDepositStatus(transactionId: string): Promise<TransactionStatus>;
  createWithdrawal(data: WithdrawalRequest): Promise<WithdrawalResponse>;
  handleWebhook(payload: unknown): Promise<WebhookResult>;
}

// adapters/efi-bank.adapter.ts (PIX)
@Injectable()
export class EfiBankAdapter implements PaymentAdapter {
  private client: EfiPay;

  async createDeposit(data: DepositRequest): Promise<DepositResponse> {
    // Criar cobranca PIX
    const charge = await this.client.pixCreateImmediateCharge({
      calendario: { expiracao: 3600 }, // 1 hora
      valor: { original: data.amount.toFixed(2) },
      chave: this.config.pixKey,
      infoAdicionais: [
        { nome: 'user_id', valor: data.userId },
        { nome: 'transaction_id', valor: data.transactionId }
      ]
    });

    // Gerar QR Code
    const qrCode = await this.client.pixGenerateQRCode({
      id: charge.loc.id
    });

    return {
      transactionId: data.transactionId,
      externalId: charge.txid,
      qrCode: qrCode.imagemQrcode,
      qrCodeText: qrCode.qrcode,
      expiresAt: new Date(Date.now() + 3600000)
    };
  }

  async handleWebhook(payload: EfiWebhook): Promise<WebhookResult> {
    // Validar assinatura do webhook
    if (!this.validateSignature(payload)) {
      throw new UnauthorizedException('Invalid webhook signature');
    }

    // Encontrar transacao
    const transaction = await this.findByExternalId(payload.pix[0].txid);

    if (payload.pix[0].status === 'CONCLUIDA') {
      return {
        transactionId: transaction.id,
        status: 'completed',
        amount: parseFloat(payload.pix[0].valor),
        paidAt: new Date(payload.pix[0].horario)
      };
    }

    return { transactionId: transaction.id, status: 'pending' };
  }
}

// adapters/tigo-money.adapter.ts (Paraguai)
@Injectable()
export class TigoMoneyAdapter implements PaymentAdapter {
  // Implementacao similar para Tigo Money
}
```

### 4.5 Compliance Service

**Responsabilidades:**
- Deteccao de padroes suspeitos (AML)
- Verificacao de PEP
- Relatorios COAF/SISCOAF
- Jogo Responsavel (limites, autoexclusao)
- Geolocalizacao

**Regras AML:**

```typescript
// rules/aml-rules.ts
export const AML_RULES: AMLRule[] = [
  {
    id: 'rapid-deposit-withdraw',
    name: 'Deposito e saque rapido',
    description: 'Deposito seguido de saque sem atividade de jogo',
    check: async (userId, context) => {
      const recentDeposit = await getRecentDeposit(userId, 30); // 30 min
      const recentWithdraw = await getRecentWithdraw(userId, 30);
      const betVolume = await getBetVolume(userId, 30);

      if (recentDeposit && recentWithdraw && betVolume < recentDeposit.amount * 0.1) {
        return {
          triggered: true,
          severity: 'high',
          details: { depositAmount: recentDeposit.amount, betVolume }
        };
      }
      return { triggered: false };
    }
  },
  {
    id: 'structuring',
    name: 'Estruturacao (Smurfing)',
    description: 'Multiplos depositos pequenos para evitar limites',
    check: async (userId, context) => {
      const deposits24h = await getDeposits(userId, 24 * 60);
      const totalAmount = deposits24h.reduce((sum, d) => sum + d.amount, 0);

      if (deposits24h.length >= 5 && totalAmount > 10000) {
        return {
          triggered: true,
          severity: 'medium',
          details: { depositCount: deposits24h.length, totalAmount }
        };
      }
      return { triggered: false };
    }
  },
  {
    id: 'unusual-betting-pattern',
    name: 'Padrao de aposta incomum',
    description: 'Apostas em lados opostos ou padroes suspeitos',
    check: async (userId, context) => {
      // Implementar deteccao de apostas suspeitas
    }
  }
];
```

**Jogo Responsavel:**

```typescript
// services/responsible-gaming.service.ts
@Injectable()
export class ResponsibleGamingService {

  async checkLimits(userId: string, amount: number, type: 'deposit' | 'loss'): Promise<LimitCheckResult> {
    const limits = await this.getUserLimits(userId);
    const usage = await this.getLimitUsage(userId, type);

    const results = [];

    // Check diario
    if (limits.daily && usage.daily + amount > limits.daily) {
      results.push({ period: 'daily', exceeded: true, remaining: limits.daily - usage.daily });
    }

    // Check semanal
    if (limits.weekly && usage.weekly + amount > limits.weekly) {
      results.push({ period: 'weekly', exceeded: true, remaining: limits.weekly - usage.weekly });
    }

    // Check mensal
    if (limits.monthly && usage.monthly + amount > limits.monthly) {
      results.push({ period: 'monthly', exceeded: true, remaining: limits.monthly - usage.monthly });
    }

    return {
      allowed: results.every(r => !r.exceeded),
      limits: results
    };
  }

  async setLimit(userId: string, type: string, period: string, amount: number): Promise<void> {
    const currentLimit = await this.getCurrentLimit(userId, type, period);

    // Reducao e imediata
    if (amount < currentLimit) {
      await this.updateLimit(userId, type, period, amount, new Date());
    } else {
      // Aumento requer 24h de espera
      await this.scheduleLimit(userId, type, period, amount, new Date(Date.now() + 24 * 60 * 60 * 1000));
    }
  }

  async selfExclude(userId: string, duration: string): Promise<void> {
    const endDate = this.calculateEndDate(duration);

    await this.createExclusion(userId, endDate);

    // Bloquear conta imediatamente
    await this.blockUser(userId);

    // Cancelar todas as comunicacoes promocionais
    await this.unsubscribePromotions(userId);

    // Reportar para plataforma centralizada (gov.br)
    if (this.config.reportToCentralPlatform) {
      await this.reportToGovPlatform(userId, endDate);
    }
  }
}
```

---

## 5. Seguranca

### 5.1 Autenticacao e Autorizacao

```
┌─────────────────────────────────────────────────────────────────────────────────┐
│                              SECURITY ARCHITECTURE                               │
├─────────────────────────────────────────────────────────────────────────────────┤
│                                                                                  │
│   Request Flow:                                                                  │
│                                                                                  │
│   Client ──► CloudFlare WAF ──► API Gateway ──► Service                         │
│              (DDoS, Rules)      (JWT, Rate)     (RBAC)                          │
│                                                                                  │
│   ┌─────────────────────────────────────────────────────────────────────────┐  │
│   │  JWT Token Structure                                                     │  │
│   │  {                                                                       │  │
│   │    "sub": "user_id",                                                     │  │
│   │    "tenant_id": "tenant_uuid",                                          │  │
│   │    "roles": ["player"],                                                  │  │
│   │    "permissions": ["bet:create", "wallet:read"],                        │  │
│   │    "kyc_status": "verified",                                             │  │
│   │    "iat": 1234567890,                                                    │  │
│   │    "exp": 1234657890                                                     │  │
│   │  }                                                                       │  │
│   └─────────────────────────────────────────────────────────────────────────┘  │
│                                                                                  │
│   Roles:                                                                         │
│   - player: Apostador comum                                                      │
│   - vip: Jogador VIP com beneficios                                             │
│   - support: Atendimento ao cliente                                             │
│   - manager: Gestor da operacao                                                 │
│   - admin: Administrador total                                                  │
│   - super_admin: Acesso cross-tenant (apenas equipe Bet360)                    │
│                                                                                  │
└─────────────────────────────────────────────────────────────────────────────────┘
```

### 5.2 Rate Limiting

```typescript
// Configuracao de rate limiting por tipo de endpoint
const rateLimits = {
  auth: {
    login: { points: 5, duration: 900 },      // 5 tentativas / 15 min
    register: { points: 3, duration: 3600 },   // 3 cadastros / hora
    passwordReset: { points: 3, duration: 3600 }
  },
  wallet: {
    deposit: { points: 10, duration: 60 },     // 10 depositos / min
    withdraw: { points: 5, duration: 60 }      // 5 saques / min
  },
  game: {
    bet: { points: 100, duration: 60 },        // 100 apostas / min
    session: { points: 10, duration: 60 }      // 10 sessoes / min
  }
};
```

### 5.3 Criptografia

| Dado | Metodo | Detalhes |
|------|--------|----------|
| Senhas | Argon2id | Cost factor 3, memory 64MB |
| PII em repouso | AES-256-GCM | Key rotation mensal |
| Transito | TLS 1.3 | HSTS habilitado |
| Seeds (Provably Fair) | SHA-256 | Deterministico |

### 5.4 Auditoria

```sql
-- audit_logs table (imutavel)
CREATE TABLE audit_logs (
  id BIGSERIAL PRIMARY KEY,
  tenant_id UUID NOT NULL,
  actor_id UUID,
  actor_type VARCHAR(20), -- 'user', 'admin', 'system'
  action VARCHAR(100) NOT NULL,
  resource_type VARCHAR(50),
  resource_id UUID,
  old_value JSONB,
  new_value JSONB,
  ip_address INET,
  user_agent TEXT,
  created_at TIMESTAMP DEFAULT NOW()
);

-- Particionar por mes para performance
CREATE TABLE audit_logs_y2026m01 PARTITION OF audit_logs
FOR VALUES FROM ('2026-01-01') TO ('2026-02-01');
```

---

## 6. Observabilidade

### 6.1 Stack de Observabilidade

```
┌─────────────────────────────────────────────────────────────────────────────────┐
│                           OBSERVABILITY STACK                                    │
├─────────────────────────────────────────────────────────────────────────────────┤
│                                                                                  │
│   ┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐            │
│   │     LOGS        │    │    METRICS      │    │    TRACES       │            │
│   │                 │    │                 │    │                 │            │
│   │  - Application  │    │  - Business KPIs│    │  - Request flow │            │
│   │  - Access       │    │  - Technical    │    │  - Latency      │            │
│   │  - Audit        │    │  - SLIs/SLOs    │    │  - Errors       │            │
│   │                 │    │                 │    │                 │            │
│   └────────┬────────┘    └────────┬────────┘    └────────┬────────┘            │
│            │                      │                      │                      │
│            ▼                      ▼                      ▼                      │
│   ┌─────────────────────────────────────────────────────────────────────────┐  │
│   │                          DataDog / Grafana                               │  │
│   │   - Dashboards   - Alerts   - Anomaly Detection   - APM                 │  │
│   └─────────────────────────────────────────────────────────────────────────┘  │
│                                                                                  │
└─────────────────────────────────────────────────────────────────────────────────┘
```

### 6.2 Metricas de Negocio

```typescript
// Metricas customizadas para iGaming
const businessMetrics = {
  // Revenue
  'ggr_total': { type: 'counter', labels: ['tenant', 'game', 'currency'] },
  'deposits_total': { type: 'counter', labels: ['tenant', 'method', 'currency'] },
  'withdrawals_total': { type: 'counter', labels: ['tenant', 'method', 'currency'] },

  // Users
  'active_users': { type: 'gauge', labels: ['tenant'] },
  'new_registrations': { type: 'counter', labels: ['tenant'] },
  'kyc_conversions': { type: 'counter', labels: ['tenant', 'status'] },

  // Games
  'bets_total': { type: 'counter', labels: ['tenant', 'game'] },
  'bet_amount': { type: 'histogram', labels: ['tenant', 'game'] },
  'rtp_actual': { type: 'gauge', labels: ['tenant', 'game'] },

  // Compliance
  'aml_alerts': { type: 'counter', labels: ['tenant', 'rule', 'severity'] },
  'self_exclusions': { type: 'counter', labels: ['tenant', 'duration'] }
};
```

---

## 7. Deploy e CI/CD

### 7.1 Pipeline

```yaml
# .github/workflows/deploy.yml
name: Deploy Pipeline

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
      - run: npm ci
      - run: npm run lint
      - run: npm run test:unit
      - run: npm run test:integration
      - run: npm run test:e2e

  security:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Run Snyk
        uses: snyk/actions/node@master
      - name: Run SAST
        uses: github/codeql-action/analyze@v2

  build:
    needs: [test, security]
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Build Docker images
        run: |
          docker build -t bet360/api:${{ github.sha }} ./api
          docker build -t bet360/web:${{ github.sha }} ./web
      - name: Push to ECR
        run: |
          aws ecr get-login-password | docker login --username AWS --password-stdin $ECR_REGISTRY
          docker push bet360/api:${{ github.sha }}
          docker push bet360/web:${{ github.sha }}

  deploy-staging:
    needs: build
    if: github.ref == 'refs/heads/develop'
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to Staging
        run: |
          kubectl set image deployment/api api=bet360/api:${{ github.sha }} -n staging
          kubectl set image deployment/web web=bet360/web:${{ github.sha }} -n staging

  deploy-production:
    needs: build
    if: github.ref == 'refs/heads/main'
    runs-on: ubuntu-latest
    environment: production
    steps:
      - name: Deploy to Production (Blue-Green)
        run: |
          # Deploy to green environment
          kubectl set image deployment/api-green api=bet360/api:${{ github.sha }} -n production
          # Run smoke tests
          ./scripts/smoke-tests.sh
          # Switch traffic
          kubectl patch service api -p '{"spec":{"selector":{"version":"green"}}}' -n production
```

### 7.2 Estrutura de Ambientes

```
Environments:
├── development (local)
│   └── docker-compose.yml
├── staging (AWS)
│   ├── Namespace: staging
│   ├── 1 replica each service
│   └── Shared RDS (separate schema)
└── production (AWS)
    ├── Namespace: production
    ├── 3+ replicas (auto-scaling)
    ├── Multi-AZ RDS
    └── Read replicas for analytics
```

---

## 8. Estrutura de Diretorios

```
bet360/
├── apps/
│   ├── api/                    # Backend NestJS
│   │   ├── src/
│   │   │   ├── modules/
│   │   │   │   ├── auth/
│   │   │   │   ├── wallet/
│   │   │   │   ├── game/
│   │   │   │   ├── payment/
│   │   │   │   ├── compliance/
│   │   │   │   ├── admin/
│   │   │   │   └── tenant/
│   │   │   ├── common/
│   │   │   │   ├── decorators/
│   │   │   │   ├── filters/
│   │   │   │   ├── guards/
│   │   │   │   ├── interceptors/
│   │   │   │   └── pipes/
│   │   │   ├── config/
│   │   │   └── main.ts
│   │   ├── prisma/
│   │   │   ├── schema.prisma
│   │   │   └── migrations/
│   │   ├── test/
│   │   └── package.json
│   │
│   ├── web/                    # Frontend Next.js (Player)
│   │   ├── src/
│   │   │   ├── app/
│   │   │   ├── components/
│   │   │   ├── hooks/
│   │   │   ├── lib/
│   │   │   └── styles/
│   │   └── package.json
│   │
│   ├── admin/                  # Frontend Next.js (Admin)
│   │   └── ...
│   │
│   └── mobile/                 # React Native
│       └── ...
│
├── packages/
│   ├── ui/                     # Componentes compartilhados
│   ├── utils/                  # Utilitarios compartilhados
│   ├── types/                  # TypeScript types
│   └── games/                  # Game engines
│       ├── crash/
│       ├── mines/
│       ├── plinko/
│       ├── dice/
│       └── slots/
│
├── infra/
│   ├── terraform/              # IaC
│   ├── kubernetes/             # K8s manifests
│   └── docker/                 # Dockerfiles
│
├── doc/                        # Documentacao
│   ├── PRD.md
│   ├── architecture/
│   ├── compliance/
│   └── research/
│
├── scripts/                    # Scripts de automacao
├── .github/                    # GitHub Actions
├── docker-compose.yml          # Dev environment
├── turbo.json                  # Turborepo config
└── package.json                # Root package
```

---

## 9. Referencias

### Documentacao Oficial
- [NestJS Documentation](https://docs.nestjs.com/)
- [Next.js Documentation](https://nextjs.org/docs)
- [Prisma Documentation](https://www.prisma.io/docs)
- [AWS Well-Architected Framework](https://aws.amazon.com/architecture/well-architected/)

### Regulamentacao
- [Lei 14.790/2023](https://www.planalto.gov.br/ccivil_03/_ato2023-2026/2023/lei/l14790.htm)
- [Portaria SPA/MF 722/2024](https://www.legisweb.com.br/legislacao/?id=458565)
- [GLI-19 Standard](https://gaminglabs.com/gli-standards/)

### Repositorios de Referencia
- [johakr/html5-slot-machine](https://github.com/johakr/html5-slot-machine)
- [LaChance-Lab/Multi-Chain-Casino-Games](https://github.com/LaChance-Lab/Multi-Chain-Casino-Games)
- [0xalberto/solana-crash-game](https://github.com/0xalberto/solana-crash-game)
- [AnsonH/plinko-game](https://github.com/AnsonH/plinko-game)

---

## Changelog

| Versao | Data | Autor | Mudancas |
|--------|------|-------|----------|
| 1.0 | 2026-01-20 | BMad Orchestrator | Versao inicial |
