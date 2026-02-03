# Análise Automatizada de Pull Requests com IA

**Nome:** Alexandre Francisco Almeida
**RA:** 2501261
**Curso:** MBA Impacta – Turma CLC14

---

## Objetivo

Demonstrar domínio de **prompt engineering** por meio da criação e evolução de prompts para análise automática de Pull Requests (PRs) de **Infraestrutura como Código (IaC)** utilizando um modelo de linguagem (LLM).

O foco é controlar e tornar **confiável** o comportamento da IA em um cenário realista de revisão técnica antes da aprovação em produção.

---

## Contexto

O trabalho simula a rotina de um engenheiro DevOps responsável por revisar múltiplos PRs diariamente, avaliando:

- Segurança  
- Custo  
- Compliance  
- Boas práticas  

Também é abordado um risco relevante no uso de LLMs: **prompt injection**, onde entradas maliciosas tentam manipular o comportamento do modelo.

---

## Estrutura do Repositório

```text
.
├── v1-PR1-add-S3-bucket-to-logs.md
├── v1-PR2-open-ssh-port.md
├── v1-PR3-increase-rds-instance.md
├── v1-PR4-add-tag-cost-center.md
├── v1-PR5-no-timeout-lambda.md
├── v1-PR6-prompt-injection.md
├── v2-PR1-add-S3-bucket-to-logs.md
├── v2-PR2-open-ssh-port.md
├── v2-PR3-increase-rds-instance.md
├── v2-PR4-add-tag-cost-center.md
├── v2-PR5-no-timeout-lambda.md
├── v2-PR6-prompt-injection.md
├── v3-PR1-add-S3-bucket-to-logs.md
├── v3-PR2-open-ssh-port.md
├── v3-PR3-increase-rds-instance.md
├── v3-PR4-add-tag-cost-center.md
├── v3-PR5-no-timeout-lambda.md
├── v3-PR6-prompt-injection.md
└── README.md

```
## Requisitos da Análise

Cada Pull Request é classificado obrigatoriamente pelos seguintes critérios:

### Nível de Risco
- `critico`
- `alto`
- `medio`
- `baixo`

### Decisão
- `aprovar`
- `pedir_mudancas`
- `precisa_discussao`
- `rejeitar`

### Categorias Afetadas
- `seguranca`
- `custo`
- `compliance`
- `boas_praticas`

### Análise Descritiva
- Texto livre justificando:
  - riscos identificados
  - classificação atribuída
  - decisão tomada

### Ações Sugeridas
- Lista objetiva de ações recomendadas
- Quando não aplicável, indicado explicitamente

---

## Evolução dos Prompts

### v1 – Baseline
- Linguagem natural
- JSON simples
- Sem regras explícitas de segurança
- Dependência do julgamento implícito do modelo

### v2 – Structured
- Estrutura fixa em JSON
- Campos obrigatórios e previsíveis
- Critérios claros de avaliação
- Maior consistência entre execuções

### v3 – Schema + Anti Prompt Injection
- Postura explícita de **confiança zero**
- PR tratado como entrada não confiável
- Regras imutáveis contra prompt injection
- Saída contratual consumível por APIs
- Adequado para automação e CI/CD

---

## Resultados

- v1 e v2 podem identificar riscos corretamente, sem garantias formais
- v3 fornece respostas previsíveis, seguras e consistentes
- O PR6 evidencia a importância da proteção explícita contra prompt injection
- Arquivos versionados permitem comparação direta entre versões

Observação importante: embora em algumas execuções o prompt v1 e v2 tenha
identificado corretamente riscos críticos (como no PR6), esse
comportamento não é garantido nem consistente.

A ausência de regras explícitas no v1 e v2 permite que o modelo, em outros
contextos ou variações do mesmo PR, seja induzido a decisões incorretas.
Essa limitação motivou a evolução para o prompt v3, que impõem
estrutura, contrato de saída e proteção explícita contra prompt injection.

---

## Conclusão

- IA confiável exige engenharia de prompts
- Estrutura e contrato de saída são essenciais
- Governança e segurança devem ser explícitas
- IA deve ser tratada como parte de um sistema de engenharia


