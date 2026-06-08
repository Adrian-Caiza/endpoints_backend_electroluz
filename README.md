# Documentación de Endpoints (Organizada por Módulos)

Esta carpeta (`documentacion`) contiene la documentación de todos los endpoints activos del proyecto, agrupados por módulo.
Cada endpoint documentado tiene su propio archivo Markdown.

Fecha de verificación: 2026-06-06

## Estado de Cobertura

- Endpoints detectados en código: **71**
- Endpoints documentados en `documentacion`: **71**
- Endpoints faltantes: **0**
- Documentos auxiliares sin ruta HTTP asociada: **1**

## Notas de Conteo

- Este `README` funciona como índice general y no se cuenta como endpoint.
- Solo se cuentan como endpoints los archivos cuya primera línea sigue el formato `# METHOD /ruta`.
- Actualmente existe **1** documento auxiliar adicional:
  - `documentacion/proforma/proforma-response-structure.md`

## Módulos

### auth

- Endpoints: **3**
- Carpeta: `documentacion/auth`

### alert

- Endpoints: **3**
- Carpeta: `documentacion/alert`

### branch

- Endpoints: **4**
- Carpeta: `documentacion/branch`

### brand

- Endpoints: **4**
- Carpeta: `documentacion/brand`

### category

- Endpoints: **4**
- Carpeta: `documentacion/category`

### checkout

- Endpoints: **4**
- Carpeta: `documentacion/checkout`

### client

- Endpoints: **4**
- Carpeta: `documentacion/client`

### company

- Endpoints: **5**
- Carpeta: `documentacion/company`

### config

- Endpoints: **5**
- Carpeta: `documentacion/config`

### medida

- Endpoints: **4**
- Carpeta: `documentacion/medida`

### playment-method

- Endpoints: **4**
- Carpeta: `documentacion/playment-method`

### product

- Endpoints: **4**
- Carpeta: `documentacion/product`

### proforma

- Endpoints: **7**
- Carpeta: `documentacion/proforma`
- Documentos auxiliares: **1**

### proveedor

- Endpoints: **4**
- Carpeta: `documentacion/proveedor`

### stock

- Endpoints: **5**
- Carpeta: `documentacion/stock`

### system

- Endpoints: **1**
- Carpeta: `documentacion/system`

### user

- Endpoints: **6**
- Carpeta: `documentacion/user`

## Índice Rápido de Endpoints

### auth

- POST /auth/login
- POST /auth/logout
- POST /auth/refresh

### alert

- GET /alerts
- GET /alerts/events
- PATCH /alerts/:id/visto

### branch

- GET /branches
- GET /branches/:id
- PATCH /branches/:id
- POST /branches

### brand

- GET /brands
- GET /brands/:id
- PATCH /brands/:id
- POST /brands

### category

- GET /categories
- GET /categories/:id
- PATCH /categories/:id
- POST /categories

### checkout

- GET /checkouts
- GET /checkouts/:id
- PATCH /checkouts/:id/status
- POST /checkouts

### client

- GET /clients
- GET /clients/:id
- PATCH /clients/:id
- POST /clients

### company

- GET /companies
- GET /companies/:id
- PATCH /companies/:id
- PATCH /companies/:id/status
- POST /companies

### config

- DELETE /configs/:configKey
- GET /configs
- GET /configs/:configKey
- PATCH /configs/:configKey
- POST /configs

### medida

- GET /medidas
- GET /medidas/:id
- PATCH /medidas/:id
- POST /medidas

### playment-method

- GET /playment-methods
- GET /playment-methods/:id
- PATCH /playment-methods/:id
- POST /playment-methods

### product

- GET /products
- GET /products/:id
- PATCH /products/:id
- POST /products

### proforma

- GET /proformas
- GET /proformas/:id
- GET /proformas/:id/pdf
- PATCH /proformas/:id/cancel
- PATCH /proformas/:id/pay
- POST /proformas
- PUT /proformas/:id

### proveedor

- GET /proveedores
- GET /proveedores/:id
- PATCH /proveedores/:id
- POST /proveedores

### stock

- GET /stocks
- GET /stocks/:id
- GET /stocks/all
- PATCH /stocks/:id
- POST /stocks

### system

- GET /

### user

- GET /users
- GET /users/:id
- PATCH /users/:id
- PATCH /users/:id/password
- PATCH /users/:id/status
- POST /users
