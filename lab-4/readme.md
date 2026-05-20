Laboratório 4: Como trabalhar com o Amazon EBS
Visão geral
Este laboratório aborda o Amazon Elastic Block Store (EBS), um serviço de armazenamento persistente utilizado com instâncias EC2.

Você irá aprender a:

Criar volumes EBS
Associá-los a instâncias EC2
Configurar sistemas de arquivos
Criar snapshots
Restaurar volumes
Objetivos
Ao final deste laboratório, você será capaz de:

Criar um volume EBS
Associar e montar em uma instância EC2
Criar snapshots
Restaurar volumes a partir de snapshots
Duração
Aproximadamente 30 minutos

Restrições
O ambiente possui permissões limitadas. Ações fora do escopo podem gerar erros.

O que é o Amazon EBS?
O Amazon EBS fornece armazenamento persistente para EC2.

Características:
Persistente (independente da instância)
Alto desempenho
Alta disponibilidade
Escalável (1 GB até 16 TB)
Suporte a snapshots (backup no S3)
Acesso ao console AWS
Clique em Iniciar laboratório
Aguarde o status ficar ativo
Abra o console AWS
Permita pop-ups se necessário
Tarefa 1: Criar volume EBS
Acesse EC2 → Volumes
Clique em Criar volume
Configure:
Tipo: gp2
Tamanho: 1 GiB
Zona: mesma da instância
Adicione tag:
Name: My Volume
Clique em Criar volume
Tarefa 2: Associar volume
Selecione o volume
Clique em Ações → Anexar volume
Escolha instância Lab
Dispositivo: /dev/sdf
Clique em Associar
Tarefa 3: Conectar na instância
Vá em Instâncias
Selecione Lab
Clique em Conectar
Use EC2 Instance Connect
Tarefa 4: Configurar sistema de arquivos
Verificar discos:
df -h