# HA de FTP

`vsftpd` en mode **actiu-passiu**: mateix servei, directori i configuració als dos nodes, amb connexió per la VIP.

## 1. Comprovar servei

Als dos servidors:

```bash
sudo systemctl status vsftpd
```

## 2. Sincronitzar directori FTP

Al `srv-primari`:

```bash
sudo rsync -av /srv/ftp/compartit/ root@192.168.0.101:/srv/ftp/compartit/
```

![Sincronització FTP](../assets/img/lab/image-29.png)

## 3. Copiar configuració

```bash
sudo rsync -av /etc/vsftpd.conf root@192.168.0.101:/etc/vsftpd.conf
```

![Còpia configuració FTP](../assets/img/lab/image-30.png)

![Verificació configuració](../assets/img/lab/image-31.png)

## 4. Reiniciar

Als dos servidors:

```bash
sudo systemctl restart vsftpd
```

## 5. Provar per la VIP

```bash
ftp 192.168.0.110
```
