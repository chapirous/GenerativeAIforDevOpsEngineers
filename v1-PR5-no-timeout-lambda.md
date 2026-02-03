# Prompt v1 – Baseline Pull Request Analysis 
Você é um engenheiro DevOps responsável por revisar Pull Requests de Infraestrutura como Código (IaC). Analise o Pull Request abaixo e avalie possíveis riscos antes da aprovação para produção. 

O Retorno precisa vir no formato JSON

Considere aspectos como: 
- Segurança 
- Custo 
- Compliance 
- Boas práticas de infraestrutura Com base na análise, informe: 
    - O nível de risco do PR - A decisão sobre a aprovação 
    - Quais áreas foram impactadas 
    - Um breve texto explicando sua avaliação 
    - Sugestões de ações, se necessárias ---

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
  "risk_level": "Médio",
  "approval_decision": "Aprovar com ressalvas",
  "impacted_areas": [
    "Custo",
    "Boas práticas de infraestrutura",
    "Segurança",
    "Compliance"
  ],
  "assessment_summary": "O PR cria uma nova função Lambda para processamento de dados, porém não define explicitamente o parâmetro Timeout, fazendo com que o valor padrão de 3 segundos seja utilizado. Para workloads de processamento de dados, esse timeout pode ser insuficiente e causar falhas intermitentes, retries excessivos e comportamento imprevisível em produção. Além disso, a ausência de definição explícita dificulta a clareza operacional e a padronização da infraestrutura como código. A variável de ambiente aponta para um recurso de produção, o que reforça a necessidade de configuração mais explícita e controlada.",
  "recommendations": [
    "Definir explicitamente o Timeout da função Lambda com base no tempo esperado de processamento",
    "Validar se o MemorySize configurado é adequado e se há correlação com o tempo de execução para otimização de custo",
    "Adicionar monitoramento e alarmes (ex: CloudWatch Alarms) para erros e duração da função",
    "Documentar o comportamento esperado da função e seus limites operacionais",
    "Avaliar o uso de variáveis de ambiente sensíveis com mecanismos seguros (ex: AWS Secrets Manager ou SSM Parameter Store)"
  ]
}
