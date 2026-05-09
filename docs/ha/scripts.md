# Scripts de Sincronització

Scripts al `srv-primari` per sincronitzar contingut amb el secundari via `rsync`.

## Script web

```bash
sudo nano /usr/local/bin/sync-web.sh
```

```bash
#!/bin/bash
rsync -av /var/www/html/ root@192.168.0.101:/var/www/html/
```

## Script FTP

```bash
sudo nano /usr/local/bin/sync-ftp.sh
```

```bash
#!/bin/bash
rsync -av /srv/ftp/compartit/ root@192.168.0.101:/srv/ftp/compartit/
```

## Script Samba

```bash
sudo nano /usr/local/bin/sync-samba.sh
```

```bash
#!/bin/bash
rsync -av /srv/samba/public/ root@192.168.0.101:/srv/samba/public/
```

## Permisos

```bash
sudo chmod +x /usr/local/bin/sync-web.sh
sudo chmod +x /usr/local/bin/sync-ftp.sh
sudo chmod +x /usr/local/bin/sync-samba.sh
```

## Automatització amb crontab

Els scripts es poden programar per executar-se periòdicament:

```bash
sudo crontab -e
```

```cron
*/5 * * * * /usr/local/bin/sync-web.sh
*/5 * * * * /usr/local/bin/sync-ftp.sh
*/5 * * * * /usr/local/bin/sync-samba.sh
```
