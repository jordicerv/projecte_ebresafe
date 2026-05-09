# Configuració de Telegram

## Mètodes de configuració

El token de Telegram es pot configurar de tres maneres (per ordre de prioritat):

| Prioritat | Mètode | Detalls |
|-----------|--------|---------|
| 1 (màxima) | **Variables d'entorn** | `TELEGRAM_BOT_TOKEN` i `TELEGRAM_CHAT_ID` |
| 2 | **Fitxer JSON** | `telegram_config.json` |
| 3 | **GUI** | Pestanya de configuració dins de l'aplicació |

## Fitxer de configuració

```json
{
    "token": "EL_TEU_BOT_TOKEN",
    "chat_id": "EL_TEU_CHAT_ID"
}
```

## Variables d'entorn

```bash
export TELEGRAM_BOT_TOKEN="el_teu_token"
export TELEGRAM_CHAT_ID="el_teu_chat_id"
```

## Amb Docker

Si detecta `telegram_config.json` a la carpeta d'execució, el munta automàticament al contenidor:

```bash
docker run -it --rm --network host \
    -v $(pwd)/resultats:/app/dades \
    -v $(pwd)/telegram_config.json:/app/telegram_config.json \
    auditoria_pendrive
```

Mai publicar el token del bot ni el chat_id en repositoris públics. Afegir `telegram_config.json` al `.gitignore`.
