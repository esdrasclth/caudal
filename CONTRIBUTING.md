# Contribuir a Caudal

Caudal es software libre bajo [AGPL v3](LICENSE): puedes usarlo, estudiarlo y
modificarlo. Si contribuyes aquí, tu aportación se publica con esa misma
licencia.

Los reportes de fallos y las propuestas son bienvenidos.

---

## Entorno

Requisitos: **Node 20+** y **Docker Desktop**.

```bash
npm install
```

`.env` en la raíz (ignorado por git):

```env
DATABASE_URL="postgresql://caudal:caudal_dev_2026@localhost:5436/caudal?schema=public"
DIRECT_URL="postgresql://caudal:caudal_dev_2026@localhost:5436/caudal?schema=public"
AUTH_SECRET="cambia-este-secreto"
ZONA_HORARIA="America/Tegucigalpa"
```

```bash
npm run db:up
npm run db:migrate
npm run db:seed
npm run dev
```

**Prisma lee `.env` y su valor gana sobre las variables del entorno.** Si
exportas `DATABASE_URL` en la terminal esperando apuntar a otra base, el `.env`
lo ignora en silencio y trabajas contra la que estuviera escrita ahí. Antes de
migrar o sembrar, comprueba el destino:

```bash
npx prisma migrate status   # imprime host y base a los que va a conectarse
```

No cuesta nada y evita el accidente que sí cuesta.

---

## Cómo está hecho

- **App Router de Next.js.** Las pantallas con sesión viven bajo `app/(app)/`;
  `login` y `registro` son las públicas.
- **La sesión es propia**, con cookies firmadas — no hay proveedor externo.
- **Los importes son `Decimal(19,4)` en Postgres.** Nunca uses coma flotante
  para dinero: `0.1 + 0.2` no da `0.3` y en una app de finanzas eso se nota.
- **Los valores de dominio van en minúscula**: `tipo` es `'gasto'`, `'ingreso'`
  o `'transferencia'`; las carteras son `'efectivo'`, `'banco'`, `'tarjeta'`,
  `'ahorro'`. Son columnas de texto, no enums de Postgres, así que nada te
  avisa si escribes `'GASTO'` — simplemente deja de aparecer en los listados.
- **Las categorías del sistema tienen `user_id` nulo** y `es_sistema = true`.
  Se siembran con `db:seed` y se comparten entre todas las cuentas.
- **Multimoneda**: un movimiento guarda `monto` en la moneda de la cartera,
  y `monto_original` con `tasa_cambio` cuando se registró en otra.

---

## Antes del pull request

```bash
npm run lint
npm run build
npm run test:unit
npm run test:e2e     # si tocaste flujos de la interfaz
```

Si cambiaste el esquema, añade la migración (`npx prisma migrate dev`) y súbela
con el cambio. Nada de `prisma db push` contra algo que no sea tu base local.

---

## Estilo

- **Español** en comentarios, nombres de dominio, mensajes de interfaz y de
  commit. El vocabulario del código es el del negocio: `cartera`, `movimiento`,
  `presupuesto`, `reparto`.
- **Conventional Commits**: `feat:`, `fix:`, `refactor:`, `docs:`.
- Comenta el **porqué**, no el qué.
- El bloque `<!-- BEGIN:nextjs-agent-rules -->` de `AGENTS.md` lo reescribe
  `next dev`. Va con el commit; borrarlo del diff sólo lo recrea.

---

## Pull requests

1. Rama descriptiva: `feat/metas-de-ahorro`, `fix/arrastre-presupuesto`.
2. Un pull request, un tema.
3. En la descripción: qué problema resuelve y cómo lo probaste.
4. Nunca subas `.env`, volcados de base ni capturas con datos financieros
   reales — las de este repositorio salen de una instancia local con datos
   inventados.

---

## Seguridad

Una vulnerabilidad no se reporta en un issue público. Escribe a
<Esdras.Clother@outlook.com> con los pasos para reproducirla.
