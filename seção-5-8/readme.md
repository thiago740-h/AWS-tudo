# Otimização e Arquitetura de Computação na AWS

Este documento apresenta uma visão geral das principais opções de computação na AWS, estratégias de otimização de custos e critérios para seleção de arquitetura com base no equilíbrio entre controle e agilidade.

---

## 1. Estratégias de Eficiência Financeira no Amazon EC2
O Amazon EC2 é o pilar de processamento na nuvem da AWS, mas extrair o seu valor máximo exige uma estratégia constante de **Right Sizing** (ajuste ideal de recursos) para evitar desperdícios.

### Formatos de Contratação:
* **On-Demand (Sob Demanda):** Sem fidelidade ou contratos de longo prazo; a cobrança é feita pelo tempo exato de uso (segundos ou horas). É a escolha perfeita para demandas sazonais, testes ou fluxos de trabalho instáveis.
* **Instâncias Reservadas (RI):** Garante abatimentos de até 75% em troca de uma assinatura de uso de 1 a 3 anos para um padrão específico de máquina.
* **Savings Plans:** Proporcionam margens de economia equivalentes às das Instâncias Reservadas, porém com muito mais liberdade para alterar famílias de servidores ou regiões geográficas, baseando-se numa promessa de consumo financeiro horário ($/hora).
* **Instâncias Spot:** Permitem arrematar a infraestrutura excedente da AWS com reduções de preço que chegam a 90%. A contrapartida é que podem ser finalizadas pela AWS a qualquer momento, sendo indicadas apenas para processamentos resilientes a interrupções.

> 💡 **Recomendação Prática:** Adote o **AWS Compute Optimizer**. Esta ferramenta utiliza algoritmos de inteligência artificial para avaliar as suas métricas históricas de consumo e sugerir upgrades ou downgrades precisos de hardware.

---

## 2. Soluções de Orquestração de Contêineres
Os contêineres asseguram a portabilidade e a consistência dos sistemas em diferentes cenários. A AWS disponibiliza três abordagens principais para esta arquitetura:

* **Amazon ECS (Elastic Container Service):** A ferramenta proprietária da AWS para gestão de contêineres, desenhada para rodar de forma fluida e nativa com o restante ecossistema da provedora.
* **Amazon EKS (Elastic Kubernetes Service):** Plataforma gerenciada que simplifica a operação do Kubernetes, ideal para empresas que priorizam ferramentas de código aberto de padrão global.
* **AWS Fargate:** Tecnologia "Serverless" voltada para contêineres. Ao adotá-la, a preocupação com a administração de servidores virtuais desaparece, deixando o provisionamento e o suporte físico totalmente sob responsabilidade da AWS.

---

## 3. Conceitos Iniciais do AWS Lambda
O Lambda consolida o conceito de **Serverless** (computação sem servidor), permitindo que os desenvolvedores dediquem a sua atenção unicamente à escrita de suas funções.

* **Ativação sob Demanda (Event-Driven):** O processamento só inicia após um estímulo específico, como a chegada de um novo ficheiro no Amazon S3, atualizações em tabelas do DynamoDB ou chamadas de API.
* **Benefícios:** Ajuste automático de escala conforme o volume de requisições e cobrança restrita aos milissegundos de atividade do código. Se não houver chamadas, o custo é zero.
* **Ponto de Atenção:** Voltado para execuções rápidas e pontuais, possuindo um tempo limite de processamento (timeout) de até 15 minutos.

---

## 4. Facilitando Entregas com o AWS Elastic Beanstalk
Atuando no modelo de **PaaS** (Plataforma como Serviço), esta ferramenta viabiliza a publicação simplificada de portais e sistemas web.

* **Operação Automatizada:** Basta subir o código-fonte (em linguagens como Python, Node.js, Java ou .NET) que o Beanstalk cuida de toda a configuração do balanceamento de carga, políticas de escalabilidade e verificação de integridade da aplicação.
* **Flexibilidade Preservada:** Mesmo com a automação, o desenvolvedor não perde o acesso administrativo direto às máquinas virtuais subjacentes caso precise de fazer customizações profundas.

---

## 5. Tomada de Decisão: Como Escolher a Melhor Arquitetura?
A definição do serviço ideal é um exercício de ponderação entre o nível de autonomia desejado e o esforço de manutenção operacional:

| Modelo | Serviço AWS | Foco / Nível de Controle | Cenário Ideal |
| :--- | :--- | :--- | :--- |
| **IaaS** | Amazon EC2 | Controle total, desde o sistema operacional até à rede. | Migrações diretas (*lift-and-shift*) ou apps que exigem SO customizado. |
| **PaaS** | Elastic Beanstalk | Deploy ágil com automação da infraestrutura de suporte. | Aplicações web tradicionais com foco em desenvolvimento rápido. |
| **FaaS** | AWS Lambda | Foco total na lógica de negócio, sem gestão de servidores. | Microsserviços orientados a eventos e tarefas de curta duração. |
| **Serverless Containers** | AWS Fargate | Equilíbrio para microsserviços em contêineres sem gerir VMs. | Arquiteturas modernas de contêineres sem a sobrecarga de gerir o SO. |