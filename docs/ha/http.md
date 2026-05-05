# HA de HTTP

!!! success "Ja configurat"
    Aquesta part ja està feta amb keepalived. Aquí queda resumida per coherència.

## Idea

- Mateixa web als dos nodes
- Accés per `http://192.168.0.110`
- Si cau el primari, el secundari continua responent

## Comprovació ràpida

Des del PC:

```bash
curl http://192.168.0.110
```

El failover ja ha estat provat a la secció de [Keepalived](../laboratori/keepalived.md).
