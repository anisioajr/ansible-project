# ansible-project

Para a execução deste projeto, primeiramente deve ser criada uma máquina EC2 para a execução do servidor ansible. 
Criar a maquina do tipo [RedHat] com o nome [ansible-server] no security group existente [default] na região [us-east-1].
Salvar seu par de chaves .pem para utilização
Não esqueça de ter certeza que o security group onde sua maquina ansible está possui a porta 22 aberta para realização da conexão

Após sua inicialização realizar conexão via SSH a partir do cloud shell, fazendo upload e dando permissão [$ chmod 400 "0606key.pem"] , antes, do arquivo PEM criado com a instancia e realizar os passos de configuração a seguir:

Alterar nome da maquina (opcional)

    $ hostnamectl set-hostname server1
   
Gerar a chave ssh publica para compartilhamento posterior 

    sudo su
    ssh-keygen
que será gerado em
> /root/.ssh/id_rsa.pub

instalar python

    sudo dnf install python -y

instalar ansible     

    dnf install ansible-core

instalar o git

    dnf install git

>> configurar o git

    git config --global user.name "Seu Nome"
    git config --global user.email "seu_email@example.com"
>> 

Realizar clone do repositorio

    https://github.com/anisioajr/ansible-project
 no diretório /root/


Entrar no diretorio colnado e executar configuração aws e do servidor atual a partir do playbook a seguir copiado no diretorio ansible-project:

    play1-aws-config.yml

Realizar a configuração (como ec2-user) da credential da aws no arquivo:

    aws configure

inserindo seu access key, seu secret-key e definindo a region default como [us-east-1].
Caso você tenha os dados da aws lab canvas, pode adicionar diretamente no repositório

    /root/.aws/credentials ou ec2-user/.aws/credentials

Configurar o [play2-aws-ec2.yml] de criação das instancias colocando no step [Provisionar instância EC2 para o servidor web] o nome da chave pem no atributo [key_name], o nome de sua subnet no atributo [vpc_subnet_id] e a imagem de instancia desejada no atributo [image_id] podendo manter o que já está configurado por hora.

    play2-aws-ec2.yml

~Colocar o arquivo .pem no qual utilizará para a criação da instância utilizando o editor de texto e colando o conteúdo no arquivo. Ex:~

    ~vi 0606key.pem~


~Conectar na instância via ssh em um novo terminal e copiar o conteúdo da chave pública do servidor nas novas instâncias para permitir suas configurações remotas:~
- ~visualizar o conteúdo no servidor ansible~

  > ~cat /root/.ssh/id_rsa.pub~
  
~conectar na instancia destino via SSH, exemplo~

    ~ssh -i "0406key.pem" ec2-user@ec2-18-229-148-27.sa-east-1.compute.amazonaws.com~

- ~ir até o arquivo para edicao~ 

  > ~vi /root/.ssh/authorized_keys~
  
~Copiar a chave do servidor ansible e colar o conteúdo da chave id_rsa.pub, salvar e retornar à sua instância ansible~.
  
Realizar  verificação de conexão na instância criada com o comando a partir da maquina do ansible

    ansible web-server-1 -m ping

Se retornar erro significa que a maquina está disponível e a configuração está finalizada

Executar o playbook para criação da instancia RDS e inclusão no arquivo de hosts

    play4-aws-rds.yml

Caso queira testar a conexão, execute o playbook a seguir alterando o login_host para o DSN da sua instancia RDS criada:

    play4-aws-teste-rds.yml

 Agora vamos fazer o deploy da página estática e configuração do servidor executando os playbooks a seguir:

     play5-aws-webconfig.yml
     play6-app-web-deploy.yml

Vá até o repositório e crie um webhook apontando para o DNS publico da sua instância ansible-server criada

    https://github.com/anisioajr/web-app-ajr/settings/hooks
    http://<<DNS-IPV4PUB>>/webhook   ===== exemplo:
    http://ec2-35-169-124-79.compute-1.amazonaws.com/webhook












  
