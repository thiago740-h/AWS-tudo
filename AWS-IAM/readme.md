# 🛡️ Relatório de Laboratório: Introdução ao AWS IAM

Este repositório contém o resumo técnico das atividades práticas realizadas no laboratório de **AWS Identity and Access Management (IAM)**. O foco do estudo foi a gestão centralizada de usuários, grupos e políticas de segurança na nuvem.

---

## 1. Visão Geral do IAM
O **AWS IAM** é o serviço essencial para o controle de acesso. Ele garante a segurança da conta através de dois pilares:
* **Autenticação:** Identifica quem está acessando (usuários e perfis).
* **Autorização:** Define o que cada identidade pode fazer (políticas e permissões).

---

## 2. Estrutura de Permissões do Laboratório

Durante a prática, analisamos como as permissões são herdadas via grupos. A estrutura configurada foi:

| Grupo | Política Associada | Permissão Resultante |
| :--- | :--- | :--- |
| **S3-Support** | `AmazonS3ReadOnlyAccess` | Visualizar buckets e arquivos no S3. |
| **EC2-Support** | `AmazonEC2ReadOnlyAccess` | Visualizar instâncias e configurações do EC2. |
| **EC2-Admin** | *Política Inline* | Visualizar, Iniciar e Interromper instâncias EC2. |



---

## 3. Atividades Práticas Executadas

### Tarefa 1: Exploração e Mapeamento
* Identificação dos usuários pré-criados: `user-1`, `user-2` e `user-3`.
* Análise das **Políticas Gerenciadas** (padrão AWS) vs. **Políticas Inline** (personalizadas para um grupo específico).

### Tarefa 2: Aplicação do Princípio do Privilégio Mínimo
* **Ação:** Movimentação dos usuários para seus respectivos grupos conforme a função de trabalho.
* **Conceito:** Demonstração de como o gerenciamento por grupos é mais escalável do que aplicar permissões individuais a cada usuário.

### Tarefa 3: Testes de Acesso (Validação)
Para validar as políticas, realizamos logins individuais em janelas anônimas:

1.  **Usuário de S3 (`user-1`):**
    * **Resultado:** Conseguiu listar arquivos no S3, mas recebeu erro de "Acesso Negado" ao tentar abrir o console do EC2.
2.  **Usuário de Suporte EC2 (`user-2`):**
    * **Resultado:** Visualizou os servidores, mas ao tentar **interromper** uma instância, a AWS bloqueou a ação por falta de privilégios de escrita.
3.  **Usuário Administrador EC2 (`user-3`):**
    * **Resultado:** Conseguiu executar o comando de **parar (Stop)** a instância `LabHost` com sucesso.

---

## 4. Conclusão e Aprendizados

* **Segurança Granular:** O IAM permite um controle tão fino que um usuário pode ter permissão para ver um servidor, mas não para desligá-lo.
* **Políticas:** Vimos que uma política é um documento JSON que define:
    * **Efeito:** Permitir ou Negar.
    * **Ação:** O comando da API (ex: `ec2:StopInstances`).
    * **Recurso:** Onde a ação pode ser feita (ex: em todos os recursos `*`).
* **Isolamento:** Usuários sem permissões explícitas não conseguem realizar nenhuma ação, garantindo a proteção dos dados da empresa.

---
*Documentação preparada para fins de estudo técnico sobre AWS Cloud.*