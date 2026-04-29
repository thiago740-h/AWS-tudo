# Relatório de Laboratório: Introdução ao Amazon EC2

Este documento detalha o roteiro e as principais atividades realizadas no **Laboratório 3 da AWS Academy**, focado no provisionamento, gerenciamento e redimensionamento de instâncias EC2.

---

## 🚀 Percurso do Laboratório

### Tarefa 1: Lançamento da Instância
O objetivo foi configurar um servidor virtual com automação de processos.
* **Nome do Recurso:** `Web Server`
* **AMI (Imagem de Máquina):** Amazon Linux 2023.
* **Tipo de Instância:** `t2.micro` (1 vCPU, 1 GiB de memória).
* **Configuração de Rede:** Alocado na `Lab VPC` dentro da `Public Subnet 1`.
* **Segurança:** Criação do `Web Server security group`.
* **Automação (User Data):** Inserção de script Bash para instalação automática do servidor Apache (`httpd`) e criação de uma página HTML de boas-vindas.

### Tarefa 2: Monitoramento
Acompanhamento do status e integridade do recurso.
* **Status Checks:** Verificação do progresso até a aprovação das verificações `2/2`.
* **System Log:** Análise dos logs de inicialização para confirmar se a instalação do Apache ocorreu sem erros.
* **Instance Screenshot:** Captura visual do console remoto para validar o estado do sistema operacional sem necessidade de acesso SSH direto.

### Tarefa 3: Acesso ao Servidor
Configuração de firewall e teste de conectividade.
* **Endereçamento:** Identificação do **IPv4 Público** da instância.
* **Regras de Firewall:** Edição do *Security Group* para permitir tráfego de entrada na **porta 80 (HTTP)** vindo de qualquer origem (`0.0.0.0/0`).
* **Teste de Acesso:** Validação via navegador confirmando a mensagem: *"Hello From Your Web Server!"*.

### Tarefa 4: Redimensionamento (Scaling Up)
Ajuste de recursos computacionais conforme a demanda.
1. **Interrupção:** Execução do *Stop* na instância (obrigatório para alteração de hardware).
2. **Upgrade de Tipo:** Mudança de `t2.micro` para `t2.small`.
3. **Expansão de Armazenamento:** Modificação do volume **EBS** de 8 GiB para 10 GiB.
4. **Reinicialização:** Inicialização da instância com os novos parâmetros de CPU, Memória e Disco.

### Tarefa 5: Service Quotas
* **Exploração de Limites:** Uso da ferramenta *Service Quotas* para verificar os limites regionais da AWS, como o número máximo de vCPUs para instâncias On-Demand Standard.

### Tarefa 6: Teste de Proteção
Validação de mecanismos contra erros operacionais.
* **Trava de Segurança:** Tentativa de interrupção da instância com a proteção ativa (resultado: erro esperado e bloqueio da ação).
* **Desativação:** Alteração manual do atributo via menu *Actions > Instance Settings*.
* **Encerramento:** Interrupção final da instância para limpeza do ambiente.

---

## 🛠️ Tecnologias Utilizadas

| Tecnologia | Descrição |
| :--- | :--- |
| **AWS EC2** | Serviço de computação em nuvem (servidores virtuais). |
| **AWS EBS** | Armazenamento em bloco persistente para instâncias. |
| **Amazon VPC** | Rede virtual logicamente isolada. |
| **AWS Service Quotas** | Gerenciamento de limites de recursos da conta. |
| **Apache (httpd)** | Servidor Web de código aberto. |

---

> **Nota:** Este laboratório faz parte do currículo AWS Academy e demonstra práticas recomendadas de segurança (firewall restritivo) e automação (scripts de inicialização).