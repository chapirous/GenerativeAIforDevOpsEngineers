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
  "risk_level": "baixo",
  "approval_decision": "aprovar",
  "impacted_areas": ["custo", "compliance", "boas_praticas"],
  "assessment_summary": "O Pull Request adiciona uma tag de CostCenter às instâncias EC2, melhorando significativamente a rastreabilidade de custos e a governança financeira. A mudança não altera configurações de segurança, rede ou capacidade, não introduz riscos operacionais e está alinhada com boas práticas de FinOps e compliance interno. A inclusão de tags padronizadas facilita auditorias, rateio de custos e gestão do ambiente de produção.",
  "recommendations": [
    "Nenhuma ação adicional necessária. A mudança está alinhada com boas práticas e pode ser aprovada."
  ]
}
