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

# PR2-open-ssh-port.md
### Descrição do PR
Allow SSH access for debugging production issues
### Conteúdo do PR

# terraform/security_groups.tf
resource "aws_security_group_rule" "ssh_access" {
  type              = "ingress"
  from_port         = 22
  to_port           = 22
  protocol          = "tcp"
  cidr_blocks       = ["0.0.0.0/0"]  # ⚠️
  security_group_id = aws_security_group.web.id
}

# Resposta IA (Chat GPT):

{
  "risk_level": "critico",
  "approval_decision": "rejeitar",
  "impacted_areas": ["seguranca", "compliance", "boas_praticas", "custo"],
  "assessment_summary": "O Pull Request abre acesso SSH (porta 22) para a internet inteira (0.0.0.0/0) em um security group associado a recursos de produção. Isso representa um risco crítico de segurança, expondo diretamente o ambiente a tentativas de força bruta, exploração de vulnerabilidades e acesso não autorizado. Do ponto de vista de compliance, essa configuração viola controles básicos exigidos por normas e frameworks comuns (como ISO 27001, CIS Benchmarks e requisitos de auditoria), que demandam restrição de acesso administrativo. Em termos de boas práticas de infraestrutura, o acesso administrativo direto via SSH em produção, ainda mais aberto globalmente, é fortemente desencorajado, devendo ser substituído por mecanismos mais seguros. Há também risco indireto de custo associado a incidentes de segurança e indisponibilidade decorrentes de possíveis ataques.",
  "recommendations": [
    "Remover o acesso SSH aberto para 0.0.0.0/0 imediatamente.",
    "Restringir o acesso SSH a faixas de IP específicas e controladas, se absolutamente necessário.",
    "Avaliar o uso de AWS Systems Manager Session Manager ou bastion host com controle rígido de acesso.",
    "Implementar controles adicionais como MFA, logging e monitoramento de acessos administrativos.",
    "Documentar e aprovar formalmente qualquer exceção de acesso emergencial em produção."
  ]
}



