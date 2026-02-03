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
  "risk_level": "Médio",
  "approval_decision": "Aprovar com ressalvas",
  "impacted_areas": [
    "Custo",
    "Boas práticas de infraestrutura",
    "Compliance"
  ],
  "assessment_summary": "O PR propõe um aumento significativo de capacidade do banco de dados para suportar o pico de tráfego da Black Friday, o que é justificável do ponto de vista operacional. Entretanto, a mudança implica um salto expressivo de custo ao migrar de uma instância db.t3.medium para db.r6g.8xlarge, além de um aumento considerável de armazenamento. Não há evidências no PR de validações de necessidade real, plano de rollback ou estratégia de redução pós-evento, o que pode gerar custos elevados e prolongados.",
  "recommendations": [
    "Validar a necessidade do tamanho da instância com base em métricas históricas (CPU, IOPS, conexões)",
    "Avaliar se um scale gradual ou uso de read replicas atenderia ao cenário com menor impacto de custo",
    "Definir explicitamente um plano de rollback ou downsizing após o período de Black Friday",
    "Garantir que backups automáticos, monitoramento e alarmes de custo estejam configurados",
    "Documentar a mudança como temporária, se aplicável, para fins de governança e auditoria"
  ]
}
