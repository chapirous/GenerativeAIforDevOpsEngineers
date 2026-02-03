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

# PR3-increase-rds-instance.md
### Descrição do PR
Scale up database for Black Friday traffic
### Conteúdo do PR

# terraform/database.tf
resource "aws_rds_instance" "main" {
  identifier        = "myapp-db"
  instance_class    = "db.r6g.8xlarge"  # antes: db.t3.medium
  allocated_storage = 1000                # antes: 100
  engine            = "postgres"
  engine_version    = "14.7"
}

# Resposta IA (Chat GPT):

{
  "risk_level": "medio",
  "approval_decision": "precisa_discussao",
  "impacted_areas": ["custo", "boas_praticas", "seguranca", "compliance"],
  "assessment_summary": "O Pull Request propõe um aumento significativo de capacidade da instância RDS para suportar tráfego de Black Friday, alterando a classe de db.t3.medium para db.r6g.8xlarge e expandindo o armazenamento de 100 GB para 1 TB. Embora a motivação operacional seja válida, o impacto de custo é elevado e não há evidências no PR de análises de capacidade, testes de carga ou estimativas financeiras que justifiquem a mudança. Além disso, não são abordadas boas práticas como uso de autoscaling (quando aplicável), read replicas, ou escalonamento temporário. Do ponto de vista de segurança e compliance, o PR não introduz riscos diretos, mas a ausência de contexto sobre backups, monitoring, manutenção e rollback pode afetar requisitos operacionais e de governança.",
  "recommendations": [
    "Apresentar estimativa de custo do novo tamanho de instância e do aumento de armazenamento.",
    "Anexar evidências de testes de carga ou métricas históricas que justifiquem o dimensionamento proposto.",
    "Avaliar alternativas como read replicas, escalonamento temporário ou uso de instâncias menores com maior eficiência.",
    "Garantir que estratégias de backup, monitoring e plano de rollback estejam documentadas antes da mudança em produção.",
    "Validar se o aumento atende políticas internas de governança e aprovação de custos."
  ]
}
