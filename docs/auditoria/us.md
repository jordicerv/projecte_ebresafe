# Ús de l'Eina

## Mode Local (GUI)

```bash
cd /home/alumnat/Visual/coses
source entorn/bin/activate
python telegram_gui.py
```

!!! info "Descripció"
    La GUI tkinter ofereix pestanyes per a cada mòdul d'auditoria, permetent executar escaneigs, veure resultats i enviar informes via Telegram.

## Mode Docker (USB)

### Preparació (a casa)

```bash
# Construir la imatge Docker
docker build -t auditoria_pendrive .

# Exportar la imatge al USB
docker save auditoria_pendrive -o docker_export/auditoria_pendrive.tar
```

### Execució (al client)

```bash
cd /media/.../docker_export
chmod +x executar.sh
./executar.sh
```

!!! tip "Detecció automàtica"
    El script `executar.sh` detecta automàticament si hi ha pantalla disponible:

    - **Si n'hi ha:** obre la GUI
    - **Si no:** arrenca el mode terminal

    A més, si detecta un fitxer `telegram_config.json`, el munta automàticament al contenidor.

### Comanda completa d'execució

```bash
docker run -it --rm --network host \
    -v $(pwd)/resultats:/app/dades \
    auditoria_pendrive
```

| Paràmetre | Funció |
|-----------|--------|
| `-it` | Mode interactiu (menú CLI) |
| `--rm` | Esborra el contenidor en sortir |
| `--network host` | Utilitza la xarxa real del host |
| `-v $(pwd)/resultats:/app/dades` | Munta carpeta local per a persistència dels informes |

## Flux de Treball USB

```mermaid
graph LR
    subgraph "A casa"
        A[docker build] --> B[docker save]
        B --> C[Copiar al USB]
    end
    subgraph "Al client"
        D[docker load] --> E[./executar.sh]
        E --> F{Pantalla?}
        F -->|Sí| G[GUI tkinter]
        F -->|No| H[CLI terminal]
        G & H --> I[Informes a resultats/]
    end
    C -->|USB| D
```

### Pas a pas

**Preparació (a casa):**

1. Construir la imatge: `docker build -t auditoria_pendrive .`
2. Exportar al USB: `docker save auditoria_pendrive -o /media/USB/auditoria_pendrive.tar`
3. Copiar la carpeta `docker_export/` al USB

**Execució al client:**

1. Carregar la imatge: `docker load -i auditoria_pendrive.tar`
2. Executar el llançador: `./executar.sh`
3. Desconnectar el USB — **no queda cap rastre al sistema amfitrió**

!!! success "Portabilitat"
    L'eina és completament portable gràcies a Docker. Un cop exportada en un USB, es pot executar en qualsevol màquina amb Docker instal·lat sense deixar cap rastre al sistema amfitrió.
