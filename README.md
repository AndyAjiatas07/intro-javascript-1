# 🏦 KinRural API Documentation

![Build Status](https://img.shields.io/badge/build-passing-brightgreen)
![Docker](https://img.shields.io/badge/docker-ready-blue)
![Node](https://img.shields.io/badge/node-v18+-green)
![License](https://img.shields.io/badge/license-MIT-yellow)

> Sistema bancario rural con panel administrativo y portal de clientes, desplegado con Docker.

---

## 📋 Tabla de Contenidos

- [Instalación y Configuración](#-instalación-y-configuración)
- [Panel Administrativo (Admin API)](#️-panel-administrativo-admin-api)
  - [Usuarios](#-usuarios)
  - [Roles](#-roles)
  - [Cuentas](#-cuentas)
  - [Solicitudes de Cuenta](#-solicitudes-de-cuenta)
  - [Préstamos](#-préstamos)
  - [Tarjetas](#-tarjetas)
  - [Transacciones](#-transacciones)
- [Panel Cliente (User API)](#-panel-cliente-user-api)
  - [Perfil](#-perfil)
  - [Mis Cuentas](#-mis-cuentas)
  - [Mis Solicitudes](#-mis-solicitudes)
  - [Beneficiarios](#-beneficiarios)
  - [Tarjetas (Usuario)](#-tarjetas-usuario)
  - [Préstamos (Usuario)](#-préstamos-usuario)
  - [Movimientos](#-movimientos)
  - [Transacciones (Usuario)](#-transacciones-usuario)
- [Reglas de Negocio](#️-reglas-de-negocio)

---

## 🚀 Instalación y Configuración

### 1. Crear carpeta principal

```bash
mkdir kinrural
cd kinrural
```

### 2. Clonar repositorios

```bash
# Servidor Admin
git clone https://github.com/breynerbd/kinRural-server-admin.git kinRural-server-admin

# Servidor User
git clone https://github.com/breynerbd/kinRural-server-user.git kinRural-server-user
```

### 3. Instalar dependencias

**📱 User Service**

```bash
cd ./kinRural-server-user/
npm install
```

**🛠️ Admin Service**

```bash
cd ../kinRural-server-admin/
npm install
```

### 4. Levantar contenedor Docker

```bash
docker compose up --build
```

> ✅ Esto levantará automáticamente:
> - API Admin
> - API User
> - Base de datos
>
> Todo dentro de contenedores Docker.

---

## 🛠️ Panel Administrativo (Admin API)

**Base URL:**

```
http://localhost:3005/kinrural/v1
```

---

### 👥 Usuarios

| Método | Endpoint | Descripción | Auth | Body |
|--------|----------|-------------|------|------|
| `POST` | `/users` | Crear usuario | Admin | `{ "nombre", "apellido", "dpi", "correo", "telefono", "direccion", "ingresos_mensuales", "role_id" }` |
| `GET` | `/users` | Listar usuarios | Admin | — |
| `GET` | `/users/:id` | Usuario por ID | Admin | — |
| `PUT` | `/users/:id` | Actualizar usuario | Admin | `{ "nombre", "correo" }` *(campos opcionales)* |
| `DELETE` | `/users/:id` | Eliminar usuario | Admin | — |

**Ejemplo — Crear usuario (`POST /users`):**

```json
{
  "nombre": "Kenneth",
  "apellido": "Mazariegos",
  "dpi": "9485760101",
  "correo": "km@gmail.com",
  "telefono": "98764532",
  "direccion": "Zona 5",
  "ingresos_mensuales": 8000,
  "role_id": 1
}
```

**Ejemplo — Actualizar usuario (`PUT /users/:id`):**

```json
{
  "nombre": "Andy",
  "correo": "am@gmail.com"
}
```

---

### 🔐 Roles

| Método | Endpoint | Descripción | Auth | Body |
|--------|----------|-------------|------|------|
| `POST` | `/roles` | Crear rol | Admin | `{ "nombre" }` |
| `GET` | `/roles` | Listar roles | Admin | — |
| `DELETE` | `/roles/:id` | Eliminar rol | Admin | — |

**Ejemplo — Crear rol (`POST /roles`):**

```json
{
  "nombre": "ADMIN"
}
```

---

### 💰 Cuentas

| Método | Endpoint | Descripción | Auth | Body |
|--------|----------|-------------|------|------|
| `POST` | `/accounts` | Crear cuenta | Admin | `{ "tipo", "saldo", "user_id" }` |
| `GET` | `/accounts` | Listar cuentas | Admin | — |
| `GET` | `/accounts/:id` | Cuenta por ID | Admin | — |
| `DELETE` | `/accounts/:id` | Eliminar cuenta | Admin | — |

**Ejemplo — Crear cuenta (`POST /accounts`):**

```json
{
  "tipo": "AHORRO",
  "saldo": 1500,
  "user_id": 1
}
```

> 💡 El campo `tipo` acepta los valores: `AHORRO` o `MONETARIA`.

---

### 📩 Solicitudes de Cuenta

| Método | Endpoint | Descripción | Auth | Body |
|--------|----------|-------------|------|------|
| `GET` | `/account-requests` | Listar solicitudes | Admin | — |
| `PATCH` | `/account-requests/:id/approve` | Aprobar solicitud | Admin | — |
| `PATCH` | `/account-requests/:id/reject` | Rechazar solicitud | Admin | — |

---

### 📝 Préstamos

| Método | Endpoint | Descripción | Auth | Body |
|--------|----------|-------------|------|------|
| `GET` | `/loans` | Listar préstamos | Admin | — |
| `GET` | `/loans/:id` | Préstamo por ID | Admin | — |
| `PUT` | `/loans/approve/:id` | Aprobar préstamo | Admin | — |
| `PUT` | `/loans/reject/:id` | Rechazar préstamo | Admin | — |
| `PUT` | `/loans/pay/:id` | Pagar cuota | Admin | — |
| `POST` | `/loans/check-mora` | Revisar mora | Admin | — |

---

### 💳 Tarjetas

| Método | Endpoint | Descripción | Auth | Body |
|--------|----------|-------------|------|------|
| `GET` | `/cards` | Listar tarjetas | Admin | — |
| `GET` | `/cards/:id` | Tarjetas por cuenta | Admin | — |
| `POST` | `/cards/:id` | Aprobar/Rechazar tarjeta | Admin | `{ "accion" }` |
| `POST` | `/cards/:id/activate` | Activar tarjeta | Admin | — |
| `POST` | `/cards/:id/block` | Bloquear tarjeta | Admin | `{ "accion" }` |

**Ejemplo — Aprobar tarjeta (`POST /cards/:id`):**

```json
{
  "accion": "APROBADA"
}
```

**Ejemplo — Rechazar tarjeta (`POST /cards/:id`):**

```json
{
  "accion": "RECHAZADA"
}
```

**Ejemplo — Bloquear tarjeta (`POST /cards/:id/block`):**

```json
{
  "accion": "BLOQUEADA"
}
```

> 💡 El campo `accion` acepta: `APROBADA`, `RECHAZADA` o `BLOQUEADA`.

---

### 💸 Transacciones

| Método | Endpoint | Descripción | Auth | Body |
|--------|----------|-------------|------|------|
| `GET` | `/transactions` | Historial global | Admin | — |
| `POST` | `/transactions` | Transferencia admin | Admin | `{ "cuenta_origen_id", "cuenta_destino_id", "monto" }` |
| `GET` | `/transactions/:id` | Transacciones por cuenta | Admin | — |

**Ejemplo — Transferencia (`POST /transactions`):**

```json
{
  "cuenta_origen_id": 1,
  "cuenta_destino_id": 2,
  "monto": 100.00
}
```

---

## 📱 Panel Cliente (User API)

**Base URL:**

```
http://localhost:3006/kinrural/v1/user
```

---

### 👤 Perfil

| Método | Endpoint | Descripción | Auth | Body |
|--------|----------|-------------|------|------|
| `GET` | `/users` | Ver mi perfil | User | — |
| `PUT` | `/users` | Actualizar perfil | User | `{ "nombre", "apellido", "telefono", "direccion", "ingresos_mensuales" }` |

**Ejemplo — Actualizar perfil (`PUT /users`):**

```json
{
  "nombre": "Diego",
  "apellido": "Leiva",
  "telefono": "12345678",
  "direccion": "El Naranjo",
  "ingresos_mensuales": 8000
}
```

---

### 💰 Mis Cuentas

| Método | Endpoint | Descripción | Auth | Body |
|--------|----------|-------------|------|------|
| `GET` | `/accounts` | Ver mis cuentas | User | — |

---

### 📩 Mis Solicitudes

| Método | Endpoint | Descripción | Auth | Body |
|--------|----------|-------------|------|------|
| `GET` | `/account-requests` | Ver mis solicitudes | User | — |
| `POST` | `/account-requests` | Solicitar apertura de cuenta | User | `{ "tipo", "dpi", "nombre_completo", "telefono", "correo" }` |

**Ejemplo — Solicitar apertura de cuenta (`POST /account-requests`):**

```json
{
  "tipo": "AHORRO",
  "dpi": "1234567890123",
  "nombre_completo": "Breyner Benitez",
  "telefono": "55551234",
  "correo": "bb@gmail.com"
}
```

> 💡 El campo `tipo` acepta los valores: `AHORRO` o `MONETARIA`.

---

### 👥 Beneficiarios

| Método | Endpoint | Descripción | Auth | Body |
|--------|----------|-------------|------|------|
| `POST` | `/beneficiaries` | Agregar beneficiario | User | `{ "cuenta_id", "alias" }` |
| `GET` | `/beneficiaries` | Listar beneficiarios | User | — |
| `PUT` | `/beneficiaries/:id` | Actualizar beneficiario | User | `{ "alias" }` |
| `DELETE` | `/beneficiaries/:id` | Eliminar beneficiario | User | — |

**Ejemplo — Agregar beneficiario (`POST /beneficiaries`):**

```json
{
  "cuenta_id": 1,
  "alias": "El águila"
}
```

**Ejemplo — Actualizar beneficiario (`PUT /beneficiaries/:id`):**

```json
{
  "alias": "nuevo_alias"
}
```

> 💡 El `alias` debe ser único por usuario y se utiliza para identificar al destinatario en transferencias.

---

### 💳 Tarjetas (Usuario)

| Método | Endpoint | Descripción | Auth | Body |
|--------|----------|-------------|------|------|
| `POST` | `/cards` | Solicitar tarjeta | User | `{ "cuenta_id", "tipo" }` |
| `GET` | `/cards` | Ver mis tarjetas | User | — |

**Ejemplo — Solicitar tarjeta (`POST /cards`):**

```json
{
  "cuenta_id": 1,
  "tipo": "DEBITO"
}
```

> 💡 El campo `tipo` acepta: `DEBITO` o `CREDITO`.

---

### 📝 Préstamos (Usuario)

| Método | Endpoint | Descripción | Auth | Body |
|--------|----------|-------------|------|------|
| `POST` | `/loans/quote` | Simular préstamo | User | `{ "monto", "tasa_interes", "plazo_meses" }` |
| `POST` | `/loans/request` | Solicitar préstamo | User | `{ "user_id", "cuenta_id", "monto", "tasa_interes", "plazo_meses", "tipo_tasa", "meses_recalculo" }` |
| `GET` | `/loans/user/:user_id` | Ver mis préstamos | User | — |

**Ejemplo — Simular préstamo (`POST /loans/quote`):**

```json
{
  "monto": 10000,
  "tasa_interes": 12,
  "plazo_meses": 12
}
```

**Ejemplo — Solicitar préstamo (`POST /loans/request`):**

```json
{
  "user_id": 5,
  "cuenta_id": 1,
  "monto": 5000,
  "tasa_interes": 12,
  "plazo_meses": 12,
  "tipo_tasa": "FIJA",
  "meses_recalculo": null
}
```

> 💡 El campo `tipo_tasa` acepta: `FIJA` o `VARIABLE`. Si es `FIJA`, `meses_recalculo` debe enviarse como `null`.

---

### 💸 Movimientos

| Método | Endpoint | Descripción | Auth | Body |
|--------|----------|-------------|------|------|
| `GET` | `/movements` | Historial de movimientos | User | — |

---

### 💳 Transacciones (Usuario)

| Método | Endpoint | Descripción | Auth | Body |
|--------|----------|-------------|------|------|
| `POST` | `/transactions` | Depósito | User | `{ "tipo": "DEPOSITO", "monto" }` |
| `POST` | `/transactions` | Retiro | User | `{ "tipo": "RETIRO", "monto" }` |
| `POST` | `/transactions` | Transferencia por alias | User | `{ "tipo": "TRANSFERENCIA", "monto", "alias" }` |
| `POST` | `/transactions` | Transferencia por ID de cuenta | User | `{ "tipo": "TRANSFERENCIA", "monto", "cuenta_destino_id" }` |

**Ejemplo — Depósito (`POST /transactions`):**

```json
{
  "tipo": "DEPOSITO",
  "monto": 100.50
}
```

**Ejemplo — Retiro (`POST /transactions`):**

```json
{
  "tipo": "RETIRO",
  "monto": 50.00
}
```

**Ejemplo — Transferencia por alias (`POST /transactions`):**

```json
{
  "tipo": "TRANSFERENCIA",
  "monto": 150.00,
  "alias": "El águila"
}
```

**Ejemplo — Transferencia por ID de cuenta (`POST /transactions`):**

```json
{
  "tipo": "TRANSFERENCIA",
  "monto": 150.00,
  "cuenta_destino_id": 123
}
```

> 💡 Para transferencias usa **`alias`** o **`cuenta_destino_id`**, nunca ambos a la vez.

---

## ⚙️ Reglas de Negocio

| Regla | Detalle |
|-------|---------|
| 📈 **Interés anual** | 5% sobre cuentas de ahorro |
| ⚠️ **Mora** | Después de 30 días → estado `EN_MORA` + 3% de recargo |
| 🏦 **Límite de cuentas** | Máx. **2 cuentas de ahorro** y **1 monetaria** por usuario |
| 💸 **Transferencias** | Límite de **Q10,000 diarios** |

> ⚠️ **Importante:** El estado `EN_MORA` se activa automáticamente. Ejecute `POST /loans/check-mora` periódicamente para mantener los estados actualizados.

---

*Documentación generada para el proyecto **KinRural** — Sistema Bancario Rural* 🌾
