# Manualia

Plataforma de recursos de IA en español: recetas paso a paso, skills, prompts y guías para conectar herramientas como Claude, ElevenLabs o Higgsfield.

🌐 [manualiahub.com](https://manualiahub.com)

> Proyecto privado basado en [nextjs/saas-starter](https://github.com/nextjs/saas-starter) (licencia MIT).

## Stack

- **Framework:** Next.js
- **Base de datos:** Postgres + Drizzle ORM
- **Pagos:** Stripe
- **UI:** shadcn/ui
- **Despliegue:** VPS con Coolify

## Desarrollo local

```bash
pnpm install
stripe login
pnpm db:setup      # crea el archivo .env
pnpm db:migrate
pnpm db:seed       # crea un usuario y equipo de prueba
pnpm dev
```

Abre [http://localhost:3000](http://localhost:3000).

Para recibir los webhooks de Stripe en local:

```bash
stripe listen --forward-to localhost:3000/api/stripe/webhook
```

Tarjeta de prueba: `4242 4242 4242 4242`, cualquier fecha futura y cualquier CVC.

## Variables de entorno

Se definen en `.env` (en local) y en Coolify (en producción). **Nunca se suben al repositorio.**

| Variable | Descripción |
|---|---|
| `BASE_URL` | URL de la web (en producción: `https://manualiahub.com`) |
| `STRIPE_SECRET_KEY` | Clave secreta de Stripe |
| `STRIPE_WEBHOOK_SECRET` | Secreto del webhook de Stripe |
| `POSTGRES_URL` | URL de conexión a la base de datos |
| `AUTH_SECRET` | Cadena aleatoria: `openssl rand -base64 32` |

## Producción

1. Desplegar en Coolify desde este repositorio, con límites de CPU y RAM.
2. Añadir las variables de entorno en Coolify.
3. Crear en Stripe un webhook apuntando a `https://manualiahub.com/api/stripe/webhook`, con los eventos `checkout.session.completed` y `customer.subscription.updated`.
