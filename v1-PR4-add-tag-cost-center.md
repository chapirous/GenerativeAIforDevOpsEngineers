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

# PR4-add-tag-cost-center.md
### Descrição do PR
Add cost tracking tags to EC2 instances
### Conteúdo do PR

# terraform/ec2.tf
resource "aws_instance" "worker" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t3.micro"

  tags = {
    Name       = "worker-01"
    CostCenter = "engineering"  # ✅ novo
    Owner      = "platform-team"
  }
}

# Resposta IA (Chat GPT):

{
  "risk_level": "Baixo",
  "approval_decision": "Aprovar",
  "impacted_areas": [
    "Custo",
    "Compliance",
    "Boas práticas de infraestrutura"
  ],
  "assessment_summary": "O PR adiciona a tag CostCenter às instâncias EC2, melhorando a rastreabilidade de custos e a governança financeira da infraestrutura. A mudança não altera comportamento funcional, não impacta segurança e está alinhada às boas práticas de cloud governance e compliance, especialmente para controle e alocação de custos por equipe ou projeto.",
  "recommendations": [
    "Padronizar os valores de CostCenter por meio de variáveis ou validações para evitar inconsistências",
    "Garantir que a política de tags obrigatórias esteja documentada e aplicada a outros recursos além de EC2",
    "Avaliar o uso de ferramentas de compliance ou policies as code (ex: AWS Config, SCPs) para enforcement automático"
  ]
}
