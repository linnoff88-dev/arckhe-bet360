# Product Requirements Document (PRD)
# Bet360 - Plataforma White-Label de iGaming

**Versao:** 1.0
**Data:** 2026-01-20
**Status:** Draft
**Autor:** BMad Orchestrator + Equipe Bet360

---

## 1. Visao Geral do Produto

### 1.1 Resumo Executivo

O Bet360 e uma plataforma White-Label de iGaming B2B que fornece tecnologia completa para operadores licenciados de apostas esportivas e jogos online. O produto permite que empresas com licenca regulatoria (SPA/MF no Brasil, CONAJZAR no Paraguai) operem sua propria marca de apostas sem necessidade de desenvolver tecnologia do zero.

### 1.2 Problema a Resolver

1. **Alto custo de desenvolvimento**: Criar plataforma de iGaming do zero custa R$ 2-10 milhoes
2. **Complexidade regulatoria**: Lei 14.790/2023 exige compliance tecnico rigoroso
3. **Time-to-market**: Operadores licenciados precisam lancar rapidamente
4. **Falta de opcoes nacionais**: Maioria das solucoes White-Label sao estrangeiras

### 1.3 Solucao Proposta

Plataforma SaaS multi-tenant que oferece:
- Backend completo (auth, wallet, games, payments, admin)
- Frontend customizavel (white-label)
- Compliance nativo (KYC, AML, Jogo Responsavel)
- Jogos HTML5 proprios (sem royalties)
- Integracao com pagamentos locais (PIX, Tigo Money)

### 1.4 Modelo de Negocio

| Receita | Descricao |
|---------|-----------|
| Taxa de Setup | R$ 50.000 - 200.000 (implantacao) |
| Mensalidade SaaS | R$ 5.000 - 20.000/mes |
| Revenue Share | 1-3% do GGR (Gross Gaming Revenue) |
| Desenvolvimento Custom | R$ 200-500/hora |

---

## 2. Mercado e Usuarios

### 2.1 Mercado-Alvo

#### Brasil
- **TAM**: 183+ operadores licenciados pela SPA/MF (Jan 2026)
- **Regulamentacao**: Lei 14.790/2023, Portarias SPA/MF
- **Potencial**: Mercado estimado em R$ 100+ bilhoes/ano

#### Paraguai
- **TAM**: Mercado em abertura com Lei 7438/2025
- **Regulamentacao**: CONAJZAR/DNIT
- **Potencial**: Primeiro mercado LATAM apos Brasil

### 2.2 Personas de Usuario

#### Persona 1: Operador Licenciado (Cliente B2B)
- **Quem**: Empresa com licenca SPA/MF para operar apostas
- **Necessidade**: Plataforma pronta para lancar operacao rapidamente
- **Dor**: Alto investimento inicial, complexidade tecnica
- **Ganho**: Time-to-market rapido, compliance garantido

#### Persona 2: Apostador (Usuario Final)
- **Quem**: Brasileiro 18+, interessado em apostas esportivas/jogos
- **Necessidade**: Plataforma segura, depositos/saques rapidos
- **Dor**: Sites nao confiaveis, demora em pagamentos
- **Ganho**: PIX instantaneo, jogos justos (provably fair)

#### Persona 3: Administrador da Plataforma (Staff do Operador)
- **Quem**: Funcionario do operador que gerencia a plataforma
- **Necessidade**: Dashboard intuitivo, relatorios compliance
- **Dor**: Interfaces complexas, falta de visibilidade
- **Ganho**: Gestao simplificada, alertas automaticos

---

## 3. Requisitos Funcionais

### 3.1 Modulo de Autenticacao e KYC

#### RF-AUTH-001: Cadastro de Usuario
- **Descricao**: Permitir cadastro com email, senha, CPF
- **Criterios de Aceite**:
  - Validar formato de email
  - Senha minima 8 caracteres (maiuscula, minuscula, numero)
  - CPF validado via algoritmo e consulta RF (opcional)
  - Aceite obrigatorio de termos de uso
  - Verificacao de idade (18+)

#### RF-AUTH-002: Login Seguro
- **Descricao**: Autenticacao com email/senha + 2FA opcional
- **Criterios de Aceite**:
  - Rate limiting (5 tentativas/15 min)
  - Bloqueio temporario apos falhas
  - Suporte a 2FA (TOTP, SMS)
  - Sessao expira em 24h de inatividade

#### RF-AUTH-003: Biometria Facial (KYC)
- **Descricao**: Verificacao biometrica obrigatoria conforme Portaria 1.330/2024
- **Criterios de Aceite**:
  - Captura de selfie no cadastro
  - Comparacao com documento (CNH/RG)
  - Liveness detection (anti-spoofing)
  - Revalidacao a cada 7 dias
  - Revalidacao apos 30 min de inatividade
- **Integracao**: Legitimuz, Veriff, Onfido (a definir)

#### RF-AUTH-004: Verificacao PEP/AML
- **Descricao**: Identificar Pessoas Politicamente Expostas
- **Criterios de Aceite**:
  - Consulta em bases PEP nacionais
  - Classificacao de risco (baixo/medio/alto)
  - Alerta automatico para risco alto
  - Bloqueio preventivo se necessario

### 3.2 Modulo de Carteira (Wallet)

#### RF-WALLET-001: Saldo Multi-Moeda
- **Descricao**: Gerenciar saldo em multiplas moedas
- **Criterios de Aceite**:
  - Suporte a BRL, PYG, USD, USDT
  - Conversao automatica com taxa configuravel
  - Saldo disponivel vs saldo em bonus
  - Historico completo de transacoes

#### RF-WALLET-002: Deposito via PIX
- **Descricao**: Depositos instantaneos via PIX (Brasil)
- **Criterios de Aceite**:
  - Geracao de QR Code dinamico
  - Confirmacao em tempo real (webhook)
  - Limite minimo/maximo configuravel
  - Somente CPF titular da conta
- **Integracao**: Efi Bank, Zro Bank

#### RF-WALLET-003: Deposito via Tigo Money
- **Descricao**: Depositos via carteira digital (Paraguai)
- **Criterios de Aceite**:
  - Integracao API Tigo Money
  - Confirmacao via callback
  - Conversao PYG automatica
- **Integracao**: Tigo Money API

#### RF-WALLET-004: Saque
- **Descricao**: Saques para conta bancaria do usuario
- **Criterios de Aceite**:
  - Prazo maximo 120 minutos (lei)
  - Somente para conta de mesma titularidade
  - Aprovacao automatica ou manual (configuravel)
  - Rollover de bonus antes de saque

### 3.3 Modulo de Jogos

#### RF-GAME-001: Catalogo de Jogos
- **Descricao**: Exibir jogos disponiveis com filtros
- **Criterios de Aceite**:
  - Grid responsivo com thumbnails
  - Filtro por categoria (slots, crash, arcade, cartas)
  - Busca por nome
  - Indicador de popularidade
  - Modo demo disponivel

#### RF-GAME-002: Motor de Jogo (Game Engine)
- **Descricao**: Executar jogos HTML5 com apostas reais
- **Criterios de Aceite**:
  - Carregar jogo em iframe/canvas
  - Comunicacao bidirecional (aposta, resultado)
  - Provably fair (server seed + client seed)
  - Registro de todas as apostas
  - RTP configuravel por administrador

#### RF-GAME-003: Crash Game (Aviator)
- **Descricao**: Jogo tipo crash com multiplicador crescente
- **Criterios de Aceite**:
  - Apostas antes do inicio da rodada
  - Multiplicador crescente em tempo real
  - Cash-out a qualquer momento
  - Crash aleatorio (provably fair)
  - Historico de rodadas anteriores
  - House edge: 3-5%

#### RF-GAME-004: Mines (Campo Minado)
- **Descricao**: Jogo de revelacao com multiplicadores
- **Criterios de Aceite**:
  - Grid configuravel (ex: 5x5)
  - Quantidade de minas configuravel
  - Multiplicador aumenta a cada casa segura
  - Cash-out a qualquer momento
  - Provably fair
  - House edge: 5-8%

#### RF-GAME-005: Plinko
- **Descricao**: Jogo de fisica com bola caindo
- **Criterios de Aceite**:
  - Simulacao de fisica realista
  - Multiplicadores nos slots inferiores
  - Niveis de risco (baixo/medio/alto)
  - Provably fair
  - House edge: 4-6%

#### RF-GAME-006: Dice (Dados)
- **Descricao**: Jogo de previsao de numero
- **Criterios de Aceite**:
  - Escolher over/under de um numero
  - Multiplicador baseado na probabilidade
  - Slider interativo
  - Provably fair
  - House edge: 2-4%

#### RF-GAME-007: Slot Machine
- **Descricao**: Caca-niqueis HTML5
- **Criterios de Aceite**:
  - Animacoes suaves de giro
  - Linhas de pagamento configuraveis
  - Bonus rounds
  - RTP configuravel (92-96%)
  - Provably fair

### 3.4 Modulo de Jogo Responsavel

#### RF-RG-001: Limites de Deposito
- **Descricao**: Permitir usuario definir limites
- **Criterios de Aceite**:
  - Limite diario, semanal, mensal
  - Reducao imediata, aumento apos 24h
  - Alerta ao atingir 80% do limite
  - Bloqueio ao atingir limite

#### RF-RG-002: Limites de Perda
- **Descricao**: Limitar perdas financeiras
- **Criterios de Aceite**:
  - Configuracao por periodo
  - Suspensao de limite apos 24h
  - Relatorio de perdas visivel

#### RF-RG-003: Limites de Tempo
- **Descricao**: Controlar tempo de sessao
- **Criterios de Aceite**:
  - Definir duracao maxima de sessao
  - Alerta a cada hora de jogo
  - Logout automatico ao atingir limite

#### RF-RG-004: Autoexclusao
- **Descricao**: Permitir usuario se excluir
- **Criterios de Aceite**:
  - Opcoes: 24h, 7 dias, 30 dias, 6 meses, permanente
  - Bloqueio imediato de conta
  - Sem comunicacoes promocionais durante exclusao
  - Integracao com Plataforma Centralizada (gov.br)

#### RF-RG-005: Reality Check
- **Descricao**: Alertas periodicos durante jogo
- **Criterios de Aceite**:
  - Pop-up a cada 60 minutos (configuravel)
  - Mostrar tempo jogado e saldo gasto
  - Opcao de continuar ou encerrar

### 3.5 Modulo Administrativo (Back-Office)

#### RF-ADMIN-001: Dashboard
- **Descricao**: Visao geral da operacao
- **Criterios de Aceite**:
  - GGR por periodo (dia/semana/mes)
  - Usuarios ativos, novos, churned
  - Depositos vs Saques
  - Graficos interativos

#### RF-ADMIN-002: Gestao de Usuarios
- **Descricao**: Gerenciar contas de apostadores
- **Criterios de Aceite**:
  - Lista com filtros avancados
  - Visualizar perfil completo
  - Ajustar saldo (com justificativa e log)
  - Bloquear/desbloquear conta
  - Exportar para compliance

#### RF-ADMIN-003: Gestao de Jogos
- **Descricao**: Configurar jogos disponiveis
- **Criterios de Aceite**:
  - Ativar/desativar jogos
  - Configurar RTP por jogo
  - Definir limites de aposta (min/max)
  - Ver estatisticas por jogo

#### RF-ADMIN-004: Gestao Financeira
- **Descricao**: Controle de transacoes
- **Criterios de Aceite**:
  - Fila de saques pendentes
  - Aprovar/rejeitar saques
  - Conciliacao bancaria
  - Relatorios fiscais

#### RF-ADMIN-005: Compliance e AML
- **Descricao**: Monitoramento de compliance
- **Criterios de Aceite**:
  - Alertas AML em tempo real
  - Relatorios COAF/SISCOAF
  - Lista de autoexclusoes
  - Logs de auditoria completos

#### RF-ADMIN-006: Configuracoes White-Label
- **Descricao**: Customizacao da marca
- **Criterios de Aceite**:
  - Upload de logo
  - Definir cores (primary, secondary, accent)
  - Configurar dominio
  - Textos customizaveis

### 3.6 Modulo Multi-Tenant

#### RF-MT-001: Isolamento de Dados
- **Descricao**: Garantir separacao entre tenants
- **Criterios de Aceite**:
  - Cada tenant tem schema/database proprio
  - Nenhum vazamento de dados entre tenants
  - Backup independente por tenant

#### RF-MT-002: Onboarding de Tenant
- **Descricao**: Processo de criacao de novo cliente
- **Criterios de Aceite**:
  - Formulario de cadastro de operador
  - Provisionamento automatico de ambiente
  - Configuracao inicial guiada
  - Ambiente sandbox para testes

---

## 4. Requisitos Nao-Funcionais

### 4.1 Performance

| Metrica | Requisito |
|---------|-----------|
| Tempo de resposta API | < 200ms (p95) |
| Tempo de carregamento pagina | < 3s (3G) |
| Transacoes simultaneas | 10.000+ TPS |
| Disponibilidade | 99.9% uptime |

### 4.2 Seguranca

| Requisito | Descricao |
|-----------|-----------|
| Criptografia | TLS 1.3, AES-256 em repouso |
| Autenticacao | JWT + refresh token |
| Rate Limiting | Por IP e por usuario |
| WAF | Protecao contra OWASP Top 10 |
| Logs | Retencao 5 anos (compliance) |
| Penetration Test | Anual (certificacao) |

### 4.3 Escalabilidade

- Arquitetura stateless para horizontal scaling
- Auto-scaling baseado em carga
- CDN para assets estaticos
- Database sharding por tenant (enterprise)

### 4.4 Compliance

| Regulamentacao | Requisitos |
|----------------|------------|
| Lei 14.790/2023 | Todos os requisitos listados em RF |
| LGPD | Consentimento, portabilidade, exclusao |
| PCI-DSS | Se processar cartoes diretamente |
| GLI-19 | Certificacao de RNG e jogos |

### 4.5 Localizacao

| Aspecto | Suporte |
|---------|---------|
| Idiomas | PT-BR, ES, EN |
| Moedas | BRL, PYG, USD, USDT |
| Timezone | America/Sao_Paulo, America/Asuncion |
| Formatos | Data, numero, telefone locais |

---

## 5. Integrações de Terceiros

### 5.1 Pagamentos

| Provedor | Pais | Metodo | Prioridade |
|----------|------|--------|------------|
| Efi Bank | Brasil | PIX | P0 |
| Zro Bank | Brasil | PIX | P1 |
| Asaas | Brasil | Cartao, Boleto | P1 |
| Tigo Money | Paraguai | Wallet | P0 |
| Binance Pay | Global | Crypto | P2 |

### 5.2 KYC/Biometria

| Provedor | Servico | Prioridade |
|----------|---------|------------|
| Legitimuz | Biometria + Docs | P0 |
| Veriff | Alternativa | P1 |
| Onfido | Alternativa | P1 |

### 5.3 Compliance

| Sistema | Descricao | Prioridade |
|---------|-----------|------------|
| SISCOAF | Relatorios AML | P0 |
| Plataforma Autoexclusao | Gov.br | P0 |
| Bureau PEP | Consulta PEPs | P0 |

### 5.4 Infraestrutura

| Servico | Uso |
|---------|-----|
| AWS Brasil (sa-east-1) | Hosting principal |
| CloudFlare | CDN + WAF |
| SendGrid/AWS SES | Email transacional |
| Twilio | SMS (2FA) |
| Firebase | Push notifications |

---

## 6. Fases de Desenvolvimento

### Fase 1: MVP (Core Platform)

**Escopo:**
- Autenticacao + KYC basico
- Carteira BRL + PIX
- 3 jogos: Crash, Mines, Dice
- Admin basico
- 1 tenant demo

**Criterio de Conclusao:**
- Usuario consegue se cadastrar, depositar, jogar e sacar
- Admin consegue visualizar dashboard e aprovar saques

### Fase 2: Compliance Completo

**Escopo:**
- KYC completo (biometria, PEP, AML)
- Jogo Responsavel (todos os limites)
- Integracao SISCOAF
- Geolocalizacao continua
- Multi-tenant basico
- +2 jogos: Plinko, Slot

**Criterio de Conclusao:**
- Plataforma atende todos requisitos Portarias SPA/MF
- Multi-tenant funcionando com 2+ operadores

### Fase 3: Multi-Jurisdicao

**Escopo:**
- Multi-moeda (BRL, PYG, USD, USDT)
- Pagamentos Paraguai (Tigo Money)
- Multi-idioma (PT-BR, ES)
- White-label customization
- +3 jogos

**Criterio de Conclusao:**
- Operador no Paraguai consegue operar
- Branding totalmente customizavel

### Fase 4: Enterprise

**Escopo:**
- Certificacao GLI/BMM
- API para integracoes terceiros
- Sports betting module (opcional)
- Analytics avancado
- Mobile apps

**Criterio de Conclusao:**
- Certificado GLI-19 obtido
- Apps publicados nas stores

---

## 7. Metricas de Sucesso

### KPIs de Produto

| Metrica | Meta MVP | Meta 12 meses |
|---------|----------|---------------|
| Tenants ativos | 1 (demo) | 10+ |
| GGR processado | R$ 100k | R$ 10M/mes |
| Uptime | 99% | 99.9% |
| NPS operadores | - | > 50 |

### KPIs Tecnicos

| Metrica | Meta |
|---------|------|
| Cobertura de testes | > 80% |
| Tempo deploy | < 30 min |
| Incidentes criticos/mes | < 2 |
| MTTR | < 1 hora |

---

## 8. Riscos e Mitigacoes

| Risco | Probabilidade | Impacto | Mitigacao |
|-------|---------------|---------|-----------|
| Mudanca regulatoria | Media | Alto | Arquitetura modular, acompanhamento SPA |
| Falha em certificacao | Media | Alto | Consultoria GLI desde o inicio |
| Integracao pagamentos | Media | Medio | Multiplos provedores, fallback |
| Fraude/Lavagem | Alta | Alto | AML robusto, limites, monitoramento |
| Problemas de escala | Baixa | Alto | Load testing, auto-scaling |

---

## 9. Glossario

| Termo | Definicao |
|-------|-----------|
| GGR | Gross Gaming Revenue - Receita bruta de jogos (apostas - premios) |
| RTP | Return to Player - Percentual teorico de retorno ao jogador |
| House Edge | Margem da casa (100% - RTP) |
| KYC | Know Your Customer - Verificacao de identidade |
| AML | Anti-Money Laundering - Prevencao a lavagem de dinheiro |
| PEP | Pessoa Politicamente Exposta |
| COAF | Conselho de Controle de Atividades Financeiras |
| SPA | Secretaria de Premios e Apostas |
| Provably Fair | Sistema verificavel de aleatoriedade |
| Tenant | Cliente B2B (operador) na plataforma multi-tenant |

---

## 10. Aprovacoes

| Papel | Nome | Data | Status |
|-------|------|------|--------|
| Product Owner | [Pendente] | - | Pendente |
| Tech Lead | [Pendente] | - | Pendente |
| Stakeholder | [Pendente] | - | Pendente |

---

## Changelog

| Versao | Data | Autor | Mudancas |
|--------|------|-------|----------|
| 1.0 | 2026-01-20 | BMad Orchestrator | Versao inicial |
