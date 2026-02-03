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

# PR2-open-ssh-port.md
### Descrição do PR
Allow SSH access for debugging production issues
### Conteúdo do PR
```text
# terraform/security_groups.tf
resource "aws_security_group_rule" "ssh_access" {
  type              = "ingress"
  from_port         = 22
  to_port           = 22
  protocol          = "tcp"
  cidr_blocks       = ["0.0.0.0/0"]  # ⚠️
  security_group_id = aws_security_group.web.id
}

# PR2-open-ssh-port.md

{
  "risk_level": "Alto",
  "approval_decision": "Rejeitar",
  "impacted_areas": [
    "Segurança",
    "Compliance",
    "Boas práticas de infraestrutura"
  ],
  "assessment_summary": "O PR abre acesso SSH (porta 22) para toda a internet (0.0.0.0/0) em um ambiente de produção, o que representa um risco crítico de segurança. Essa configuração aumenta significativamente a superfície de ataque, expondo os recursos a tentativas de força bruta, exploração de vulnerabilidades e acessos não autorizados. Além disso, a prática não está alinhada com padrões de compliance e boas práticas de cloud security, que recomendam acesso administrativo restrito, auditável e temporário.",
  "recommendations": [
    "Restringir o acesso SSH a blocos CIDR específicos (ex: IPs corporativos ou VPN)",
    "Preferir acesso via bastion host ou AWS Systems Manager Session Manager, evitando SSH direto",
    "Implementar controles de acesso temporários (ex: regras criadas sob demanda e removidas após o uso)",
    "Adicionar justificativa operacional e prazo de validade para acessos de exceção em produção",
    "Avaliar o uso de chaves SSH rotacionadas e autenticação forte, se o SSH for realmente necessário"
  ]
}
