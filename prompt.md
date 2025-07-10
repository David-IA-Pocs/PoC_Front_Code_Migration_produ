Eres un **arquitecto de software especializado en Angular 19** y principios de ingeniería avanzada.  
Genera **un boilerplate completo** (solo código) que pueda servirme como punto de partida profesional para agregar codigo de una migracion de codigo angularjs 1.8 a angular 19 vas a tener que leer todo el codebase y seguir las sigueintes instruccion para nuevas aplicaciones Angular.  

## Objetivo general
Construir la base de una aplicación **Angular 19** que:
- Aplique **principios SOLID, DRY, KISS y YAGNI**. 
- Emplee **Angular Signals** para reactividad y estado.
- Use **componentes stand-alone** y **lazy loading** para optimizar el rendimiento.
- Mantenga una **arquitectura modular y escalable**.
- Sea **fácil de extender** con nuevas funcionalidades sin complicar la base existente.
- Usa patrones de diseño modernos y buenas prácticas de desarrollo. 
- Use las **mejores prácticas modernas de Angular 19** (stand-alone components, Signals, Control Flow, defer-loading, etc.).  
- Mantenga una **arquitectura limpia y modular**, preparada para escalar.  
- **No incluya** tests, CI/CD ni configuraciones externas: _solo_ el código fuente mínimo indispensable.

# Pasos (Recuerda que debes migrar el código de AngularJS 1.8 a Angular 19, debe verse exactamente igual al código de AngularJS 1.8, pero con las nuevas características de Angular 19, como los componentes stand-alone, Angular Signals. Ademas no se debe perder la funcionalidad del sistema, debe comportarse igual al código de AngularJS 1.8, pero con las nuevas características de Angular 19, como los componentes stand-alone, Angular Signals, etc.):

1. Primero, crea la estructura de carpetas y archivos base para una aplicación Angular 19, siguiendo las mejores prácticas y patrones modernos.

2. Luego migra los estilos, ya que estos son los que menos cambian entre AngularJS y Angular 19 y seran usados como base para el nuevo diseño en toda la aplicación.

3. Crea clases o componentes que puedan ser reutilizados en toda la aplicación, como botones, tarjetas, etc. Estos deben ser stand-alone y seguir el principio DRY.

4. Implementa el resto de la estructura modular, incluyendo servicios, interceptores, guards y librerías compartidas, todo usando Angular Signals y componentes stand-alone.


1. **Inicializar el proyecto** con Angular CLI 19 en modo _strict_.
2. **Estructurar carpetas** para aplicaciones, librerías y herramientas.
---

## Requerimientos detallados

1. **Inicialización del proyecto**  
   - Comando exacto con **Angular CLI 19** (o Nx en modo Angular) para crear el workspace en modo _strict_.  
   - Configura `package.json` con scripts esenciales: `start`, `build`, `lint`, `prepare`.  
   - Activa _strict templates_, _strict injection_, `moduleResolution: node16`, `resolveJsonModule`, `es2022`.

2. **Estructura de carpetas** (nivel raíz)  

┬ raíz del repo
├─ apps/                    # aplicaciones ejecutables
│  └─ web/                  # SPA principal (Angular 19)
│     ├─ src/
│     │  ├─ app/            # capa de "composición"
│     │  │  ├─ app.routes.ts
│     │  │  ├─ app.config.ts
│     │  │  └─ bootstrap.ts
│     │  └─ main.ts
│     └─ angular.json       # configuración específica de la app
├─ libs/                    # librerías reutilizables
│  ├─ core/                 # servicios singleton & bootstrapping
│  │  ├─ tokens/           # inyección de dependencias (InjectionToken)
│  │  ├─ http/             # interceptores, API client genérico
│  │  ├─ auth/             # AuthService, guards
│  │  ├─ core.providers.ts
│  │  └─ index.ts
│  ├─ shared/               # UI “presentacional” (atomic design)
│  │  ├─ components/       # stand-alone components (Button, Card…)
│  │  ├─ directives/
│  │  ├─ pipes/
│  │  ├─ style/             # SCSS global, design tokens
│  │  └─ index.ts
│  ├─ features/             # dominio funcional (lazy)
│  │  ├─ auth/              # sign-in / sign-up / profile
│  │  │  ├─ pages/
│  │  │  ├─ components/
│  │  │  ├─ services/      # únicamente lógica de dominio
│  │  │  ├─ state/         # Signals o NgRx minimal
│  │  │  └─ index.ts
│  │  ├─ dashboard/
│  │  └─ …
│  ├─ data-access/          # puertas a APIs externas o BFF
│  │  ├─ user-api/
│  │  ├─ product-api/
│  │  └─ …
│  └─ ui/                   # librerías de componentes de diseño propio
├─ tools/                   # scripts, generators, schematics
├─ docs/                    # ADR, diagramas de arquitectura
├─ package.json
└─ tsconfig.base.json

3. **Configuración global**  
- ESLint con las reglas recomendadas de Angular + extensiones para programación funcional.  
- Prettier con width = 100, tabs = false, trailingComma = all.  
- Alias de paths en `tsconfig.base.json` (`@core/*`, `@shared/*`, `@features/*`).  

4. **Core Library (`libs/core`)**  
- Servicios `ApiService`, `AuthService` con **dependency-injection tokens** (ej. `API_BASE_URL`).  
- Interceptores de _HTTP_ para _auth_ y _error handling_.  
- Guardas de ruta basados en `inject(AuthService)`.  
- _Providers_ centralizados en `core.providers.ts`.  

5. **Shared Library (`libs/shared`)**  
- **Componentes stand-alone** de presentación (ej. `ButtonComponent`, `CardComponent`).  
- Pipes y directivas comunes (`truncate.pipe.ts`, `hasRole.directive.ts`).  
- SCSS global con variables CSS (modo claro/oscuro).

6. **Feature Modules (`libs/features/*`)**  
- Cada feature como **stand-alone routed component** + sub-carpetas `components/`, `services/`, `models/`.  
- Configura **lazy-loading** en el `app.routes.ts` usando `loadComponent()` y `defer: true`.  

7. **Estado & Reactividad**  
- Emplea **Angular Signals** para estado local de componentes.  
- Para estado global moderado, crea un servicio `AppStateService` basado en `signal`, `computed`, `effect`.  
- No usar NgRx a menos que el feature lo requiera; si lo necesitas, incopora solo la mínima configuración.  

8. **Ejemplos de código clave**  
- Un componente stand-alone (`HomePageComponent`) con nueva sintaxis de control de flujo (`@for`, `@if`).  
- Un servicio `UserRepository` que implementa la interfaz `IUserRepository` (invierte dependencias, principio D).  
- `environment.ts` y `environment.prod.ts` con ejemplo de variables.  

9. **Style Guide & Documentación en línea**  
- Comentarios JSDoc de alto nivel en servicios y componentes críticos.  
- Añade README con instrucciones de instalación y ejecución (`npm i && npm start`).  

10. **Restricciones explícitas**  
 - **No** incluyas ningún archivo de prueba (unit, e2e, contract).  
 - **No** generes workflows de GitHub Actions, Dockerfiles, Sonar, Husky, ni configuraciones de CI/CD.  
 - **No** incluyas librerías de UI de terceros (Angular Material, PrimeNG, etc.). Solo CSS/SCSS propio.

---

## Entregables
1. Estructura de carpetas y archivos listos para copiar/pegar.  
2. Fragmentos de código representativos en cada sección clave.  
3. Comandos para ejecutar (`npm run start:web`, `npm run build:web`).  


----

Todo debe estar contenido en una carpeta nueva llamada `mig19`