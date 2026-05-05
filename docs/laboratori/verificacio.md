# Verificació del Laboratori

## Escaneig dels dos nodes

```bash
nmap -sV 192.168.0.100
nmap -sV 192.168.0.101
```

## Escaneig de la IP virtual

```bash
nmap -sV 192.168.0.110
```

## Proves manuals útils

```bash
curl http://192.168.0.100
curl http://192.168.0.101
curl http://192.168.0.110
curl http://192.168.0.110/backup.sql.bak
curl http://192.168.0.110/uploads/
```

## Què hauria de detectar l'eina

!!! success "Deteccions esperades"
    Com a mínim:

    - [x] Ports oberts
    - [x] Servei detectat
    - [x] IP virtual activa
    - [x] Servei web redundant
    - [x] SSH amb configuració dèbil
    - [x] Fitxers sensibles accessibles al web
    - [x] Directori web navegable
    - [x] MariaDB amb accés remot
    - [x] DNS amb transferència de zona oberta
    - [x] FTP anònim
    - [x] Recurs Samba amb permisos insegurs

## Evidències per a la memòria

Guardeu:

- Captures de `ip a`
- Estat de `keepalived`
- Resultat abans i després del failover
- Sortida de l'escaneig
- Proves d'accés al backup des del web

### Taula final recomanada

| Servei | Node afectat | Vulnerabilitat | Evidència | Risc | Recomanació |
|--------|-------------|----------------|-----------|------|-------------|
| SSH | Tots dos | PermitRootLogin yes | Captura ssh-audit | Alt | Desactivar login root |
| Apache | Tots dos | Directory listing | Captura curl /uploads/ | Mig | Desactivar Indexes |
| MariaDB | Tots dos | bind-address 0.0.0.0 | Captura nmap -sV | Alt | Limitar a localhost |
| DNS | Secundari | Transferència oberta | Captura dig AXFR | Mig | Restringir allow-transfer |
| FTP | Secundari | Accés anònim | Captura ftp anònim | Mig | Desactivar anonymous |
| Samba | Secundari | Permisos oberts | Captura smbclient | Mig | Requerir autenticació |

## Llista de control

- [ ] `srv-primari` amb IP `192.168.0.100`
- [ ] `srv-secundari` amb IP `192.168.0.101`
- [ ] `keepalived` instal·lat als dos nodes
- [ ] IP virtual `192.168.0.110` configurada
- [ ] `Apache` funcionant als dos nodes
- [ ] `MariaDB` funcionant als dos nodes
- [ ] Usuari `auditor` creat als dos nodes
- [ ] `PasswordAuthentication yes`
- [ ] `PermitRootLogin yes`
- [ ] Directori `/uploads/` navegable
- [ ] Fitxer `backup.sql.bak` accessible
- [ ] Prova de failover feta correctament

!!! note "Nota final"
    No intentis fer una alta disponibilitat perfecta de tot. Per aquesta pràctica, el més intel·ligent és demostrar bé la IP virtual amb `keepalived`, tenir els dos nodes preparats, i afegir unes quantes vulnerabilitats clares perquè l'eina d'auditoria pugui lluir.
