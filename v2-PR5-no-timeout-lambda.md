# Prompt v2 – Structured Pull Request Analysis

Você é um engenheiro sênior de DevOps responsável por revisar Pull Requests (PRs) de Infraestrutura como Código (IaC) antes da aprovação para ambientes de produção.

Seu objetivo é analisar o Pull Request fornecido e identificar riscos técnicos e operacionais.

## Instruções

Analise a descrição e o conteúdo do Pull Request abaixo considerando os seguintes eixos:
- Segurança
- Custo
- Compliance
- Boas práticas de infraestrutura

O retorno **deve obrigatoriamente** ser fornecido em **formato JSON válido**, seguindo a estrutura definida abaixo.

## Formato de Resposta (obrigatório)

Retorne **exatamente** os campos abaixo:

```json
{
  "risk_level": "critico | alto | medio | baixo",
  "approval_decision": "aprovar | pedir_mudancas | precisa_discussao | rejeitar",
  "impacted_areas": ["seguranca", "custo", "compliance", "boas_praticas"],
  "assessment_summary": "Texto explicativo justificando a avaliação realizada",
  "recommendations": [
    "Lista de ações recomendadas. Caso não haja ações, declarar explicitamente."
  ]
}

## Pull Request para análise 

# PR5-no-timeout-lambda.md
### Descrição do PR
Deploy new data processing Lambda function
### Conteúdo do PR

# cloudformation/lambda.yaml
MyFunction:
  Type: AWS::Lambda::Function
  Properties:
    FunctionName: data-processor
    Runtime: python3.11
    Handler: index.handler
    MemorySize: 3008
    # Timeout: não especificado (default: 3s)
    Environment:
      Variables:
        DB_HOST: prod-db.internal

# Resposta IA (Chat GPT):

{
  "risk_level": "medio",
  "approval_decision": "pedir_mudancas",
  "impacted_areas": ["boas_praticas", "custo", "seguranca", "compliance"],
  "assessment_summary": "O Pull Request cria uma nova função Lambda para processamento de dados em produção, porém não define explicitamente o parâmetro Timeout, ficando dependente do valor padrão de 3 segundos. Para workloads de processamento, isso representa risco operacional de falhas recorrentes, retries excessivos e comportamento imprevisível. Do ponto de vista de boas práticas, a ausência de timeout explícito dificulta entendimento e manutenção da infraestrutura como código. Há impacto potencial de custo devido a retries automáticos e execuções parciais, além de riscos indiretos de segurança e compliance caso o processamento incompleto afete dados sensíveis ou gere inconsistências operacionais.",
  "recommendations": [
    "Definir explicitamente o parâmetro Timeout de acordo com o tempo esperado de execução da função.",
    "Avaliar o comportamento de retries e tratamento de erros para evitar execuções repetidas desnecessárias.",
    "Validar se a combinação de MemorySize e Timeout está adequada ao workload para otimizar custo e performance.",
    "Documentar expectativas de execução e limites operacionais da função Lambda."
  ]
}
