# Objectius del Projecte

## Objectiu principal

Desenvolupar una eina d'auditoria de seguretat de xarxa portable i completa, amb un laboratori d'alta disponibilitat per demostrar-ne les capacitats.

## Objectius específics

### Funcionals

- [x] Descobriment d'hosts a la xarxa local (ping scan)
- [x] Escaneig de ports amb detecció de versions (nmap -sV)
- [x] Anàlisi de vulnerabilitats amb NSE (--script vuln)
- [x] Auditoria de configuració SSH (ssh-audit)
- [x] Enumeració de recursos SMB (enum4linux)
- [x] Recollida d'informació OSINT (theHarvester)
- [x] Generació d'informes HTML i JSON
- [x] Integració amb Telegram per a enviament d'informes

### Tècnics

- [x] Interfície gràfica amb tkinter (mode local)
- [x] Mode CLI interactiu per a Docker
- [x] Contenidor Docker portable per a USB
- [x] Laboratori amb dos servidors Ubuntu a Proxmox
- [x] Alta disponibilitat amb keepalived (VIP)
- [x] Replicació MariaDB master-slave
- [x] DNS master-slave amb bind9
- [x] Sincronització de serveis amb rsync

## Abast del projecte

```mermaid
graph LR
    subgraph Eina
        A[GUI tkinter] --> B[Backend Python]
        B --> C[nmap]
        B --> D[ssh-audit]
        B --> E[enum4linux]
        B --> F[theHarvester]
        B --> G[Telegram Bot]
    end
    subgraph Laboratori
        H[srv-primari] <-->|keepalived| I[srv-secundari]
        H & I --> J[VIP 192.168.0.110]
    end
    subgraph Distribució
        K[Docker] --> L[USB portable]
    end
    Eina -->|audita| Laboratori
    Eina -->|empaquetada en| Distribució
```

## Planificació

```mermaid
gantt
    title Planificació del Projecte EbreSafe
    dateFormat  YYYY-MM-DD
    section Fase 1 - Disseny
    Anàlisi de requisits           :a1, 2025-10-01, 14d
    Disseny de l'arquitectura      :a2, after a1, 14d
    section Fase 2 - Eina d'Auditoria
    Backend v1.0 (portscan)        :b1, after a2, 14d
    Backend v2.0 (+SSH)            :b2, after b1, 14d
    GUI tkinter v2.3               :b3, after b2, 14d
    Backend v3.0 (+SMB+Docker)     :b4, after b3, 21d
    section Fase 3 - Laboratori
    Muntatge servidors Proxmox     :c1, after b4, 14d
    Configuració HA keepalived     :c2, after c1, 14d
    HA de tots els serveis         :c3, after c2, 14d
    section Fase 4 - Docker & Docs
    Contenidor Docker              :d1, after c3, 14d
    Documentació MkDocs            :d2, after d1, 14d
```
