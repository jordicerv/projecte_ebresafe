# Vulnerabilitats del Laboratori

Les vulnerabilitats configurades són **intencionades** per al laboratori d'auditoria i serveixen per demostrar les capacitats de l'eina.

## 1. SSH insegur

Als dos nodes:

```bash
sudo nano /etc/ssh/sshd_config
```

```text
PasswordAuthentication yes
PermitRootLogin yes
```

![Configuració SSH insegura](../assets/img/lab/image-10.png)

```bash
sudo systemctl restart ssh
```

**Deteccions esperades:** port `22` obert, servei `OpenSSH`, autenticació per contrasenya habilitada, hardening pobre.

## 2. Apache feble

Als dos nodes:

```bash
sudo nano /etc/apache2/apache2.conf
```

```apache
<Directory /var/www/>
    Options Indexes FollowSymLinks
    AllowOverride None
    Require all granted
</Directory>
```

```bash
sudo systemctl restart apache2
```

![Configuració Apache insegura](../assets/img/lab/image-11.png)

**Deteccions esperades:** port `80` obert, servidor `Apache`, headers de seguretat absents, directori navegable, fitxer sensible visible (`backup.sql.bak`).

## 3. MariaDB exposat

Als dos nodes:

```bash
sudo nano /etc/mysql/mariadb.conf.d/50-server.cnf
```

Canviar `bind-address = 127.0.0.1` per:

```text
bind-address = 0.0.0.0
```

![MariaDB exposat](../assets/img/lab/image-12.png)

```bash
sudo systemctl restart mariadb
```

**Deteccions esperades:** port `3306` obert, servei `MariaDB`, accés remot habilitat, usuari amb permisos excessius.

## Resum

| Servei | Vulnerabilitat | Risc |
|--------|---------------|------|
| SSH | `PasswordAuthentication yes`, `PermitRootLogin yes` | Alt |
| Apache | Directory listing, fitxers sensibles | Alt |
| MariaDB | `bind-address = 0.0.0.0`, usuari amb tots els permisos | Alt |
