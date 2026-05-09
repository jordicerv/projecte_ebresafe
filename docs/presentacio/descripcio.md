# Descripció del Projecte

## Introducció

EbreSafe és una eina multi-protocol d'auditoria de seguretat de xarxa amb interfície GUI (tkinter) i mode CLI per a Docker. Permet realitzar descobriment d'hosts, escaneig de ports, anàlisi de vulnerabilitats, auditoria SSH, enumeració SMB i recollida d'informació OSINT.

## Context

- **Centre educatiu:** Institut de l'Ebre
- **Cicle formatiu:** ASIX (Administració de Sistemes Informàtics en Xarxa)
- **Curs acadèmic:** 2025-2026
- **Mòduls implicats:** Seguretat informàtica, Serveis de xarxa, Implantació de sistemes operatius

## Problemàtica

En l'àmbit de l'administració de sistemes, és fonamental poder:

1. **Auditar la seguretat** dels serveis desplegats a una xarxa
2. **Garantir l'alta disponibilitat** dels serveis crítics
3. **Documentar vulnerabilitats** de manera professional i portable

No existia una eina integrada que permetés fer tot això de manera senzilla, portable (via USB amb Docker) i amb generació d'informes automàtics.

## Solució proposada

EbreSafe proporciona:

- :material-radar: **Descobriment de hosts** a la xarxa local
- :material-lan: **Escaneig de ports** amb detecció de versions
- :material-shield-alert: **Anàlisi de vulnerabilitats** amb Nmap Scripting Engine
- :material-ssh: **Auditoria SSH** de configuració
- :material-folder-network: **Enumeració SMB** de recursos compartits
- :material-earth: **OSINT** amb theHarvester
- :material-send: **Integració amb Telegram** per enviar informes
- :material-docker: **Portabilitat total** gràcies a Docker i distribució USB

Tot complementat amb un **laboratori d'alta disponibilitat** sobre Proxmox amb dos servidors Ubuntu.
