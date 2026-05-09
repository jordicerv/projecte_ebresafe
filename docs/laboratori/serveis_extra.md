# Serveis Extra

Serveis instal·lats al `srv-secundari` per ampliar la superfície d'auditoria:

```bash
sudo apt install -y bind9 vsftpd samba
```

## 1. DNS amb bind9

- `srv-primari`: DNS mestre
- `srv-secundari`: DNS esclau

Vulnerabilitats simulades: transferència de zona oberta i versió del servei visible.

## 2. FTP amb vsftpd

Vulnerabilitats simulades: accés anònim activat i carpeta pública amb fitxers de prova (`inventari.txt`, `copia.sql`).

## 3. Samba

Vulnerabilitats simulades: recurs compartit amb permisos massa amplis i lectura anònima (`backup.sql`, `usuaris.txt`, `config_old.conf`).

La configuració detallada d'alta disponibilitat d'aquests serveis es troba a la secció [Alta Disponibilitat](../ha/index.md).
