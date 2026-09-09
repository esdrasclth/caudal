<div align="center">

# Caudal

### Finanzas personales que cuadran con cómo se gasta de verdad

### [💧 Abrir Caudal en caudal.brandsofts.com](https://caudal.brandsofts.com/)

[![Licencia AGPL v3](https://img.shields.io/badge/licencia-AGPL--3.0-2c6e49)](LICENSE)
[![Node 20](https://img.shields.io/badge/node-20-2c6e49)](https://nodejs.org/)
[![Next.js](https://img.shields.io/badge/Next.js-15-000000)](https://nextjs.org/)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-16-336791)](https://www.postgresql.org/)

Registra un gasto en dos toques, pon límites por categoría y mira a fin de mes
dónde se fue el dinero — con deudas, suscripciones y gastos compartidos en el
mismo sitio.

![Panel de Caudal](docs/capturas/panel.png)

</div>

## Por qué existe

Las apps de finanzas suelen fallar por un extremo o por el otro: o son una hoja
de cálculo con colores, o piden conectar el banco y clasificar cien movimientos
antes de servir para algo.

Caudal parte de otro sitio: **el gasto se anota a mano en segundos**, y todo lo
demás —presupuestos, deudas, suscripciones, reparto entre amigos— se apoya en
esos movimientos sin pedir nada más.

## Qué hace

| Área | Qué resuelve |
| --- | --- |
| **Movimientos** | Ingresos, gastos y transferencias entre carteras, con categoría y descripción |
| **Carteras** | Efectivo, banco, tarjeta de crédito y ahorro. Cada una con su moneda y su saldo |
| **Multimoneda** | Cuentas en lempiras y dólares conviviendo, con tasa de cambio aplicada al movimiento |
| **Presupuestos** | Límite por categoría, con arrastre del sobrante al mes siguiente |
| **Deudas** | Lo que debes y lo que te deben, con tasa, plazo y abonos |
| **Suscripciones** | Los cobros recurrentes y cuándo toca el siguiente |
| **Gastos compartidos** | Grupos, divisiones y liquidación: quién le debe cuánto a quién |
| **Reportes** | Evolución mensual, tasa de ahorro y presupuesto contra gasto real |
| **Exportar** | Los datos salen de aquí cuando quieras |

## Cómo se ve

### Presupuesto con alertas

El límite por categoría no sirve de nada si te enteras a fin de mes. Aquí la
categoría que se pasó se marca en cuanto ocurre, y la proyección dice a dónde
vas si sigues gastando al mismo ritmo.

![Presupuesto](docs/capturas/presupuesto.png)

### Reportes

![Reportes](docs/capturas/reportes.png)

<table>
<tr>
<td width="50%"><img src="docs/capturas/transacciones.png" alt="Movimientos"></td>
<td width="50%"><img src="docs/capturas/carteras.png" alt="Carteras"></td>
</tr>
<tr>
<td><b>Movimientos</b> — el registro de todo, filtrable</td>
<td><b>Carteras</b> — patrimonio repartido por cuenta y moneda</td>
</tr>
<tr>
<td><img src="docs/capturas/deudas.png" alt="Deudas"></td>
<td><img src="docs/capturas/suscripciones.png" alt="Suscripciones"></td>
</tr>
<tr>
<td><b>Deudas</b> — lo que debes y lo que te deben, con su avance</td>
<td><b>Suscripciones</b> — los cobros que se repiten solos</td>
</tr>
</table>

> Las capturas salen de una instancia local con datos inventados.

## Tecnologías

| Área | Tecnología |
| --- | --- |
| Aplicación | Next.js 15 (App Router), React, TypeScript |
| Estilos | Tailwind CSS |
| Base de datos | PostgreSQL 16 con Prisma |
| Sesión | Autenticación propia con cookies firmadas |
| Instalable | PWA con modo sin conexión |
| Desarrollo | Docker Compose para la base |

## Puesta en marcha

Requisitos: **Node 20+** y **Docker Desktop**.

```bash
git clone https://github.com/esdrasclth/caudal.git
cd caudal
npm install
```

Crea un `.env` en la raíz (está en `.gitignore`):

```env
DATABASE_URL="postgresql://caudal:caudal_dev_2026@localhost:5436/caudal?schema=public"
DIRECT_URL="postgresql://caudal:caudal_dev_2026@localhost:5436/caudal?schema=public"
AUTH_SECRET="cambia-este-secreto"
ZONA_HORARIA="America/Tegucigalpa"
```

`DIRECT_URL` la exige Prisma aunque en local apunte a la misma base; en
producción es la conexión sin pool.

```bash
npm run db:up        # PostgreSQL en Docker
npm run db:migrate   # migraciones de Prisma
npm run db:seed      # las 21 categorías del sistema
npm run dev          # http://localhost:3000
```

Abre `http://localhost:3000`, regístrate y empieza. **No hay datos de ejemplo**:
la cuenta nace vacía con las categorías del sistema.

## Scripts

| Script | Qué hace |
| --- | --- |
| `npm run dev` | Servidor de desarrollo |
| `npm run build` · `npm start` | Compilar y servir en producción |
| `npm run db:up` | Levanta PostgreSQL en Docker |
| `npm run db:migrate` | Aplica migraciones |
| `npm run db:seed` | Categorías del sistema |
| `npm run db:studio` | Prisma Studio |
| `npm run test:unit` · `npm run test:e2e` | Pruebas |
| `npm run lint` | ESLint |

## Estructura

```text
app/
  (app)/            pantallas con sesión: panel, movimientos, presupuesto…
  login · registro  entrada pública
  api/              rutas del servidor
prisma/             esquema, migraciones y seed
tests/              unitarias y de extremo a extremo
docs/capturas/      imágenes de este README
```

## Contribuir

[`CONTRIBUTING.md`](CONTRIBUTING.md) explica el entorno, las convenciones del
proyecto y qué comprobar antes de abrir un pull request.

## Licencia

**[GNU AGPL v3](LICENSE)**. Puedes usar, estudiar y modificar Caudal
libremente. La única condición: si lo despliegas y das acceso a otras personas
por red, tienes que publicar tu versión del código con la misma licencia.
