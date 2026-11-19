# Bet360 - Plataforma White-Label de iGaming

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="48" />
  <p><strong>Arckhé AI</strong></p>
</div>


> Solucao completa para operadores licenciados de apostas esportivas e jogos online no Brasil e America Latina.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](http://makeapullrequest.com)

---

## Sobre o Projeto

O **Bet360** e uma plataforma White-Label B2B de iGaming que fornece tecnologia completa para operadores licenciados operarem sua propria marca de apostas. O produto foi projetado desde o inicio para atender aos requisitos regulatorios da Lei 14.790/2023 (Brasil) e regulamentacoes internacionais.

### Modelo de Negocio

| Aspecto | Descricao |
|---------|-----------|
| **Tipo** | White-Label SaaS B2B |
| **Clientes** | Operadores com licenca SPA/MF (Brasil), CONAJZAR (Paraguai) |
| **Receita** | Taxa de setup + Mensalidade + Revenue share (GGR) |
| **Diferencial** | Jogos proprios (sem royalties) + Compliance nativo |

### Mercados-Alvo

- **Brasil**: 183+ operadores licenciados (Jan 2026)
- **Paraguai**: Mercado em abertura (Lei 7438/2025)
- **Outros LATAM**: Expansao futura

---

## Documentacao

| Documento | Descricao |
|-----------|-----------|
| [PRD.md](doc/PRD.md) | Product Requirements Document - Requisitos funcionais e nao-funcionais |
| [ARCHITECTURE.md](doc/architecture/ARCHITECTURE.md) | Arquitetura tecnica detalhada |
| [BRAZIL_COMPLIANCE.md](doc/compliance/BRAZIL_COMPLIANCE.md) | Requisitos regulatorios Brasil (Lei 14.790/2023) |
| [OPEN_SOURCE_REPOS_ANALYSIS.md](doc/research/OPEN_SOURCE_REPOS_ANALYSIS.md) | Analise de repositorios open source para jogos |

---

## Funcionalidades

### Modulo de Autenticacao e KYC
- Cadastro com validacao de CPF
- Biometria facial obrigatoria
- Verificacao de documentos (CNH/RG)
- Revalidacao periodica (7 dias)
- Deteccao de PEPs
- 2FA (TOTP/SMS)

### Modulo de Carteira (Wallet)
- Multi-moeda: BRL, PYG, USD, USDT
- Depositos via PIX (instantaneo)
- Depositos via Tigo Money (Paraguai)
- Saques em ate 120 minutos (lei)
- Bonus wallet separado
- Conversao automatica de moedas

### Modulo de Jogos
- **Crash/Aviator**: Jogo de multiplicador crescente
- **Mines**: Campo minado com apostas
- **Plinko**: Jogo de fisica com multiplicadores
- **Dice**: Dados provably fair
- **Slots**: Caca-niqueis HTML5 customizaveis
- **Blackjack**: Cartas classico
- **Roulette**: Roleta europeia

Todos os jogos implementam **Provably Fair** para garantir transparencia e verificabilidade.

### Modulo de Jogo Responsavel
- Limites de deposito (diario/semanal/mensal)
- Limites de perda
- Limites de tempo de sessao
- Autoexclusao temporaria e permanente
- Reality check (alertas periodicos)
- Integracao com Plataforma Centralizada (gov.br)

### Modulo de Compliance (AML)
- Classificacao de risco de usuarios
- Deteccao de padroes suspeitos
- Verificacao de PEPs
- Relatorios automaticos COAF/SISCOAF
- Logs de auditoria (5 anos)

### Painel Administrativo
- Dashboard com metricas de GGR
- Gestao de usuarios
- Gestao de jogos (RTP configuravel)
- Aprovacao de saques
- Relatorios de compliance
- Configuracao white-label (branding)

### Multi-Tenancy
- Isolamento de dados por operador
- Database-per-tenant (enterprise)
- Customizacao de marca (logo, cores, dominio)
- Onboarding automatizado

---

## Stack Tecnologico

### Backend
- **Runtime**: Node.js 20 LTS
- **Framework**: NestJS 10
- **ORM**: Prisma
- **Linguagem**: TypeScript

### Frontend
- **Framework**: Next.js 14 (App Router)
- **Styling**: Tailwind CSS 3
- **State**: Zustand
- **Components**: shadcn/ui

### Databases
- **Relacional**: PostgreSQL 16
- **Cache**: Redis 7
- **Logs**: MongoDB 7
- **Analytics**: ClickHouse

### Infraestrutura
- **Cloud**: AWS (sa-east-1 - Sao Paulo)
- **Container**: Kubernetes (EKS)
- **CDN**: CloudFlare
- **CI/CD**: GitHub Actions

---

## Estrutura do Projeto

```
bet360/
â”œâ”€â”€ apps/
â”‚   â”œâ”€â”€ api/                    # Backend NestJS
â”‚   â”œâ”€â”€ web/                    # Frontend Next.js (Player)
â”‚   â”œâ”€â”€ admin/                  # Frontend Next.js (Admin)
â”‚   â””â”€â”€ mobile/                 # React Native
â”œâ”€â”€ packages/
â”‚   â”œâ”€â”€ ui/                     # Componentes compartilhados
â”‚   â”œâ”€â”€ utils/                  # Utilitarios
â”‚   â”œâ”€â”€ types/                  # TypeScript types
â”‚   â””â”€â”€ games/                  # Game engines
â”‚       â”œâ”€â”€ crash/
â”‚       â”œâ”€â”€ mines/
â”‚       â”œâ”€â”€ plinko/
â”‚       â”œâ”€â”€ dice/
â”‚       â””â”€â”€ slots/
â”œâ”€â”€ infra/
â”‚   â”œâ”€â”€ terraform/              # IaC
â”‚   â”œâ”€â”€ kubernetes/             # K8s manifests
â”‚   â””â”€â”€ docker/                 # Dockerfiles
â”œâ”€â”€ doc/                        # Documentacao
â”‚   â”œâ”€â”€ PRD.md
â”‚   â”œâ”€â”€ architecture/
â”‚   â”œâ”€â”€ compliance/
â”‚   â””â”€â”€ research/
â”œâ”€â”€ scripts/                    # Scripts de automacao
â””â”€â”€ docker-compose.yml          # Dev environment
```

---

## Requisitos de Certificacao

Para uso em producao regulada, a plataforma deve ser certificada por laboratorio reconhecido pela SPA/MF:

- [GLI (Gaming Laboratories International)](https://gaminglabs.com/)
- [BMM Testlabs](https://bmm.com/)
- [iTech Labs](https://itechlabs.com/)

Certificacoes necessarias:
- RNG (Random Number Generator)
- RTP (Return to Player)
- Seguranca da plataforma
- Integracao de pagamentos

---

## Fases de Desenvolvimento

### Fase 1: MVP
- Autenticacao + KYC basico
- Carteira BRL + PIX
- 3 jogos: Crash, Mines, Dice
- Admin basico
- 1 tenant demo

### Fase 2: Compliance Completo
- KYC completo (biometria, PEP, AML)
- Jogo Responsavel (todos os limites)
- Integracao SISCOAF
- +2 jogos: Plinko, Slot

### Fase 3: Multi-Jurisdicao
- Multi-moeda (BRL, PYG, USD, USDT)
- Pagamentos Paraguai (Tigo Money)
- Multi-idioma (PT-BR, ES)
- +3 jogos

### Fase 4: Enterprise
- Certificacao GLI/BMM
- Sports betting module
- Mobile apps
- Analytics avancado

---

## Integradores de Terceiros

### Pagamentos
| Provedor | Metodo | Pais |
|----------|--------|------|
| [Efi Bank](https://efipay.bet/) | PIX | Brasil |
| [Zro Bank](https://zrobank.com.br/) | PIX | Brasil |
| [Tigo Money](https://www.tigomoney.com/) | Wallet | Paraguai |
| Binance Pay | Crypto | Global |

### KYC/Biometria
| Provedor | Servico |
|----------|---------|
| [Legitimuz](https://legitimuz.com/) | Biometria + Documentos |
| [Veriff](https://www.veriff.com/) | Alternativa |
| [Onfido](https://onfido.com/) | Alternativa |

### Compliance
| Sistema | Descricao |
|---------|-----------|
| SISCOAF | Relatorios AML Brasil |
| Plataforma Autoexclusao | Gov.br |

---

## Repositorios de Referencia

Repositorios open source usados como base para os jogos:

| Jogo | Repositorio | Licenca |
|------|-------------|---------|
| Slots | [johakr/html5-slot-machine](https://github.com/johakr/html5-slot-machine) | MIT |
| Multi-Games | [LaChance-Lab/Multi-Chain-Casino-Games](https://github.com/LaChance-Lab/Multi-Chain-Casino-Games) | MIT |
| Crash | [0xalberto/solana-crash-game](https://github.com/0xalberto/solana-crash-game) | MIT |
| Plinko | [AnsonH/plinko-game](https://github.com/AnsonH/plinko-game) | MIT |
| Casino | [oanapopescu93/casino](https://github.com/oanapopescu93/casino) | MIT |

Veja a [analise completa](doc/research/OPEN_SOURCE_REPOS_ANALYSIS.md) dos repositorios.

---

## Referencias Regulatorias

### Brasil
- [Lei 14.790/2023](https://www.planalto.gov.br/ccivil_03/_ato2023-2026/2023/lei/l14790.htm) - Marco regulatorio das apostas
- [Portaria SPA/MF 722/2024](https://www.legisweb.com.br/legislacao/?id=458565) - Requisitos tecnicos
- [Jogo Responsavel - MF](https://www.gov.br/fazenda/pt-br/composicao/orgaos/secretaria-de-premios-e-apostas/jogo-responsavel)
- [Bets Autorizadas](https://www.lance.com.br/sites-de-apostas/bets-autorizadas.html) - Lista oficial

### Paraguai
- [CONAJZAR/DNIT](https://www.dnit.gov.py/en/web/portal-institucional/conajzar) - Orgao regulador
- [Lei 7438/2025](https://igamingbusiness.com/legal-compliance/regulation/paraguay-enacts-regulation-ending-gambling-monopoly/) - Reforma regulatoria

### Certificacao
- [GLI Standards](https://gaminglabs.com/gli-standards/)
- [BMM Testlabs](https://bmm.com/)
- [Provably Fair](https://www.provably.com/)

---

## Contribuindo

Este e um projeto privado. Para contribuir, entre em contato com a equipe.

---

## Licenca

Este projeto e licenciado sob a licenca MIT - veja o arquivo [LICENSE](LICENSE) para detalhes.

---

## Contato

Para mais informacoes sobre o projeto, entre em contato.

---

*Documentacao gerada em 2026-01-20 com BMad Method.*

---

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="32" />
  <p><strong>Arckhé AI</strong></p>
</div>