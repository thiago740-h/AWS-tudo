# Laboratório AWS: Criação de VPC Personalizada e Implantação de Servidor Web no Amazon EC2

Este repositório contém o roteiro de execução e as anotações teóricas do laboratório prático de arquitetura AWS. O objetivo deste exercício é construir uma rede virtual personalizada e altamente disponível, configurando sub-redes públicas e privadas em múltiplas Zonas de Disponibilidade (AZs), além de provisionar um servidor web automatizado via instâncias EC2.

---

## 📘 Anotações Teóricas (Conceitos-Chave)

Antes de iniciar o percurso prático, é fundamental compreender o papel de cada componente provisionado na infraestrutura:

* **Amazon VPC (Virtual Private Cloud):** Uma rede virtual isolada logicamente na nuvem AWS, onde você define o esquema de endereçamento IP (CIDR), sub-redes, tabelas de rotas e gateways. Ela replica o ambiente de um data center tradicional com a escalabilidade da AWS.
* **Sub-rede Pública:** Zona da rede associada a uma tabela de rotas com destino direto a um Gateway de Internet (IGW). Recursos aqui inseridos (como balanceadores de carga ou bastion hosts) podem receber tráfego externo.
* **Sub-rede Privada:** Zona isolada da internet direta. Recursos aqui dentro (como bancos de dados) só acessam a internet para atualizações de forma unidirecional, utilizando um Gateway NAT.
* **Internet Gateway (IGW):** O componente de VPC que viabiliza a comunicação bidirecional entre os recursos da VPC pública e a internet.
* **NAT Gateway (Network Address Translation):** Serviço gerenciado implantado na sub-rede pública que permite que instâncias da sub-rede privada realizem conexões de saída para a internet, mas impede que a internet inicie conexões diretamente com elas.
* **Security Groups (Grupos de Segurança):** Atuam como firewalls virtuais a nível de instância (instância-stateful), controlando o tráfego de entrada (Inbound) e saída (Outbound).
* **User Data (Dados do Usuário):** Scripts de inicialização executados automaticamente com privilégios de administrador (`root`) no primeiro boot da instância EC2, ideais para automatizar a instalação de softwares e configurações.

---

## 🗺️ Percurso do Laboratório

### Pré-requisitos e Acesso
1. No console do laboratório, clique em **Iniciar laboratório** (atente-se ao cronômetro de duração).
2. Aguarde o indicador de status da AWS ficar verde.
3. Clique no link da AWS para abrir o **Console de Gerenciamento** em uma nova guia.
4. Certifique-se de que a região selecionada no canto superior direito é **Norte da Virgínia (us-east-1)**.

---

### Tarefa 1: Criação da Estrutura Base da VPC
Nesta etapa, foi utilizada a funcionalidade "VPC e muito mais" para acelerar o provisionamento dos componentes base em uma Zona de Disponibilidade (us-east-1a).

1. No console da AWS, busque por **VPC** e clique em **Criar VPC**.
2. Configure o painel:
   * Selecione **VPC e muito mais**.
   * **Geração automática da etiqueta de nome:** Defina o sufixo/projeto como `lab`.
   * **Bloco CIDR IPv4:** `10.0.0.0/16`.
   * **Número de Zonas de Disponibilidade (AZs):** `1`.
   * **Número de sub-redes públicas:** `1`.
   * **Número de sub-redes privadas:** `1`.
3. Expanda **Personalizar blocos CIDR de sub-redes** e ajuste:
   * Sub-rede pública em us-east-1a: `10.0.0.0/24`
   * Sub-rede privada em us-east-1a: `10.0.1.0/24`
4. **Gateways NAT:** Selecionar `Em 1 AZ`.
5. **Endpoints da VPC:** `Nenhum`.
6. Valide a visualização gráfica gerada pelo console (`lab-vpc`, `lab-subnet-public1`, `lab-subnet-private1`, `lab-rtb-public`, `lab-rtb-private1`, `lab-igw`, `lab-nat`) e clique em **Criar VPC**. Aguarde a finalização.

---

### Tarefa 2: Expansão da Rede para Alta Disponibilidade (Multi-AZ)
Para garantir resiliência, adicionamos sub-redes extras em uma segunda Zona de Disponibilidade (`us-east-1b`) e ajustamos os roteamentos.

#### Criação das novas sub-redes:
1. No menu esquerdo, acesse **Sub-redes** e clique em **Criar sub-rede**.
2. **Sub-rede Pública 2:**
   * VPC ID: `lab-vpc`
   * Nome: `lab-subnet-public2`
   * Zona de Disponibilidade: `us-east-1b`
   * Bloco CIDR IPv4: `10.0.2.0/24`
3. **Sub-rede Privada 2:**
   * VPC ID: `lab-vpc`
   * Nome: `lab-subnet-private2`
   * Zona de Disponibilidade: `us-east-1b`
   * Bloco CIDR IPv4: `10.0.3.0/24`

#### Associação das Tabelas de Rotas:
1. Acesse **Tabelas de rotas** no menu lateral.
2. Selecione a tabela de rotas privada (`lab-rtb-private1-us-east-1a`):
   * Vá na aba **Associações de sub-rede** -> **Editar associações de sub-rede**.
   * Mantenha a original selecionada e marque também a nova `lab-subnet-private2`. Salve.
3. Selecione a tabela de rotas pública (`lab-rtb-public`):
   * Vá na aba **Associações de sub-rede** -> **Editar associações de sub-rede**.
   * Mantenha a original selecionada e marque também a nova `lab-subnet-public2`. Salve.

---

### Tarefa 3: Configuração do Firewall (Security Group)
Criação do grupo de segurança que permitirá o acesso externo via protocolo HTTP ao nosso futuro servidor web.

1. No menu esquerdo, clique em **Grupos de segurança** -> **Criar grupo de segurança**.
2. Preencha os dados básicos:
   * **Nome:** `Web Security Group`
   * **Descrição:** `Enable HTTP access`
   * **VPC:** Remova a padrão clicando no "X" e selecione a `lab-vpc`.
3. Em **Regras de entrada (Inbound)**, adicione a seguinte regra:
   * **Tipo:** `HTTP`
   * **Origem:** `Anywhere-IPv4` (`0.0.0.0/0`)
   * **Descrição:** `Permit web requests`
4. Clique em **Criar grupo de segurança**.

---

### Tarefa 4: Provisionamento Automatizado do Servidor Web (EC2)
Lançamento da máquina virtual na sub-rede pública com script automatizado de instalação do Apache e PHP.

1. Navegue até o serviço **EC2** usando a barra de pesquisas superior.
2. No painel, clique em **Executar instância**.
3. **Nome e etiquetas:** `Web Server 1`.
4. **Imagens de aplicativo e de sistema operacional (AMI):** Padrão `Amazon Linux` (Amazon Linux 2023 AMI).
5. **Tipo de instância:** Padrão `t2.micro`.
6. **Par de chaves:** Selecione `vockey`.
7. **Configurações de rede** (Clique em **Editar**):
   * Rede: `lab-vpc`
   * Sub-rede: **`lab-subnet-public2`** *(Atenção: certifique-se de escolher a pública)*
   * Atribuir IP público automaticamente: `Ativar`
   * Firewall: Marque `Selecionar grupo de segurança existente` e escolha o **`Web Security Group`**.
8. **Configurar armazenamento:** Manter o padrão (8 GiB SSD gp3).
9. **Dados do usuário (User Data):** Expanda a seção **Detalhes avançados**, role até o final da página e cole o script abaixo:

```bash
#!/bin/bash
# Instala o Servidor Web Apache, PHP e dependências
dnf install -y httpd wget php mariadb105-server
# Baixa os arquivos da aplicação do laboratório
wget [https://aws-tc-largeobjects.s3.us-west-2.amazonaws.com/CUR-TF-100-ACCLFO-2/2-lab2-vpc/s3/lab-app.zip](https://aws-tc-largeobjects.s3.us-west-2.amazonaws.com/CUR-TF-100-ACCLFO-2/2-lab2-vpc/s3/lab-app.zip)
unzip lab-app.zip -d /var/www/html/
# Configura o Apache para iniciar junto com o sistema e liga o serviço
chkconfig httpd on
service httpd start