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

# PR1-add-S3-bucket-to-logs.md 
### Descrição do PR Adding S3 bucket for application logs storage 
### Conteúdo do PR:

# terraform/storage.tf
resource "aws_s3_bucket" "app_logs" {
  bucket = "myapp-logs-prod"

  tags = {
    Environment = "production"
    Team        = "platform"
  }
}

# Resposta IA (Chat GPT):

{
  "risk_level": "alto",
  "approval_decision": "pedir_mudancas",
  "impacted_areas": ["seguranca", "compliance", "boas_praticas", "custo"],
  "assessment_summary": "O PR cria um bucket S3 para logs de aplicação em ambiente de produção, porém não define controles essenciais de segurança e governança. Não há configuração de criptografia em repouso, bloqueio de acesso público, políticas de retenção/expiração de logs, versionamento ou logging/auditoria do próprio bucket. Além disso, o nome do bucket é estático e hardcoded, o que reduz reutilização e aumenta risco de conflito entre contas ou regiões. A ausência dessas configurações representa riscos de segurança (exposição de dados sensíveis), compliance (requisitos comuns como LGPD, ISO 27001 e boas práticas de auditoria) e custo (crescimento ilimitado de armazenamento de logs).",
  "recommendations": [
    "Habilitar criptografia em repouso (SSE-S3 ou SSE-KMS) no bucket.",
    "Ativar block public access para evitar qualquer exposição pública acidental.",
    "Definir versionamento e, preferencialmente, lifecycle rules para retenção e expiração de logs.",
    "Configurar políticas de acesso (IAM/bucket policy) com princípio do menor privilégio.",
    "Avaliar habilitar access logging ou integração com CloudTrail para auditoria.",
    "Parametrizar o nome do bucket (ex: usando variáveis Terraform) para melhorar reutilização e evitar conflitos.",
    "Avaliar impacto de custo e definir políticas de retenção alinhadas às necessidades de compliance."
  ]
}




