# Verificació del Laboratori

## Escaneig dels nodes

```bash
nmap -sV 192.168.0.100
nmap -sV 192.168.0.101
```

## Escaneig de la IP virtual

```bash
nmap -sV 192.168.0.110
```

## Proves manuals

```bash
curl http://192.168.0.100
curl http://192.168.0.101
curl http://192.168.0.110
curl http://192.168.0.110/backup.sql.bak
curl http://192.168.0.110/uploads/
```

## Deteccions esperades de l'eina

- [x] Ports oberts
- [x] Servei detectat per cada port
- [x] IP virtual activa
- [x] Servei web redundant
- [x] SSH amb configuració dèbil
- [x] Fitxers sensibles accessibles al web
- [x] Directori web navegable
- [x] MariaDB amb accés remot
- [x] DNS amb transferència de zona oberta
- [x] FTP anònim
- [x] Recurs Samba amb permisos insegurs

## Evidències

| Servei | Node | Vulnerabilitat | Risc | Recomanació |
|--------|------|----------------|------|-------------|
| SSH | Tots dos | PermitRootLogin yes | Alt | Desactivar login root |
| Apache | Tots dos | Directory listing | Mig | Desactivar Indexes |
| MariaDB | Tots dos | bind-address 0.0.0.0 | Alt | Limitar a localhost |
| DNS | Secundari | Transferència oberta | Mig | Restringir allow-transfer |
| FTP | Secundari | Accés anònim | Mig | Desactivar anonymous |
| Samba | Secundari | Permisos oberts | Mig | Requerir autenticació |

## Checklist

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
