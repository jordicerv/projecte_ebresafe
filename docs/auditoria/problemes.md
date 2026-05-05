# Problemes Trobats i Solucions

## Taula de problemes

| Problema | Solució Aplicada |
|----------|-----------------|
| `Unable to locate package enum4linux` | Instal·lació des de GitHub amb `git clone` + symlink |
| `No matching distribution for theHarvester>=4.0.0` | Instal·lació des de GitHub amb `pip install git+...` |
| `aiodns requires Python >=3.10` amb `python:3.9-slim` | Actualització a `python:3.12-slim` |
| Nmap només escanneja la xarxa interna Docker | Afegit `--network host` al `docker run` |

## Detalls

### enum4linux no disponible a apt

!!! warning "Problema"
    A Debian Trixie (base de `python:3.12-slim`), `enum4linux` no existeix com a paquet apt.

**Solució:** Instal·lació directa des de GitHub:

```dockerfile
RUN git clone https://github.com/CiscoCXSecurity/enum4linux.git /opt/enum4linux \
    && ln -s /opt/enum4linux/enum4linux.pl /usr/local/bin/enum4linux
```

### theHarvester des de PyPI

!!! warning "Problema"
    El paquet `theHarvester` a PyPI és un placeholder (versió 0.0.1) i no funciona.

**Solució:** Instal·lació des del repositori oficial:

```dockerfile
RUN pip install --no-cache-dir git+https://github.com/laramies/theHarvester.git
```

### Versió de Python

!!! warning "Problema"
    `aiodns` (dependència de theHarvester) requereix Python >= 3.10, però la imatge inicial era `python:3.9-slim`.

**Solució:** Actualització de la imatge base a `python:3.12-slim`.

### Xarxa Docker

!!! warning "Problema"
    Per defecte, Docker utilitza una xarxa interna. Nmap només escanneja aquesta xarxa virtual i no troba els hosts reals.

**Solució:** Afegir `--network host` per utilitzar la xarxa real del host:

```bash
docker run -it --rm --network host \
    -v $(pwd)/resultats:/app/dades \
    auditoria_pendrive
```
