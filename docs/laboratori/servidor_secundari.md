# Servidor Secundari

## Creació del servidor

Crea una segona Ubuntu Server amb la mateixa xarxa que el primari.

| Paràmetre | Valor |
|-----------|-------|
| Nom | `srv-secundari` |
| IP | `192.168.0.101` |
| Subxarxa | Mateixa que el primari |

Comprovacions bàsiques:

```bash
ip a
hostnamectl
ping 192.168.0.100
```

Posa-li el nom:

```bash
sudo hostnamectl set-hostname srv-secundari
```

## Si el secundari és un clon de Proxmox

!!! warning "Atenció"
    Si clones la VM del primari, normalment Proxmox generarà una MAC nova per a la targeta de xarxa del clon. Tot i això, cal revisar la configuració interna d'Ubuntu abans d'arrencar els dos servidors alhora.

**Ordre recomanat:**

1. Clonar la VM
2. Comprovar a Proxmox que la MAC del clon és diferent
3. Arrencar només el clon
4. Canviar IP, hostname i fitxer `hosts`
5. Revisar `netplan`
6. Regenerar claus SSH

### 1. Comprovar la MAC del clon

A Proxmox:

- Entra a la VM clonada
- Ves a `Hardware`
- Entra a `Network Device`
- Comprova que la MAC no sigui la mateixa que la del primari

### 2. Canviar hostname

Al clon:

```bash
sudo hostnamectl set-hostname srv-secundari
hostnamectl
```

![Canvi de hostname](../assets/img/lab/image.png)

### 3. Canviar la IP

Edita el fitxer de `netplan`:

```bash
sudo nano /etc/netplan/00-installer-config.yaml
```

Configuració per al secundari:

```yaml
network:
  version: 2
  ethernets:
    ens18:
      dhcp4: no
      addresses:
        - 192.168.0.101/24
      nameservers:
        addresses:
          - 8.8.8.8
      routes:
        - to: default
          via: 192.168.0.1
```

![Configuració netplan](../assets/img/lab/image-1.png)

Aplica canvis:

```bash
sudo chmod 600 /etc/netplan/00-installer-config.yaml
sudo netplan apply
ip a
```

!!! tip "Resolució de problemes amb netplan"
    Si `netplan apply` dona error, comprova si tens més d'un fitxer YAML configurant la mateixa interfície:

    ```bash
    ls -l /etc/netplan/
    sudo grep -R "ens18\\|gateway4\\|routes:" /etc/netplan
    ```

    Si hi ha dos fitxers definint `ens18` (per exemple `00-installer-config.yaml` i `50-cloud-init.yaml`), cal deixar-ne només un:

    ```bash
    sudo mv /etc/netplan/50-cloud-init.yaml /etc/netplan/50-cloud-init.yaml.bak
    sudo chmod 600 /etc/netplan/00-installer-config.yaml
    sudo netplan generate
    sudo netplan apply
    ```

    Per evitar que `cloud-init` regeneri la xarxa:

    ```bash
    echo 'network: {config: disabled}' | sudo tee /etc/cloud/cloud.cfg.d/99-disable-network-config.cfg
    ```

### 4. Revisar si netplan està lligat a una MAC

Algunes configuracions poden tenir:

```yaml
match:
  macaddress: aa:bb:cc:dd:ee:ff
```

Si hi surt la MAC antiga del primari, canvia-la o elimina el bloc `match`.

### 5. Actualitzar `/etc/hosts`

```bash
sudo nano /etc/hosts
```

```text
127.0.0.1 localhost
127.0.1.1 srv-secundari
192.168.0.100 srv-primari
192.168.0.101 srv-secundari
```

![Fitxer hosts](../assets/img/lab/image-2.png)

### 6. Regenerar claus SSH del clon

!!! note "Opcional però recomanable"
    Per tenir dues màquines realment independents.

```bash
sudo rm -f /etc/ssh/ssh_host_*
sudo dpkg-reconfigure openssh-server
sudo systemctl restart ssh
```

![Regeneració claus SSH](../assets/img/lab/image-3.png)

### 7. Comprovacions finals del clon

```bash
hostnamectl
ip a
ip route
ping 192.168.0.100
ssh auditor@192.168.0.100
```

Quan això funcioni, ja es poden arrencar alhora el primari i el secundari sense risc de conflicte de xarxa.
