### Configuracão

- Baixar o Ubuntu Server no link (https://www.ubuntu.com/download/server) - tutorial feito no Ubuntu 16.4

- Instalar o OpenSSH

```
sudo apt-get install openssh-server
```
- Instalar o Proftp

```
sudo apt-get install proftpd
```
- Fazer configuração do ftp:
```
sudo nano /etc/proftpd/proftpd.conf
```
-Alterar o ServerName para o nome do seu servidor, no caso usamos "FTP Bilac"

Descomentar a linha  - DefaultRoot                     ~
Descomentar a linha  - RequireValidShell               off

- Instalar Apache
```
apt-get update
apt-get install apache2
```
- Instalando MySQL
```
sudo apt-get update
sudo apt-get install mysql-server
mysql_secure_installation
```
Ao realizar esta instalação irá pedir para você colocar uma senha de root para o mysql.

- Instalando PHP
```
apt-get install apache2 php7.0-common php7.0-cli php7.0 libapache2-mod-php7.0
```
## Configurando usuários

#### Para adicionar novos usuarios com FTP é necessario adicionar seu path no /var/www
```
sudo useradd -s /bin/false -m -d /var/www/username username
```
Configurar a senha para seu usuario:
```
su - username
passwd username
```

#### Configurando hosts no Apache

**Verificando path do html apache2**
```
sudo nano /etc/apache2/apache2.conf
```

Verificar se existe
```
<Directory /var/www>
...
</Directory>
```

Por padrão vamos utilizar o path /var/www

### Configurando sites-available

Para configurar os sites available é necessario fazer uma cópia do 000-default.conf presente em:

/etc/apache2/sites-available/000-default.conf

Vamos presupor que nosso usuário chama: valentino

```
sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/valentino.conf
```

Agora para realizar a configuração nesse arquivo iremos mudar as seguintes configuracões:

-Descomentar #ServerName e colocar o possivel nome do host que será chamado no payload de cada requisição (www.valentino.com)

-Mudar DocumentRoot /var/www/valentino(ou nome do home path de seu usuário)

Agora precisamos fazer um link desse arquivo para /etc/apache2/sites-enabled

```
sudo 2ensite valentino.conf
```

### Pronto!

###Lado do cliente

Agora é só enviar seus arquivos via FTP:

```
ftp www.valentino.com
put seufile.extensao
```

**Antes disso deverá configurar seus hosts para apontar para esse ip se necessário**

sudo /etc/hosts;

e fazer algo como:
172.17.5.67     www.valentino.com


## Script de conexao PHP com BD se necessário
```PHP
<?php
try{
  $conn =  new PDO("mysql:host=localhost;dbname=valentino", "root", "toor");
  $stmt = $conn->query("SELECT * FROM users");
  $res = $stmt->fetchAll();
  print_r($res);
}catch(Exception $e){
  echo "Erro na conexao";
}
?>
```
