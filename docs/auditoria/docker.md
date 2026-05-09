# Docker i USB

## Objectiu

Encapsular l'eina d'auditoria en un contenidor Docker portable: portar-lo en un USB, connectar-lo a qualsevol equip amb Docker i executar-lo sense instal·lar res.

## Dockerfile

La imatge es basa en `python:3.12-slim`:

```dockerfile
FROM python:3.12-slim

RUN apt-get update && apt-get install -y --no-install-recommends \
    nmap git smbclient samba-common-bin perl ldap-utils \
    iputils-ping net-tools python3-tk libx11-6 libxext6 \
    libxrender1 libxft2 fonts-noto-color-emoji fontconfig \
    && rm -rf /var/lib/apt/lists/*

RUN git clone https://github.com/CiscoCXSecurity/enum4linux.git /opt/enum4linux \
    && ln -s /opt/enum4linux/enum4linux.pl /usr/local/bin/enum4linux

WORKDIR /app
COPY requirements.txt /app/
RUN pip install --no-cache-dir -r requirements.txt
RUN pip install --no-cache-dir ssh-audit
RUN pip install --no-cache-dir git+https://github.com/laramies/theHarvester.git

COPY portscan+ssh+enum.py telegram_bot.py telegram_gui.py main.py /app/
RUN mkdir -p /app/dades

ENV DOCKER_CONTAINER=1
ENV REPORTS_DIR=/app/dades

ENTRYPOINT ["python", "telegram_gui.py"]
```

## Decisions tècniques

| Decisió | Motiu |
|---------|-------|
| `python:3.12-slim` | theHarvester requereix Python >= 3.12 |
| enum4linux via GitHub | No existeix com a paquet apt a Debian Trixie |
| theHarvester via GitHub | El paquet PyPI és un placeholder (v0.0.1) |
| `--network host` | Per escanejar la xarxa real del host |
| `REPORTS_DIR` env var | Rutes configurables per persistència amb volums |

## Rutes configurables

Les rutes dels informes es van fer configurables via `REPORTS_DIR`:

```python
_REPORTS_BASE = os.environ.get("REPORTS_DIR", "")

if _REPORTS_BASE:
    FITXER_MESTRE_SMB = os.path.join(_REPORTS_BASE, "informe_auditoria_smb_mestre.json")
    FITXER_LOG = os.path.join(_REPORTS_BASE, "auditoria.log")
    DIRECTORI_REPORTS = os.path.join(_REPORTS_BASE, "informes_auditoria")
```

## Comandes Docker

```bash
# Construir
docker build -t auditoria_pendrive .

# Exportar al USB
docker save auditoria_pendrive -o docker_export/auditoria_pendrive.tar

# Executar
docker run -it --rm --network host \
    -v $(pwd)/resultats:/app/dades \
    auditoria_pendrive
```

## Flux USB

```mermaid
graph TB
    subgraph "Preparació a casa"
        A["docker build -t auditoria_pendrive ."] --> B["docker save ... -o auditoria_pendrive.tar"]
        B --> C["Copiar docker_export/ al USB"]
    end
    subgraph "Execució al client"
        D["docker load -i auditoria_pendrive.tar"] --> E["./executar.sh"]
        E --> F["Auditoria en marxa"]
        F --> G["Resultats a resultats/"]
    end
    C -->|"USB 💾"| D
    G --> H["Desconnectar USB<br>Cap rastre al sistema"]
```
