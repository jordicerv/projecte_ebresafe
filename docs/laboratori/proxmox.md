# Entorn Proxmox

## 1. Descàrrega i preparació

Proxmox VE 9.1 es descarrega des del [web oficial](https://www.proxmox.com/en/downloads). Un cop descarregat, es crea un pendrive d'arrencada amb Rufus (Windows) o `dd` (Linux/macOS).

![Descàrrega de Proxmox VE 9.1](../assets/img/proxmox/proxmox-01.png)

## 2. Instal·lació

### Arrencada des del pendrive

Arrenquem des del pendrive i seleccionem **Install Proxmox VE (Graphical)**.

![Pantalla d'arrencada de Proxmox](../assets/img/proxmox/proxmox-02.png)

### Selecció del disc

Seleccionem el disc on s'instal·larà Proxmox. Tot el contingut del disc s'eliminarà.

![Selecció del disc d'instal·lació](../assets/img/proxmox/proxmox-03.png)

### Localització i teclat

Configuració de país, zona horària i distribució de teclat.

![Configuració de localització](../assets/img/proxmox/proxmox-04.png)

### Contrasenya i correu

Contrasenya de l'usuari `root` i correu de l'administrador.

![Contrasenya i correu](../assets/img/proxmox/proxmox-05.png)

### Configuració de xarxa

Interfície de gestió amb IP estàtica, gateway i DNS.

![Configuració de xarxa](../assets/img/proxmox/proxmox-06.png)

### Resum i instal·lació

Revisió de la configuració final. Cliquem **Install** per començar.

![Resum de la instal·lació](../assets/img/proxmox/proxmox-07.png)

## 3. Configuració inicial

Accedim a la interfície web de Proxmox a `https://192.168.0.80:8006` i obrim la Shell del node.

### Configuració de xarxa

Editem `/etc/network/interfaces` per assignar IP estàtica al bridge `vmbr0`:

```bash
nano /etc/network/interfaces
```

```text
auto vmbr0
iface vmbr0 inet static
        address 192.168.0.80/24
        gateway 192.168.0.1
        bridge-ports nic0
        bridge-stp off
        bridge-fd 0
```

```bash
ifreload -a
```

![Configuració de xarxa del node](../assets/img/proxmox/proxmox-08.png)

### Hostname

```bash
nano /etc/hostname
nano /etc/hosts
# 192.168.0.80    proxmox.ebresafe.org proxmox
hostnamectl set-hostname proxmox.ebresafe.org
```

![Configuració del hostname](../assets/img/proxmox/proxmox-09.png)

## 4. Containers LXC

Per a serveis lleugers (VPN-Tailscale, AdGuard, chat, multimèdia), utilitzem containers LXC que consumeixen menys recursos que les VMs.

![Containers LXC al panell de Proxmox](../assets/img/proxmox/proxmox-10.png)

### Creació d'un container LXC

Des de la barra superior, botó **Create CT**:

![Botó Create CT](../assets/img/proxmox/proxmox-11.png)

**General** — CT ID, hostname i contrasenya root:

![Pestanya General](../assets/img/proxmox/proxmox-12.png)

**Template** — Selecció de la plantilla (Debian 12 Standard):

![Pestanya Template](../assets/img/proxmox/proxmox-13.png)

**Disks** — Mida del disc root (4-8 GiB per serveis lleugers):

![Pestanya Disks](../assets/img/proxmox/proxmox-14.png)

**CPU** — Nuclis assignats (1 core per serveis bàsics):

![Pestanya CPU](../assets/img/proxmox/proxmox-15.png)

**Memory** — RAM i swap (512 MiB cadascun com a base):

![Pestanya Memory](../assets/img/proxmox/proxmox-16.png)

**Network** — IP estàtica al bridge `vmbr0`:

![Pestanya Network](../assets/img/proxmox/proxmox-17.png)

**DNS** — Configuració DNS (per defecte hereta del host):

![Pestanya DNS](../assets/img/proxmox/proxmox-18.png)

**Confirm** — Resum i creació:

![Confirmació i creació del container](../assets/img/proxmox/proxmox-19.png)

## 5. Màquines Virtuals del Projecte

Les VMs **105 (ServidorPrimari)** i **106 (ServidorSecundari)** són les màquines principals del laboratori, configurades amb Ubuntu Server per als serveis d'alta disponibilitat.

![VMs del projecte a Proxmox — container de prova](../assets/img/proxmox/proxmox-20.png)

![VMs 105 i 106 — ServidorPrimari i ServidorSecundari](../assets/img/proxmox/proxmox-21.png)

### Paràmetres de les VMs

| Paràmetre | Valor |
|-----------|-------|
| BIOS | SeaBIOS |
| Màquina | q35 |
| CPU | 2 vCPU |
| RAM | 2 GB |
| Disc | VirtIO SCSI, 20 GB |
| Xarxa | VirtIO, bridge `vmbr0` |
| SO | Ubuntu Server 22.04 LTS |

### IPs assignades

| VM | Nom | IP |
|----|-----|----|
| 105 | ServidorPrimari | `192.168.0.100` |
| 106 | ServidorSecundari | `192.168.0.101` |
| — | **VIP** (keepalived) | `192.168.0.110` |

## 6. Post-instal·lació

```bash
# Actualitzar el sistema
apt-get update && apt-get dist-upgrade -y

# Repositori no-subscription (si no hi ha llicència)
rm /etc/apt/sources.list.d/pve-enterprise.list 2>/dev/null
echo 'deb http://download.proxmox.com/debian/pve bookworm pve-no-subscription' \
  > /etc/apt/sources.list.d/pve-no-subscription.list
apt-get update
```

## Xarxa del laboratori

```mermaid
graph LR
    subgraph "Proxmox Host (192.168.0.80)"
        subgraph "vmbr0 (Bridge)"
            P["VM 105 — srv-primari<br>192.168.0.100"]
            S["VM 106 — srv-secundari<br>192.168.0.101"]
        end
    end
    R["Router / Gateway<br>192.168.0.1"] --- vmbr0
    VIP["VIP: 192.168.0.110"] -.- P
    VIP -.- S
```
