# Resumo de Estudos AWS - Módulos 1, 2 e 3

Este documento consolida os conceitos fundamentais abordados nos módulos iniciais do curso AWS Academy, focando em conceitos de nuvem, economia e infraestrutura global.

---

## ☁️ Módulo 1: Visão Geral dos Conceitos de Nuvem

Este módulo foca nas vantagens da transição do modelo tradicional (*on-premises*) para a nuvem.

### Vantagens da Computação em Nuvem
* **Trocar despesas de capital (CapEx) por despesas variáveis (OpEx):** Paga apenas pelo que consome.
* **Economias de escala:** Custos mais baixos devido ao grande volume de clientes da AWS.
* **Parar de adivinhar a capacidade:** Escala recursos de acordo com a necessidade real.
* **Aumentar velocidade e agilidade:** Recursos de TI disponíveis em minutos.
* **Alcance global em minutos:** Implementação em diversas regiões do mundo rapidamente.

### Modelos de Serviço de Computação em Nuvem
1.  **IaaS (Infraestrutura como Serviço):** Blocos de construção básicos (servidores, rede, armazenamento).
2.  **PaaS (Plataforma como Serviço):** Gerenciamento do hardware e SO pela AWS; foco na aplicação.
3.  **SaaS (Software como Serviço):** Produto completo gerenciado pelo provedor (ex: Gmail).

---

## 💰 Módulo 2: Economia e Faturamento da Nuvem

Explica como a AWS estrutura os seus custos e a responsabilidade sobre o hardware.

* **Modelo Pay-as-you-go:** Pagamento conforme o uso, sem investimentos iniciais pesados.
* **Responsabilidade de Hardware:** A propriedade e manutenção física dos servidores são 100% da AWS.
* **Serviços de Gerenciamento Gratuitos:** Serviços como **IAM, VPC, CloudFormation e Auto Scaling** não têm custo de uso, mas você paga pelos recursos (ex: instâncias EC2) que eles criam.

---

## 🌍 Módulo 3: Infraestrutura Global da AWS

Como a AWS organiza os seus recursos físicos para garantir alta disponibilidade e baixa latência.

### Componentes da Infraestrutura
* **Regiões:** Áreas geográficas isoladas que contêm duas ou mais Zonas de Disponibilidade.
* **Zonas de Disponibilidade (AZs):** Um ou mais data centers isolados.
    * Projetadas para isolamento de falhas.
    * Conectadas por links privados de alta velocidade.
    * *Nota: Um data center físico nunca é compartilhado entre AZs diferentes.*
* **Pontos de Presença (Edge Locations):** Utilizados pelo **Amazon CloudFront** para cache de conteúdo e redução de latência.

### Categorias de Serviços Fundamentais
1.  **Computação** (ex: EC2, Lambda)
2.  **Armazenamento** (ex: S3, EBS)
3.  **Redes** (ex: VPC, Route 53)
4.  **Bancos de Dados** (ex: RDS, DynamoDB)