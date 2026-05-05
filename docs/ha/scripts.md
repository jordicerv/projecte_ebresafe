# Scripts de Sincronització

Per donar sensació de muntatge seriós, es deixen scripts simples al primari.

## 1. Script web

Al `srv-primari`:

```bash
sudo nano /usr/local/bin/sync-web.sh
```

```bash
#!/bin/bash
rsync -av /var/www/html/ root@192.168.0.101:/var/www/html/
```

## 2. Script FTP

```bash
sudo nano /usr/local/bin/sync-ftp.sh
```

```bash
#!/bin/bash
rsync -av /srv/ftp/compartit/ root@192.168.0.101:/srv/ftp/compartit/
```

## 3. Script Samba

```bash
sudo nano /usr/local/bin/sync-samba.sh
```

```bash
#!/bin/bash
rsync -av /srv/samba/public/ root@192.168.0.101:/srv/samba/public/
```

## Donar permisos

Al `srv-primari`:

```bash
sudo chmod +x /usr/local/bin/sync-web.sh
sudo chmod +x /usr/local/bin/sync-ftp.sh
sudo chmod +x /usr/local/bin/sync-samba.sh
```

## Ús

```bash
# Sincronitzar web
sudo /usr/local/bin/sync-web.sh

# Sincronitzar FTP
sudo /usr/local/bin/sync-ftp.sh

# Sincronitzar Samba
sudo /usr/local/bin/sync-samba.sh
```

!!! tip "Automatització"
    Es podrien afegir a un `crontab` per executar-se periòdicament:

    ```bash
    sudo crontab -e
    ```

    ```cron
    */5 * * * * /usr/local/bin/sync-web.sh
    */5 * * * * /usr/local/bin/sync-ftp.sh
    */5 * * * * /usr/local/bin/sync-samba.sh
    ```
