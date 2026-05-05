# API

## Documentació de l'API

!!! info "Tipus d'API"
    Descriu el tipus d'API (REST, GraphQL, etc.) i la URL base.

**URL Base:** `http://localhost:XXXX/api`

## Endpoints

### :material-account: Usuaris

#### Obtenir tots els usuaris

```http
GET /api/usuaris
```

**Resposta:**
```json
{
    "status": "success",
    "data": [
        {
            "id": 1,
            "nom": "Exemple",
            "email": "exemple@email.com"
        }
    ]
}
```

<!-- TODO: Documenta els endpoints reals -->

### :material-lock: Autenticació

#### Login

```http
POST /api/auth/login
```

**Cos de la petició:**
```json
{
    "email": "usuari@email.com",
    "contrasenya": "********"
}
```

**Resposta:**
```json
{
    "status": "success",
    "token": "eyJhbGciOiJIUzI1NiIs..."
}
```

## Codis de resposta

| Codi | Significat |
|------|-----------|
| 200 | Èxit |
| 201 | Creat correctament |
| 400 | Petició incorrecta |
| 401 | No autoritzat |
| 404 | No trobat |
| 500 | Error del servidor |

## Autenticació

Descriu com s'autentiquen les peticions a l'API.
