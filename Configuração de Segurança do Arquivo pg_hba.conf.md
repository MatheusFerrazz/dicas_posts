# Configuração de Segurança do Postgresql - pg_hba.conf
Se você assim como eu tem a necessidade de configurar a segurança do banco de dados Postgresql, seja para fins acadêmicos
ou profissionais essa dica pode ajudar. A estratégia tem o intuito de isolar as estruturas de banco, definindo privilégios
separadamente para grupos e usuários. Em outras palavras, apenas usuários com a devida permissão poderão acessar, modificar,
visualizar, criar e fazer qualquer tipo de alteração nos bancos.

O arquivo de configuração pg_hba.conf deve esta configurado o mais semelhante possível da ilustração abaixo, atentando sempre
aos seguintes detalhes: 
> * O conteúdo abaixo da linha **# Database administrative login by Unix domain socket** deve permanecer inalterado a não ser 
que você tenha garantia que o superusuário tenha acesso ao banco por outro método.  
> * As alterações devem ser feitas abaixo dessa linha comentada:  **# TYPE  DATABASE USER ADDRESS METHOD**. 
> * **ATENÇÃO!!** Para que funcione bem, devemos garantir que os usuários tenham acesso ao banco padrão postgres, e depois,
consequentemente, da permissão a outros bancos conforme a preferência. ***

`````
 78 # DO NOT DISABLE!
 79 # If you change this first entry you will need to make sure that the
 80 # database superuser can access the database using some other method.
 81 # Noninteractive access to all databases is required during automatic
 82 # maintenance (custom daily cronjobs, replication, and similar tasks).
 83 #
 84 # Database administrative login by Unix domain socket
 85 local   all             postgres                                peer
 86 
 87 # TYPE  DATABASE        USER            ADDRESS                 METHOD
 88 host      all           postgres        127.0.0.1/32            md5
 89 host      all           dba             127.0.0.1/32            md5
 90 host      postgres      programador1    127.0.0.1/32            md5
 91 host      bancoteste01  programador1    127.0.0.1/32            md5
 92 host      postgres      programador2    127.0.0.1/32            md5
 93 host      bancoteste01  programador2    127.0.0.1/32            md5

`````
Os usuários e o nome dos bancos são meramente representativos. Perceba que os usuários **postgres** e **dba** 
tem acesso a todos os bancos, *DATABASE* setados como *all*. Em contrapartida, os usuários **programdor1** e **programdor2**
tem acesso apenas ao *bancoteste01* que obrigatóriamente deve ter acesso ao banco padrão *postgres*. Essa simples dica foi 
crucial para mim e espero que seja útil para ti também. Após qualquer modificação no arquivo de configuração reinicie o 
serviço do *postgresql*.  
