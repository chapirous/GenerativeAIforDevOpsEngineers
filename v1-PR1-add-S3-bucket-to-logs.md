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
  "risk_level": "Médio",
  "approval_decision": "Aprovar com ressalvas",
  "impacted_areas": [
    "Segurança",
    "Compliance",
    "Custo",
    "Boas práticas de infraestrutura"
  ],
  "assessment_summary": "O PR cria um bucket S3 para armazenamento de logs em produção, o que é coerente com a finalidade proposta. No entanto, a configuração está incompleta para um ambiente produtivo. Não há definições explícitas de políticas de segurança (como bloqueio de acesso público, criptografia em repouso e versionamento), nem controles de ciclo de vida para gerenciamento de custos. Além disso, o nome do bucket é fixo e pode gerar conflitos em múltiplas contas ou regiões.",
  "recommendations": [
    "Habilitar bloqueio de acesso público (Block Public Access) para evitar exposição indevida de logs",
    "Configurar criptografia em repouso (SSE-S3 ou SSE-KMS) para atender requisitos de segurança e compliance",
    "Avaliar a ativação de versionamento para proteção contra exclusões acidentais",
    "Adicionar políticas de lifecycle para expiração ou transição de logs antigos, reduzindo custos",
    "Parametrizar o nome do bucket (ex: usando variáveis Terraform) para evitar conflitos entre ambientes ou contas"
  ]
}





