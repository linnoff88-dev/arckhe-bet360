# Compliance Regulatorio Brasil
# Lei 14.790/2023 e Portarias SPA/MF

**Data:** 2026-01-20
**Autor:** BMad Orchestrator

---

## 1. Marco Regulatorio

### Lei 14.790/2023

A Lei 14.790/2023 estabelece o marco regulatorio para apostas de quota fixa no Brasil. Foi sancionada em dezembro de 2023 e entrou em vigor em janeiro de 2025.

**Orgao Regulador**: Secretaria de Premios e Apostas (SPA), vinculada ao Ministerio da Fazenda.

### Principais Portarias

| Portaria | Data | Tema |
|----------|------|------|
| SPA/MF 722/2024 | 02/05/2024 | Requisitos tecnicos e seguranca |
| SPA/MF 827/2024 | - | Autorizacao de operadores |
| SPA/MF 1.143/2024 | - | AML/FTP (Anti-Lavagem) |
| SPA/MF 1.231/2024 | 31/07/2024 | Jogo Responsavel |
| SPA/MF 1.330/2024 | - | KYC e verificacao de identidade |

---

## 2. Requisitos de Licenciamento

### 2.1 Quem pode operar

- Pessoa juridica constituida segundo leis brasileiras
- Sede e administracao em territorio nacional
- Capital social minimo integralizado (conforme regulamentacao)
- Socio brasileiro com pelo menos 20% do capital
- Diretor responsavel pelo relacionamento com MF
- Ao menos um socio com experiencia em jogos/apostas/loterias

### 2.2 Outorga

| Item | Valor |
|------|-------|
| Taxa de outorga | R$ 30.000.000,00 |
| Validade | 5 anos |
| Renovacao | Disponivel |

### 2.3 Dominio obrigatorio

Todos os operadores devem usar dominio `.bet.br`.

---

## 3. Requisitos Tecnicos (Portaria 722/2024)

### 3.1 Infraestrutura

- Dados hospedados em territorio brasileiro OU
- Replicacao em tempo real para servidores no Brasil
- Certificacao por laboratorio reconhecido pela SPA
- Relatorios de auditoria anuais

### 3.2 Certificacao de Sistemas

Laboratorios reconhecidos:
- GLI (Gaming Laboratories International)
- BMM Testlabs
- iTech Labs
- Outros reconhecidos pela SPA

**Prazo**: 90 dias apos autorizacao para apresentar certificacao.

**Revalidacao**: Anual.

### 3.3 Seguranca Cibernetica

Requisitos:
- Criptografia TLS 1.2 ou superior
- Protecao contra DDoS
- WAF (Web Application Firewall)
- Logs de auditoria por 5 anos
- Plano de continuidade de negocios
- Testes de penetracao periodicos

---

## 4. KYC - Know Your Customer (Portaria 1.330/2024)

### 4.1 Cadastro Obrigatorio

Dados minimos:
- Nome completo
- CPF (validado)
- Data de nascimento (18+)
- Endereco
- Email
- Telefone

### 4.2 Verificacao de Identidade

| Etapa | Requisito |
|-------|-----------|
| CPF | Validacao junto a Receita Federal |
| Biometria | Selfie com reconhecimento facial |
| Documento | CNH, RG ou Passaporte |
| Liveness | Deteccao de vivacidade (anti-spoofing) |

### 4.3 Revalidacao Biometrica

- **A cada 7 dias**: Revalidacao obrigatoria
- **Apos 30 min de inatividade**: Revalidacao antes de continuar
- **A cada aposta** (opcional): Configuravel pelo operador

### 4.4 Geolocalizacao

- Verificar localizacao antes da primeira aposta
- Re-geolocalizacao a cada 30 minutos
- Bloquear usuarios fora do territorio permitido

### 4.5 Restricoes

- Menores de 18 anos: BLOQUEADO
- Funcionarios do operador: BLOQUEADO
- Funcionarios de entidades esportivas (se apostas esportivas): RESTRITO
- Pessoas autoexcluidas: BLOQUEADO

---

## 5. AML - Anti-Money Laundering (Portaria 1.143/2024)

### 5.1 Politica de AML

Operadores devem:
- Documentar politica de AML/FTP
- Aprovacao pela diretoria
- Atualizacao anual
- Treinamento de funcionarios

### 5.2 Avaliacao de Risco

Classificar usuarios em:
- **Baixo Risco**: Verificacao padrao
- **Medio Risco**: Monitoramento aumentado
- **Alto Risco**: Enhanced Due Diligence (EDD)

Fatores de risco:
- Volume de transacoes
- Padroes de apostas
- PEP (Pessoa Politicamente Exposta)
- Pais de origem
- Fonte de recursos

### 5.3 Deteccao de Atividades Suspeitas

Monitorar:
- Depositos seguidos de saques sem atividade de jogo
- Multiplos depositos pequenos (estruturacao)
- Apostas em lados opostos (hedging)
- Movimentacao incompativel com perfil
- Uso de multiplas contas
- Tentativas de burlar limites

### 5.4 Reporte ao COAF/SISCOAF

Comunicar automaticamente:
- Transacoes suspeitas
- Transacoes acima de R$ 50.000 (ou padrao definido)
- Tentativas de transacao bloqueadas

Prazo: 24 horas apos deteccao.

### 5.5 Verificacao de PEP

- Consultar bases de PEPs nacionais e internacionais
- Aplicar EDD para PEPs
- Monitoramento continuo

### 5.6 Penalidades

| Infracao | Penalidade |
|----------|------------|
| Descumprimento leve | Advertencia |
| Descumprimento grave | Multa ate R$ 2.000.000.000 |
| Reincidencia | Suspensao/Cassacao de licenca |
| Crime | Responsabilizacao penal |

---

## 6. Jogo Responsavel (Portaria 1.231/2024)

### 6.1 Ferramentas Obrigatorias

#### Limites de Deposito
- Diario
- Semanal
- Mensal
- Reducao: Imediata
- Aumento: Apos 24 horas

#### Limites de Perda
- Mesmo formato de deposito
- Suspensao de limite: Apos 24 horas

#### Limites de Tempo
- Duracao maxima de sessao
- Alertas periodicos (ex: a cada 1 hora)
- Logout automatico ao atingir limite

#### Autoexclusao
- Temporaria: 24h, 7 dias, 30 dias, 6 meses
- Permanente: Definitiva
- Bloqueio imediato
- Sem comunicacoes promocionais
- Integracao com Plataforma Centralizada (gov.br)

### 6.2 Reality Check

- Pop-up a cada 60 minutos (configuravel)
- Mostrar: Tempo jogado, saldo gasto, resultado liquido
- Opcoes: Continuar ou Encerrar sessao

### 6.3 Comunicacoes

Proibido:
- Sugerir que apostas sao fonte de renda
- Direcionar a menores de idade
- Usar influenciadores para promover apostas

Obrigatorio:
- Aviso de risco em todas as comunicacoes
- Canal de ajuda para ludopatia
- Links para organizacoes de apoio

### 6.4 Atendimento

- Disponivel 24 horas
- Em lingua portuguesa
- Gratuito
- Canais: Chat, telefone, email

---

## 7. Pagamentos

### 7.1 Regras Gerais

- Somente para contas de mesma titularidade (CPF)
- Prazo maximo para saque: 120 minutos
- Transacoes via instituicoes autorizadas pelo BACEN
- Rastreabilidade completa

### 7.2 Metodos Permitidos

- PIX
- Transferencia bancaria
- Cartao de credito/debito
- Criptomoedas (via exchange regulada)

### 7.3 PIX

- Somente com CPF validado
- Deposito: Instantaneo
- Saque: Ate 120 minutos
- Limites configuraveis

---

## 8. Integridade Esportiva

### 8.1 Monitoramento

Operadores devem:
- Integrar organismo de monitoramento (nacional/internacional)
- Detectar padroes suspeitos de apostas
- Reportar irregularidades

### 8.2 Eventos Permitidos

- Eventos esportivos reconhecidos
- Competicoes oficiais
- Restricoes para categorias de base (menores)

---

## 9. Checklist de Implementacao

### Autenticacao/KYC
- [ ] Validacao de CPF
- [ ] Verificacao de idade (18+)
- [ ] Biometria facial no cadastro
- [ ] Deteccao de liveness
- [ ] Revalidacao a cada 7 dias
- [ ] Revalidacao apos inatividade
- [ ] Geolocalizacao inicial
- [ ] Re-geolocalizacao a cada 30 min

### AML/Compliance
- [ ] Classificacao de risco de usuarios
- [ ] Deteccao de PEPs
- [ ] Monitoramento de transacoes suspeitas
- [ ] Integracao com SISCOAF
- [ ] Logs de auditoria (5 anos)
- [ ] Politica de AML documentada

### Jogo Responsavel
- [ ] Limites de deposito (D/S/M)
- [ ] Limites de perda (D/S/M)
- [ ] Limites de tempo de sessao
- [ ] Autoexclusao temporaria
- [ ] Autoexclusao permanente
- [ ] Reality check (alertas periodicos)
- [ ] Integracao plataforma centralizada
- [ ] Canal de atendimento 24h

### Tecnicos
- [ ] Dominio .bet.br
- [ ] Hospedagem/replicacao no Brasil
- [ ] TLS 1.3
- [ ] WAF configurado
- [ ] Certificacao GLI/BMM
- [ ] Auditoria anual
- [ ] Plano de continuidade

### Pagamentos
- [ ] PIX integrado
- [ ] Saque em ate 120 min
- [ ] Somente CPF titular
- [ ] Rastreabilidade completa

---

## 10. Referencias

- [Lei 14.790/2023 - Texto completo](https://www.planalto.gov.br/ccivil_03/_ato2023-2026/2023/lei/l14790.htm)
- [Portaria SPA/MF 722/2024](https://www.legisweb.com.br/legislacao/?id=458565)
- [Jogo Responsavel - Ministerio da Fazenda](https://www.gov.br/fazenda/pt-br/composicao/orgaos/secretaria-de-premios-e-apostas/jogo-responsavel)
- [KYC Requirements Brazil 2025](https://didit.me/blog/kyc-igaming-brazil/)
- [AML Regulations Gaming Brazil](https://alessa.com/blog/brazilian-aml-regulations-gaming-industry/)
- [Bets Autorizadas Brasil](https://www.lance.com.br/sites-de-apostas/bets-autorizadas.html)
