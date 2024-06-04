# ansible-project

Para a execução deste projeto, primeiramente deve ser criada uma máquina EC2 para a execução do servidor ansible. 
Criar a maquina do tipo [RedHat] com o nome [ansible-server] no security group [default].
Salvar seu par de chaves .pem para utilização
Não esqueça de ter certeza que o security group onde sua maquina ansible está possui a porta 22 aberta para realização da conexão

Após sua inicialização e conexão via SSH, realizar os passos de configuração a seguir:

Alterar nome da maquina

    $ hostnamectl set-hostname server1
   
Criar chave SSH

    ssh-keygen

gerado em
> /root/.ssh/id_rsa.pub

instalar python

    dnf install python -y

instalar ansible     

    dnf install ansible-core

instalar o git
    dnf install git

Realizar clone do repositorio
    https://github.com/anisioajr/ansible-project
de preferência no diretório ~/root/


executar install aws a partir do playbook a seguir:

    play1-aws-config.yml

Realizar a configuração da credential da aws no arquivo:

    /root/.aws/credentials ou ec2-user/.aws/credentials

Gerar a chave pública para compartilhamento com as demais instancias:

    ssh-keygen 

Configurar o playbook de criação das instancias colocando no step [Provisionar instância EC2 para o servidor web] o nome da chave pem no atributo [key_name], o nome de sua subnet no atributo [vpc_subnet_id] e a imagem de instancia desejada no atributo [image_id] podendo manter o que já está configurado por hora.

> play2

ir até o repositório clonado e executar o playbook para a criação das instancias 

    Play2
    Play4

Conectar nas instâncias e copiar o conteúdo da chave pública do servidor nas novas instâncias para permitir suas configurações remotas:
- visualizar o conteúdo

  > cat /root/.ssh/id_rsa.pub
  
- conectar na instancia destino via SSH

  > 
  
