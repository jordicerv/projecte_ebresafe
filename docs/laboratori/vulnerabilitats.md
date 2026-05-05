# Vulnerabilitats del Laboratori

!!! danger "Atenció"
    Les vulnerabilitats configurades en aquesta secció són **intencionades** per al laboratori d'auditoria. **Mai s'han d'aplicar en un entorn de producció.**

La idea és tenir vulnerabilitats senzilles, visibles i fàcils de justificar per demostrar les capacitats de l'eina d'auditoria.

## 1. SSH insegur

Edita als dos nodes:

```bash
sudo nano /etc/ssh/sshd_config
```

Deixa actiu:

```text
PasswordAuthentication yes
PermitRootLogin yes
```

![Configuració SSH insegura](../assets/img/lab/image-10.png)

Reinicia:

```bash
sudo systemctl restart ssh
```

!!! warning "Què hauria de detectar l'eina"
    - Port `22` obert
    - Servei `OpenSSH`
    - Autenticació per contrasenya habilitada
    - Hardening pobre

## 2. Apache feble

Edita als dos nodes:

```bash
sudo nano /etc/apache2/apache2.conf
```

Deixa la secció de `/var/www/` semblant a:

```apache
<Directory /var/www/>
    Options Indexes FollowSymLinks
    AllowOverride None
    Require all granted
</Directory>
```

Reinicia:

```bash
sudo systemctl restart apache2
```

![Configuració Apache insegura](../assets/img/lab/image-11.png)

!!! warning "Què hauria de detectar l'eina"
    - Port `80` obert
    - Servidor `Apache`
    - Headers de seguretat absents
    - Directori navegable
    - Fitxer sensible visible (`backup.sql.bak`)

## 3. MariaDB massa exposat

Edita als dos nodes:

```bash
sudo nano /etc/mysql/mariadb.conf.d/50-server.cnf
```

Canvia:

```text
bind-address = 127.0.0.1
```

per:

```text
bind-address = 0.0.0.0
```

![MariaDB exposat](../assets/img/lab/image-12.png)

Reinicia:

```bash
sudo systemctl restart mariadb
```

!!! warning "Què hauria de detectar l'eina"
    - Port `3306` obert
    - Servei `MariaDB`
    - Accés remot habilitat
    - Usuari amb permisos excessius

## Resum de vulnerabilitats

| Servei | Vulnerabilitat | Risc |
|--------|---------------|------|
| SSH | `PasswordAuthentication yes`, `PermitRootLogin yes` | :material-alert: Alt |
| Apache | Directory listing, fitxers sensibles | :material-alert: Alt |
| MariaDB | `bind-address = 0.0.0.0`, usuari amb tots els permisos | :material-alert: Alt |
