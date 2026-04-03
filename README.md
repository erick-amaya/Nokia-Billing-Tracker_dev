# Nokia Billing Tracker — Liquidador de Sitios 2026

PWA para liquidación y seguimiento de sitios Nokia. Desarrollada para empresas subcontratistas de Nokia en Colombia. Cada empresa opera con su propio repositorio en GitHub Pages y su propio proyecto en Supabase.

---

## Características

- **Liquidador TI** — Actividades BASE / ADJ / CR con catálogo de precios por zona y categoría (A / AA / AAA)
- **Liquidador CW** — Obra Civil con catálogo URBANO / RURAL, campos SMP, Región, Tipo Zona y LC
- **Consolidado TI y CW** — Vista resumen con filtros, margen, utilidad y exportación a Excel con colores
- **Gastos registrados** — Por sitio: materiales, logística, adicionales, backoffice con lápiz de edición
- **Reportes y Analítica** — Gráficas por mes, categoría, LC y dispersión venta vs costo
- **Historial de cambios** — Auditoría completa de acciones por usuario (tabla `historial`)
- **Realtime** — Cambios de otros usuarios reflejados en tiempo real (Supabase Realtime)
- **Multi-empresa** — Un repositorio + un proyecto Supabase por empresa
- **PWA instalable** — Funciona offline, instalable en escritorio y celular

---

## Arquitectura

```
GitHub Pages          Supabase (por empresa)
─────────────         ──────────────────────
index.html  ←──────→  Auth (usuarios y roles)
(frontend)             sitios, gastos
                       catalogo, catalogo_cw
                       liquidaciones_cw
                       subcontratistas
                       user_roles
                       app_config
                       historial
```

El frontend es un único archivo estático (`index.html`). No hay build ni servidor propio. Toda la persistencia y autenticación corre en Supabase.

---

## Tecnología

| Capa | Herramienta |
|---|---|
| Frontend | HTML + CSS + JS (single-file, sin build) |
| Backend / BD | Supabase — PostgreSQL + Auth + Realtime |
| Hosting | GitHub Pages |
| Excel con colores | ExcelJS 4.3.0 |
| Gráficas | Chart.js 4.4.0 |
| Import Excel catálogo | SheetJS 0.18.5 |

---

## Estructura del repositorio

```
Nokia-Billing-Tracker-[Empresa]/
├── index.html          ← App completa (único archivo del frontend)
├── Index_Master.html   ← Plantilla en blanco para nuevas empresas
├── manifest.json       ← Configuración PWA
├── icon-192x192.png
├── icon-512x512.png
├── README.md
├── ONBOARDING.md       ← Guía paso a paso para nueva empresa
├── supabase_liquidaciones_cw.sql
└── supabase_historial.sql
```

---

## Roles de usuario

| Rol | Permisos |
|---|---|
| `admin` | Todo: configuración, catálogo, historial, eliminar sitios, reabrir liquidaciones |
| `coord` | Editar sitios, reabrir liquidaciones finales (TI y CW), agregar LC — sin acceso a precios ni configuración |
| `operador` | Crear/editar sitios, liquidar, registrar gastos |
| `viewer` | Solo lectura |

Los roles se asignan en la tabla `user_roles` de Supabase (columnas: `user_id` UUID, `role`).

---

## Tablas Supabase

| Tabla | Contenido |
|---|---|
| `sitios` | Sitios TI y TSS con actividades en JSON |
| `gastos` | Gastos por sitio |
| `catalogo` | Precios TI por actividad, ciudad y categoría |
| `catalogo_cw` | Precios CW por actividad (URBANO / RURAL) |
| `liquidaciones_cw` | Liquidaciones Obra Civil por sitio (id = sitio_id) |
| `user_roles` | Email → rol (admin / operador / viewer) |
| `subcontratistas` | LC / cuadrillas con categoría y tipo de cuadrilla |
| `app_config` | Configuración de empresa (nombre, logo, color primario) |
| `historial` | Auditoría de cambios con usuario, acción y detalle |

---

## Despliegue nueva empresa

Ver [ONBOARDING.md](ONBOARDING.md) para el proceso paso a paso.

---

## Instalación como PWA

**iPhone / iPad (Safari):**
1. Abrir la URL de GitHub Pages en Safari
2. Botón Compartir → *Añadir a pantalla de inicio*
3. Confirmar

**Android (Chrome):**
1. Abrir la URL en Chrome
2. Menú → *Instalar aplicación*

**Escritorio (Chrome / Edge):**
1. Abrir la URL
2. Ícono de instalación en la barra de dirección

---

## Licencia

Uso interno. No distribuir sin autorización.
