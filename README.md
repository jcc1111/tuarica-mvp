# TuArica — README Inicial (MVP Público · KISS)

> Directorio digital de Arica (Chile) con navegación por **4 niveles**: **Categorías → Subcategorías → Comercios → Productos**.  
> Objetivo del MVP: **solo lectura pública**, sin ventas en línea ni administración de comerciantes todavía.  
> Este documento define todo lo necesario para construir y operar el proyecto **de extremo a extremo** sin ambigüedades.

---

## 0) Principios guía

- **KISS**: *Keep It Simple, Simple*. Menos piezas → menos errores → más velocidad.  
- **Robusto, compatible, moderno y simple**: decisiones que facilitan crecer.  
- **Sin monorepo por ahora**: un repo con dos apps (`/api`, `/web`) y **documentación clara**.  
- **Contratos primero**: API pública **GET** con OpenAPI; FE consume esos contratos.  
- **Escalabilidad futura**: dejar ganchos para portal de comerciantes y membresías.

---

## 1) Alcance del MVP (sí / no)

**Incluye (sí):**
- Navegación jerárquica N1→N2→N3→N4 (Home, Subcategorías, Comercios, Comercio).  
- API pública **solo GET** (catálogo); documentación Swagger.  
- Render de **imágenes estáticas** (categorías, subcategorías, comercios, productos).  
- SEO básico y accesibilidad mínima.  
- Búsqueda simple.  
- Seeds reproducibles para demo local.

**Excluye (no, pos-MVP):**
- Panel de **comerciantes** y **admin** internos.  
- Pagos, carrito, ventas online.  
- SSR/Next.js, microservicios, MeiliSearch, CI/CD avanzados.  
- Autenticación y autorización complejas (RBAC completo).

---

## 2) Impacto y objetivos

- **Usuarios:** encontrar comercios y productos locales rápidamente, con UI clara.  
- **Comercios:** visibilidad local (membresías a futuro).  
- **Éxito del MVP:**  
  1) Navegación fluida N1–N4.  
  2) Fichas de comercio y productos (sin compra).  
  3) Búsqueda simple funcional.  
  4) Lighthouse y accesibilidad básicas aceptables.

---

## 3) Arquitectura

- **Repositorio único** con dos aplicaciones:
  ```
  tuarica/
    api/   # NestJS + Prisma + PostgreSQL
    web/   # React + TypeScript + Vite
    docs/  # diagramas, decisiones, contratos
  ```
- **Backend:** NestJS 10/11, Prisma, PostgreSQL 16.x.  
- **Frontend:** React + TS + Vite, React Router v6, TanStack Query.  
- **Validación FE:** Zod (mínimo necesario).  
- **IDs:** UUID v4 (preparado para migrar a v7).  
- **Contratos:** OpenAPI **3.1** (si tooling pide, export paralelo 3.0).  
- **Imágenes estáticas:** servidas por el front desde `/public/imagenes/...`.

---

## 4) Estructura de carpetas (completa)

> **Nota**: nombres en español cuando sea razonable; convenciones técnicas se respetan.

```
tuarica/
├─ README.md
├─ docs/
│  ├─ decisiones/
│  │  ├─ 0001-kiss-arquitectura.md
│  │  └─ 0002-alcance-mvp.md
│  ├─ contratos/
│  │  ├─ openapi-3.1.yaml
│  │  └─ openapi-3.0.yaml   # si hace falta por tooling
│  ├─ diagramas/
│  │  ├─ mer.png            # entidad-relación
│  │  └─ arquitectura.png
│  └─ checklists/
│     └─ definition-of-done.md
│
├─ api/
│  ├─ .env.example
│  ├─ package.json
│  ├─ nest-cli.json
│  ├─ tsconfig.json
│  ├─ prisma/
│  │  ├─ schema.prisma
│  │  ├─ migrations/        # generado por Prisma
│  │  └─ seed.ts
│  └─ src/
│     ├─ main.ts
│     ├─ app.module.ts
│     ├─ common/
│     │  ├─ filters/
│     │  │  └─ http-exception.filter.ts
│     │  ├─ interceptors/
│     │  │  └─ cache.interceptor.ts
│     │  ├─ pipes/
│     │  │  └─ validation.pipe.ts
│     │  └─ dtos/
│     │     └─ pagination.dto.ts
│     ├─ config/
│     │  ├─ app.config.ts
│     │  └─ swagger.config.ts
│     ├─ modules/
│     │  ├─ categorias/
│     │  │  ├─ categorias.module.ts
│     │  │  ├─ categorias.controller.ts
│     │  │  └─ categorias.service.ts
│     │  ├─ subcategorias/
│     │  │  ├─ subcategorias.module.ts
│     │  │  ├─ subcategorias.controller.ts
│     │  │  └─ subcategorias.service.ts
│     │  ├─ comercios/
│     │  │  ├─ comercios.module.ts
│     │  │  ├─ comercios.controller.ts
│     │  │  └─ comercios.service.ts
│     │  └─ productos/
│     │     ├─ productos.module.ts
│     │     ├─ productos.controller.ts
│     │     └─ productos.service.ts
│     └─ prisma/
│        ├─ prisma.module.ts
│        └─ prisma.service.ts
│
└─ web/
   ├─ .env.example
   ├─ index.html
   ├─ package.json
   ├─ tsconfig.json
   ├─ vite.config.ts
   ├─ public/
   │  └─ imagenes/
   │     ├─ categorias/
   │     ├─ subcategorias/
   │     ├─ comercios/
   │     └─ productos/
   └─ src/
      ├─ main.tsx
      ├─ App.tsx
      ├─ rutas/
      │  └─ router.tsx
      ├─ paginas/
      │  ├─ PaginaPrincipal.tsx           # N1: Home
      │  ├─ PaginaCategoria.tsx           # N2: Subcategorías
      │  ├─ PaginaSubcategoria.tsx        # N3: Comercios
      │  ├─ PaginaComercio.tsx            # N4: Comercio + Productos
      │  └─ Pagina404.tsx
      ├─ layouts/
      │  ├─ LayoutBase.tsx
      │  ├─ LayoutComercio.tsx            # BASICO (MVP)
      │  └─ LayoutMinimal.tsx
      ├─ componentes/
      │  ├─ encabezado/
      │  │  ├─ Encabezado.tsx
      │  │  ├─ Logo.tsx
      │  │  ├─ MenuNavegacion.tsx
      │  │  └─ BotonModoOscuro.tsx        # opcional
      │  ├─ busqueda/
      │  │  ├─ BarraBusqueda.tsx
      │  │  └─ SugerenciasBusqueda.tsx    # opcional
      │  ├─ destacados/
      │  │  ├─ CarruselDestacados.tsx     # opcional (mock)
      │  │  ├─ TarjetaDestacado.tsx
      │  │  └─ ControlesCarrusel.tsx
      │  ├─ categorias/
      │  │  ├─ SeccionCategorias.tsx
      │  │  ├─ TarjetaCategoria.tsx
      │  │  ├─ IconoCategoria.tsx
      │  │  ├─ ListaSubcategorias.tsx
      │  │  └─ TarjetaSubcategoria.tsx
      │  ├─ comercios/
      │  │  ├─ FiltrosComercios.tsx       # N3 (opcional MVP)
      │  │  ├─ ListaComercios.tsx
      │  │  └─ TarjetaComercio.tsx
      │  ├─ comercio/                      # N4
      │  │  ├─ HeaderComercioBasico.tsx
      │  │  ├─ FichaComercio.tsx
      │  │  ├─ ServiciosComercio.tsx      # opcional
      │  │  ├─ ProductosComercio.tsx
      │  │  └─ GaleriaComercio.tsx        # opcional
      │  ├─ contacto/
      │  │  ├─ SeccionContacto.tsx
      │  │  ├─ FormularioContacto.tsx
      │  │  ├─ MapaUbicacion.tsx
      │  │  ├─ HorarioAtencion.tsx
      │  │  └─ CorreoContacto.tsx
      │  ├─ pie/
      │  │  ├─ PiePagina.tsx
      │  │  ├─ EnlacesRapidos.tsx
      │  │  └─ RedesSociales.tsx
      │  ├─ seo/
      │  │  └─ HeadSEO.tsx
      │  ├─ animaciones/
      │  │  └─ AparecerSuave.tsx
      │  └─ ui/
      │     ├─ Boton.tsx
      │     ├─ Tarjeta.tsx
      │     ├─ TituloSeccion.tsx
      │     └─ Paginacion.tsx
      ├─ servicios/
      │  └─ api.ts                         # cliente fetch/Axios + TanStack Query config
      ├─ hooks/
      │  ├─ useCategorias.ts
      │  ├─ useSubcategorias.ts
      │  ├─ useComercios.ts
      │  └─ useComercio.ts
      ├─ tipos/
      │  └─ index.ts                       # tipos de API (simples en MVP)
      ├─ estilos/
      │  └─ globals.css
      └─ utils/
         ├─ formatos.ts
         └─ constants.ts
```

---

## 5) Modelo de datos (MVP)

- **Categoria**: `id(UUID)`, `nombre`, `codigo(unique)`, `createdAt`, `updatedAt`  
- **Subcategoria**: `id`, `categoriaId(FK)`, `nombre`, `codigo`, timestamps  
- **Comercio**: `id`, `subcategoriaId(FK)`, `nombre`, `slug(unique)`, `membresia(BASICO default)`, `telefono?`, `email?`, `direccion?`, timestamps  
- **Producto**: `id`, `comercioId(FK)`, `nombre`, `descripcion?`, `imagenUrl?`, `activo(bool default true)`, timestamps

> *Preparado para añadir `Usuario`, `Rol`, `ComercioUsuario`, `HorarioAtencion`, `Media` en post-MVP.*

---

## 6) API pública (MVP)

**Base URL local**: `http://localhost:3000`  
**Swagger**: `http://localhost:3000/docs`  
**Endpoints GET (obligatorios):**
- `GET /categorias`  
- `GET /categorias/:id/subcategorias`  
- `GET /subcategorias/:id/comercios`  
- `GET /comercios/:id`  *(incluye productos del comercio)*

**Opcionales (si hay tiempo):**
- `GET /comercios/:id/productos?pag=...`  
- `GET /buscar?q=...` *(simple)*

**Respuesta de error (estándar):**
```json
{
  "statusCode": 400,
  "error": "Bad Request",
  "message": "Detalle legible del problema"
}
```

---

## 7) Frontend — Rutas y comportamientos

- **`/` (N1)**: muestra **categorías** (grid 3×3), barra de búsqueda (placeholder), carrusel (mock).  
- **`/categoria/:id` (N2)**: muestra **subcategorías** de esa categoría; breadcrumbs.  
- **`/subcategoria/:id` (N3)**: lista de **comercios** de esa subcategoría; opcional filtros/paginación.  
- **`/comercio/:id` (N4)**: cabecera del comercio + **productos** (tarjetas con imagen, nombre, breve descripción). **Sin** compra.  
- **`/404`**: página simple.

**TanStack Query**: usar `useQuery` por vista, `staleTime` razonable, estados de *loading/error* visibles.

---

## 8) Imágenes y assets

- Rutas públicas (front):  
  - `/public/imagenes/categorias/`  
  - `/public/imagenes/subcategorias/`  
  - `/public/imagenes/comercios/`  
  - `/public/imagenes/productos/`  
- Reglas:  
  - Nombres de archivo **kebab-case**.  
  - Proporciones consistentes por tipo (ej.: 4:3 en productos).  
  - Carga diferida (lazy) y `alt` descriptivo.

---

## 9) SEO, Accesibilidad, Rendimiento (mínimos)

- **SEO**: título/descr. por página (HeadSEO), meta OG básicas en Home y N4.  
- **Accesibilidad**: `alt` en imágenes, headings jerárquicos, foco visible, contrastes AA.  
- **Rendimiento**: imágenes optimizadas, lazy-loading, Lighthouse ≥ 85 en Perf y A11y.

---

## 10) Seguridad y observabilidad

- **Backend**: Helmet, CORS (orígenes locales permitidos), rate-limit simple.  
- **Validación**: DTOs en controladores; sanitizar parámetros.  
- **Logs**: formato consistente (nivel, timestamp, ruta, status).  
- **Errores**: filtro global con estructura estándar (ver 6).

---

## 11) Variables de entorno

**`/api/.env.example`**
```
DATABASE_URL=postgresql://postgres:postgres@localhost:5432/tuarica?schema=public
PORT=3000
NODE_ENV=development
```

**`/web/.env.example`**
```
VITE_API_URL=http://localhost:3000
```

> Copia como `.env` en cada app y ajusta según tu entorno.

---

## 12) Scripts y flujo de trabajo

**API**
- `npm install`
- `npx prisma migrate dev`  *(crea/migra BD)*
- `npm run seed`           *(carga datos de ejemplo)*
- `npm run start:dev`      *(Nest con hot reload)*
- `http://localhost:3000/docs` *(Swagger)*

**WEB**
- `npm install`
- `npm run dev`            *(Vite dev server)*
- `http://localhost:5173`  *(por defecto)*

---

## 13) Seeds (datos de ejemplo)

- **9 categorías** con iconos.  
- Subcategorías suficientes (ej.: “Restaurantes”, “Pizzerías”, “Cafeterías”…).  
- **Tienda1** (comercio ejemplo) con **6–8 productos** y `imagenUrl` válidas.  
- Mantener nombres realistas y cobertura para N1–N4.

---

## 14) Estándares de código

- **Formateo**: Prettier; **Lint**: ESLint.  
- **Convenciones**:
  - Componentes React: `PascalCase`.  
  - Hooks: `useX.ts`.  
  - Rutas: nombres claros y consistentes.  
  - Tipos TS: `type`/`interface` con nombres expresivos.  
- **Commits** (opcional): *Conventional Commits* (`feat:`, `fix:`, `docs:`…).

---

## 15) Definition of Done (MVP)

**API**
- [ ] Endpoints GET funcionan y devuelven seeds reales.  
- [ ] Swagger publicado y navegable.  
- [ ] Validación y manejo de errores homogéneo.  
- [ ] Seeds idempotentes.

**WEB**
- [ ] Rutas N1–N4 operativas y enlazadas.  
- [ ] Listados y fichas con datos reales.  
- [ ] Estados de carga y error visibles.  
- [ ] Imágenes estáticas cargando correctamente.

**CALIDAD**
- [ ] Lint + typecheck sin errores.  
- [ ] Lighthouse ≥ 85 en Perf y A11y (Home y N4).  
- [ ] README y `/docs` actualizados (contratos, decisiones, diagramas).

---

## 16) Roadmap post-MVP (no implementar ahora)

- **Portal de comerciantes** (`/comerciante`) con login (JWT), RBAC y scoping por comercio.  
- Membresías **BÁSICO/PRO/PREMIUM** que cambian layout en N4.  
- Búsqueda avanzada (FTS Postgres / MeiliSearch).  
- Admin interno (`/admin`) para categorías/subcategorías/comercios/productos.  
- Migración a **monorepo** si nacen paquetes compartidos o nuevas apps (móvil/admin separado).

---

## 17) Glosario rápido

- **N1**: Home (Categorías).  
- **N2**: Página de Categoría (Subcategorías).  
- **N3**: Página de Subcategoría (Comercios).  
- **N4**: Página de Comercio (ficha + productos).  
- **KISS**: mantener simple lo que funciona.

---

## 18) Preguntas frecuentes (MVP)

- **¿Por qué sin monorepo?**  
  Para no agregar tooling extra en esta fase. Migrar más tarde es sencillo.

- **¿Por qué solo GET?**  
  Para validar navegación, datos e imágenes. El CRUD llegará en el portal de comerciantes.

- **¿Por qué imágenes estáticas?**  
  Suficiente para validar UX/UI y performance. Se cambiará a media gestionada después.

---

### Cierre

Este README es **contrato operativo** del MVP TuArica.  
Si el agente o el desarrollador encuentra un vacío, **no asuma**: documente la duda en `docs/decisiones/` y proponga **una única opción KISS** para resolver, antes de implementar.
