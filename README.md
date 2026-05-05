# 🛡️ EbreSafe - Projecte Intermodular

Documentació del projecte intermodular EbreSafe creada amb [MkDocs](https://www.mkdocs.org/) i el tema [Material for MkDocs](https://squidfund.github.io/mkdocs-material/).

## 🚀 Instal·lació local

```bash
# Clonar el repositori
git clone https://github.com/jordicerv/projecte_ebresafe.git
cd projecte_ebresafe

# Crear i activar entorn virtual
python3 -m venv venv
source venv/bin/activate

# Instal·lar dependències
pip install mkdocs mkdocs-material

# Servir localment
mkdocs serve
```

Obre el navegador a `http://127.0.0.1:8000`

## 📦 Desplegar a GitHub Pages

```bash
source venv/bin/activate
mkdocs gh-deploy
```

## 📁 Estructura

```
projecte_ebresafe/
├── docs/
│   ├── index.md
│   ├── presentacio/
│   ├── disseny/
│   ├── desenvolupament/
│   ├── desplegament/
│   └── conclusions/
├── mkdocs.yml
└── README.md
```
