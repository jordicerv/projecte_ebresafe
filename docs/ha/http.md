# HA de HTTP

L'alta disponibilitat del servei web està garantida per `keepalived`: mateixa web als dos nodes amb accés a través de la VIP.

- Si cau el primari, el secundari continua responent a `http://192.168.0.110`
- El contingut web es manté sincronitzat amb `rsync`

Comprovació:

```bash
curl http://192.168.0.110
```

El failover del servei web està documentat i provat a la secció de [Keepalived](../laboratori/keepalived.md).
