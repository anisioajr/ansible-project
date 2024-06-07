# ansible-project

Para a execução deste projeto, primeiramente deve ser criada uma máquina EC2 para a execução do servidor ansible. 
Criar a maquina do tipo [RedHat] com o nome [ansible-server] no security group [default].
Salvar seu par de chaves .pem para utilização
Não esqueça de ter certeza que o security group onde sua maquina ansible está possui a porta 22 aberta para realização da conexão

Após sua inicialização e conexão via SSH, realizar os passos de configuração a seguir:

Alterar nome da maquina (opcional)

    $ hostnamectl set-hostname server1
   

gerado em
> /root/.ssh/id_rsa.pub

instalar python

    dnf install python -y

instalar ansible     

    dnf install ansible-core

instalar o git

    dnf install git

>> configurar o git
>> 

Realizar clone do repositorio
    https://github.com/anisioajr/ansible-project
de preferência no diretório /root/


executar configuração aws e do servidor atual a partir do playbook a seguir copiado no diretorio ansible-project:

    play1-aws-config.yml

Realizar a configuração da credential da aws no arquivo:

    /root/.aws/credentials ou ec2-user/.aws/credentials

Gerar a chave pública para compartilhamento com as demais instancias:

    ssh-keygen 

Configurar o playbook de criação das instancias colocando no step [Provisionar instância EC2 para o servidor web] o nome da chave pem no atributo [key_name], o nome de sua subnet no atributo [vpc_subnet_id] e a imagem de instancia desejada no atributo [image_id] podendo manter o que já está configurado por hora.

Colocar o arquivo .pem no qual utilizará para a criação da instância utilizando o editor de texto e colando o conteúdo no arquivo. Ex:

    vi 0406key.pem

Ir até o repositório clonado e executar o playbook para a criação das instancias
Executar o playbook para criação da instancia e inclusão no arquivo de hosts

    play2-aws-ec2.yml


Conectar na instância e copiar o conteúdo da chave pública do servidor nas novas instâncias para permitir suas configurações remotas:
- visualizar o conteúdo

  > cat /root/.ssh/id_rsa.pub
  
conectar na instancia destino via SSH, exemplo

    ssh -i "0406key.pem" ec2-user@ec2-18-229-148-27.sa-east-1.compute.amazonaws.com

- ir até o arquivo

  > /root/.ssh/authorized_keys
  
Colar o conteúdo da chave id_rsa.pub, salvar e retornar à sua instância ansible.
  
Realizar  verificação de conexão na instância criada com o comando

    ansible web-server-1 -m ping

Se retornar erro significa que a maquina está disponível e a configuração está finalizada

Executar o playbook para criação da instancia RDS e inclusão no arquivo de hosts

    play4-aws-rds.yml

Caso queira testar a conexão, execute o playbook a seguir:

    play4-aws-teste-rds.yml

 Agora vamos fazer o deploy da página estática e configuração do servidor executando os playbooks a seguir:

     play5-aws-webconfig.yml
     play6-app-web-deploy.yml













  
