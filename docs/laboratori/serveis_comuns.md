# Serveis Comuns

## Instal·lació als dos servidors

Executa això tant al primari com al secundari:

```bash
sudo apt update
sudo apt install -y openssh-server apache2 mariadb-server keepalived
```

![Instal·lació de serveis](../assets/img/lab/image-4.png)

Comprova serveis:

```bash
sudo systemctl enable ssh apache2 mariadb
sudo systemctl start ssh apache2 mariadb
sudo systemctl status ssh
sudo systemctl status apache2
sudo systemctl status mariadb
```

## Configuració comuna

### 1. Crear usuari de laboratori

Fes-ho als dos servidors:

```bash
sudo adduser auditor
echo 'auditor:auditor123' | sudo chpasswd
```

![Creació de l'usuari auditor](../assets/img/lab/image-5.png)

### 2. Crear una web igual als dos servidors

=== "srv-primari"

    ```bash
    echo "<h1>Srv Primari ASIX</h1>" | sudo tee /var/www/html/index.html
    sudo mkdir -p /var/www/html/uploads
    echo "fitxer de prova" | sudo tee /var/www/html/uploads/info.txt
    echo "copia de seguretat falsa" | sudo tee /var/www/html/backup.sql.bak
    ```

=== "srv-secundari"

    ```bash
    echo "<h1>Srv Secundari ASIX</h1>" | sudo tee /var/www/html/index.html
    sudo mkdir -p /var/www/html/uploads
    echo "fitxer de prova" | sudo tee /var/www/html/uploads/info.txt
    echo "copia de seguretat falsa" | sudo tee /var/www/html/backup.sql.bak
    ```

![Web primari](../assets/img/lab/image-6.png)

![Web secundari](../assets/img/lab/image-7.png)

!!! tip "Consell"
    No cal que el text sigui idèntic. De fet, va bé que sigui diferent perquè així es pot demostrar visualment el failover.

### 3. Crear una base de dades de prova

Fes-ho als dos servidors:

```bash
sudo mysql
```

```sql
CREATE DATABASE projecte;
CREATE USER 'projecte'@'%' IDENTIFIED BY '1234';
GRANT ALL PRIVILEGES ON projecte.* TO 'projecte'@'%';
FLUSH PRIVILEGES;
EXIT;
```

![Base de dades primari](../assets/img/lab/image-9.png)

![Base de dades secundari](../assets/img/lab/image-8.png)
