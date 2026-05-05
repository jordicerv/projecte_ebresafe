# Serveis Extra

Aquests serveis donen més superfície d'auditoria, però no són la part principal de l'HA.

Instal·la al `srv-secundari`:

```bash
sudo apt install -y bind9 vsftpd samba
```

## 1. DNS amb bind9

!!! info "Idea recomanada"
    - `srv-primari`: DNS mestre
    - `srv-secundari`: DNS esclau

**Vulnerabilitats que es poden simular:**

- Transferència de zona oberta
- Versió del servei visible

## 2. FTP amb vsftpd

**Vulnerabilitats recomanades:**

- Accés anònim activat
- Carpeta pública amb fitxers de prova

Exemples de fitxers:

- `inventari.txt`
- `copia.sql`

## 3. Samba

**Vulnerabilitats recomanades:**

- Recurs compartit amb permisos massa amplis
- Lectura anònima dins del laboratori

Exemples de fitxers:

- `backup.sql`
- `usuaris.txt`
- `config_old.conf`

!!! tip "Consell"
    La configuració detallada d'alta disponibilitat d'aquests serveis es troba a la secció [Alta Disponibilitat](../ha/index.md).
