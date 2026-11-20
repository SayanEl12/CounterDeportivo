# Analysis & Design — App de Entrenamientos

> **Fase 2 · Análisis y diseño**
> Autor: Juan José Palacio Madrid · Versión: 0.1 · Fecha: 2026-11-18 · Estado: 🟡 En progreso
> Entrada: [Requirements Gathering](./Requirements_Gathering.md)

En la fase 1 definiste **qué** hace la app. Aquí decides **cómo** se va a construir, sin escribir código todavía. Cada decisión debe poder rastrearse a un requisito (RF, RNF o RN).

**Cómo usar esta guía:** cada sección trae *Qué es*, *Preguntas guía* y espacios `TODO` para llenar. Los ejemplos resueltos son de referencia: ajústalos o reemplázalos.

---

## Contenido

1. [Casos de uso](#1-casos-de-uso)
2. [Modelo de datos](#2-modelo-de-datos)
3. [Arquitectura](#3-arquitectura)
4. [Stack tecnológico](#4-stack-tecnológico)
5. [Diseño de pantallas (wireframes)](#5-diseño-de-pantallas-wireframes)
6. [Flujo offline y sincronización](#6-flujo-offline-y-sincronización)
7. [Matriz de trazabilidad](#7-matriz-de-trazabilidad)
8. [Plan de implementación](#8-plan-de-implementación)

---

## 1. Casos de uso

Cada historia de usuario se convierte en un caso de uso con el paso a paso de la interacción, incluidos los errores. Sirve para descubrir pantallas, validaciones y casos borde.

### Lista de casos de uso

| ID | Caso de uso | Actor | Requisitos | Prioridad |
| --- | --- | --- | --- | :---: |
| **CU-01** | Registrar entrenamiento | Usuario | RF-01, RF-02, RN-01, RN-02 | 🔴 Must |
| **CU-02** | Editar entrenamiento | Usuario | RF-02, RN-02 | 🔴 Must |
| **CU-03** | Eliminar entrenamiento | Usuario | RF-03 | 🔴 Must |
| **CU-04** | Crear meta | Usuario | RF-05, RN-03, RN-04 | 🟠 Should |
| **CU-05** | Editar meta | Usuario | RF-07, RN-03 | 🟠 Should |
| **CU-06** | Eliminar meta | Usuario | RF-07 | 🟠 Should |
| **CU-07** | Consultar total de entrenamientos | Usuario | RF-04 | 🟠 Should |
| **CU-08** | Comparar metas vs. entrenamientos | Usuario | RF-06 | 🟠 Should |
| **CU-09** | Sincronizar cambios pendientes | Usuario | RNF-02 | 🔴 Must |
| **CU-10** | Gestionar tipos de entrenamiento *(propuesto)* | Usuario | M-01 | 🔴 Must |

### Tus casos de uso detallados

### Flujo común FC-01 — Guardar cambio

Todos los casos que crean, editan o eliminan terminan en este flujo (la parte derecha del diagrama).

| Paso | Detalle |
| --- | --- |
| **1** | El sistema verifica si hay conexión a internet. |
| **2** | **Con internet:** la BD valida el cambio y lo guarda. El listado se actualiza. |
| **2a** | **Sin internet:** el cambio se guarda localmente, marcado como pendiente. Se continúa en CU-09. |
| **2b** | **La BD rechaza el cambio:** el sistema muestra el error y mantiene el formulario con los datos ingresados. |M-04

---

### CU-01 — Registrar entrenamiento

| Campo | Detalle |
| --- | --- |
| **Actor** | Usuario |
| **Requisitos** | RF-01, RF-02, RN-01, RN-02 |
| **Precondición** | La app está en la pantalla principal y existe al menos un tipo de entrenamiento. |
| **Flujo principal** | 1. El usuario oprime **`+`**.<br>2. El sistema abre el formulario *Entrenamiento* con la fecha y hora actuales (RN-01).<br>3. El usuario elige el tipo y, si quiere, ajusta la fecha y la hora.<br>4. El usuario oprime **Guardar**.<br>5. El sistema valida que todos los campos estén completos (RN-02).<br>6. Se ejecuta **FC-01**.<br>7. El entrenamiento aparece en el listado. |
| **Flujos alternos** | **4a.** El usuario oprime **Cancelar** → el formulario se cierra sin guardar.<br>**5a.** Hay campos vacíos → el sistema los resalta con un mensaje y vuelve al paso 3. |
| **Postcondición** | El entrenamiento queda guardado en la BD o en local como pendiente. |

### CU-02 — Editar entrenamiento

| Campo | Detalle |
| --- | --- |
| **Actor** | Usuario |
| **Requisitos** | RF-02, RN-02 |
| **Precondición** | Existe al menos un entrenamiento en el listado. |
| **Flujo principal** | 1. El usuario oprime un entrenamiento del listado.<br>2. El sistema abre el formulario *Entrenamiento* con sus datos cargados.<br>3. El usuario modifica fecha, hora o tipo.<br>4. El usuario oprime **Guardar**.<br>5. El sistema valida que todos los campos estén completos (RN-02).<br>6. Se ejecuta **FC-01**.<br>7. El listado muestra los datos actualizados. |
| **Flujos alternos** | **4a.** El usuario oprime **Cancelar** → no se guarda ningún cambio.<br>**5a.** Hay campos vacíos → el sistema los resalta y vuelve al paso 3. |
| **Postcondición** | El entrenamiento queda actualizado. |

### CU-03 — Eliminar entrenamiento

| Campo | Detalle |
| --- | --- |
| **Actor** | Usuario |
| **Requisitos** | RF-03 |
| **Precondición** | Existe al menos un entrenamiento en el listado. |
| **Flujo principal** | 1. El usuario oprime un entrenamiento del listado.<br>2. El sistema abre el formulario *Entrenamiento* con sus datos.<br>3. El usuario oprime **Eliminar** 🗑️.<br>4. El sistema pide confirmación.<br>5. El usuario confirma.<br>6. Se ejecuta **FC-01**.<br>7. El entrenamiento desaparece del listado. |
| **Flujos alternos** | **5a.** El usuario cancela la confirmación → vuelve al formulario sin cambios. |
| **Postcondición** | El entrenamiento queda eliminado y deja de contar en las estadísticas. |

### CU-04 — Crear meta

| Campo | Detalle |
| --- | --- |
| **Actor** | Usuario |
| **Requisitos** | RF-05, RN-03, RN-04 |
| **Precondición** | La app está en la pantalla principal y existe al menos un tipo de entrenamiento. |
| **Flujo principal** | 1. El usuario oprime **Nueva meta**.<br>2. El sistema abre el formulario *Meta* con `fecha_inicio` = hoy y `fecha_fin` = hoy + 7 días (RN-04).<br>3. El usuario agrega uno o más tipos, cada uno con su cantidad de entrenamientos.<br>4. El usuario ajusta las fechas si quiere.<br>5. El usuario oprime **Guardar**.<br>6. El sistema valida que los campos estén completos, que haya al menos un tipo y que `fecha_fin` sea posterior a `fecha_inicio` (RN-03).<br>7. Se ejecuta **FC-01**.<br>8. La meta aparece en el listado de metas. |
| **Flujos alternos** | **3a.** El usuario agrega un tipo que ya está en la meta → el sistema no lo permite.<br>**5a.** El usuario oprime **Cancelar** → no se guarda.<br>**6a.** Hay campos vacíos o ningún tipo → mensaje y vuelve al paso 3.<br>**6b.** `fecha_fin` ≤ `fecha_inicio` → mensaje y vuelve al paso 4. |
| **Postcondición** | La meta queda guardada y disponible en el filtro de Estadísticas. |

### CU-05 — Editar meta

| Campo | Detalle |
| --- | --- |
| **Actor** | Usuario |
| **Requisitos** | RF-07, RN-03 |
| **Precondición** | Existe al menos una meta en el listado. |
| **Flujo principal** | 1. El usuario oprime una meta del listado.<br>2. El sistema abre el formulario *Meta* con sus datos cargados.<br>3. El usuario modifica fechas, tipos o cantidades.<br>4. El usuario oprime **Guardar**.<br>5. El sistema valida igual que en CU-04, paso 6.<br>6. Se ejecuta **FC-01**.<br>7. El listado muestra la meta actualizada. |
| **Flujos alternos** | **4a.** El usuario oprime **Cancelar** → no se guarda ningún cambio.<br>**5a.** La validación falla → mensaje y vuelve al paso 3. |
| **Postcondición** | La meta queda actualizada y la comparación de Estadísticas la refleja. |

### CU-06 — Eliminar meta

| Campo | Detalle |
| --- | --- |
| **Actor** | Usuario |
| **Requisitos** | RF-07 |
| **Precondición** | Existe al menos una meta en el listado. |
| **Flujo principal** | 1. El usuario oprime una meta del listado.<br>2. El sistema abre el formulario *Meta*.<br>3. El usuario oprime **Eliminar** 🗑️.<br>4. El sistema pide confirmación.<br>5. El usuario confirma.<br>6. Se ejecuta **FC-01**.<br>7. La meta desaparece del listado. |
| **Flujos alternos** | **5a.** El usuario cancela la confirmación → vuelve al formulario sin cambios. |
| **Postcondición** | La meta se elimina; los entrenamientos registrados no se modifican. |

### CU-07 — Consultar total de entrenamientos

| Campo | Detalle |
| --- | --- |
| **Actor** | Usuario |
| **Requisitos** | RF-04 |
| **Precondición** | Ninguna. |
| **Flujo principal** | 1. El usuario abre la pestaña **Estadísticas**.<br>2. El sistema muestra la gráfica de total de entrenamientos con el filtro por defecto (TODO: definir, p. ej. semana actual).<br>3. El usuario cambia el filtro de tiempo (semana / mes) o de tipo.<br>4. El sistema consulta los datos y actualiza la gráfica. |
| **Flujos alternos** | **4a.** No hay entrenamientos en el periodo → el sistema muestra "Sin entrenamientos en este periodo".<br>**4b.** Sin internet → TODO: definir si se grafica con los datos locales. |
| **Postcondición** | Ninguna (solo consulta). |

### CU-08 — Comparar metas vs. entrenamientos

| Campo | Detalle |
| --- | --- |
| **Actor** | Usuario |
| **Requisitos** | RF-06 |
| **Precondición** | Existe al menos una meta. |
| **Flujo principal** | 1. El usuario abre la pestaña **Estadísticas**.<br>2. El usuario selecciona una meta en el filtro.<br>3. El sistema consulta los entrenamientos de cada tipo de la meta dentro de su rango de fechas.<br>4. El sistema muestra la gráfica de entrenamientos hechos vs. planeados por tipo. |
| **Flujos alternos** | **2a.** No hay metas → el sistema muestra un mensaje con acceso a **Nueva meta** (CU-04). |
| **Postcondición** | Ninguna (solo consulta). |

### CU-09 — Sincronizar cambios pendientes

| Campo | Detalle |
| --- | --- |
| **Actor** | Usuario |
| **Requisitos** | RNF-02 |
| **Precondición** | Hay cambios guardados en local (FC-01, paso 2a). |
| **Flujo principal** | 1. El sistema muestra el botón **Subir cambios guardados** con la cantidad de cambios pendientes.<br>2. El usuario oprime el botón.<br>3. El sistema verifica la conexión.<br>4. El sistema envía cada cambio a la BD, que lo valida y lo guarda.<br>5. El sistema limpia los pendientes, oculta el botón y actualiza los listados. |
| **Flujos alternos** | **3a.** Sin internet → mensaje; los cambios siguen pendientes.<br>**4a.** La BD rechaza un cambio → el sistema indica cuál y lo deja pendiente para corregirlo.<br>**4b.** El registro fue modificado desde otro dispositivo → TODO: regla de conflicto (ver sección 6). |
| **Postcondición** | Los datos locales y los de la BD quedan iguales. |

### CU-10 — Gestionar tipos de entrenamiento *(propuesto)*

> No aparece en el diagrama ni en los RF, pero M-01 es Must y el formulario necesita tipos. Decide si se vuelve **RF-08** o si los tipos se cargan solo desde la BD.

| Campo | Detalle |
| --- | --- |
| **Actor** | Usuario |
| **Requisitos** | M-01 |
| **Precondición** | Ninguna. |
| **Flujo principal** | 1. El usuario abre la lista de tipos.<br>2. El usuario oprime **Nuevo tipo** y escribe el nombre.<br>3. El sistema valida que el nombre no esté vacío ni repetido.<br>4. Se ejecuta **FC-01**.<br>5. El tipo queda disponible en los formularios de entrenamiento y meta. |
| **Flujos alternos** | **3a.** Nombre vacío o repetido → mensaje y vuelve al paso 2.<br>**3b.** Se intenta eliminar un tipo con entrenamientos asociados → el sistema no lo permite. |
| **Postcondición** | El catálogo de tipos queda actualizado. |

---

## 2. Modelo de datos

El modelo de dominio de la fase 1 convertido en tablas con llaves, tipos de dato y restricciones.

<img width="690" height="525" alt="Bases de datos(2)" src="https://github.com/user-attachments/assets/b0469bb2-3ac4-4d21-85a1-d2ada7689ac7" />

---

## 3. Arquitectura

cómo se organizan las partes del sistema y cómo se comunican. RNF-01 (web responsive) y RNF-02 (offline) son los requisitos que más la condicionan.
**Cliente web + API REST propia + PostgreSQL**

### 3.1 Estilo arquitectónico

| Decisión | Elección |
| --- | --- |
| Estilo general | Cliente-servidor, API-first |
| Backend | Monolito modular en 3 capas (Router → Service → Repository) |
| Comunicación | HTTP/HTTPS + JSON, API REST versionada (`/api/v1`) |
| Persistencia | PostgreSQL (servidor) + IndexedDB (cola local del navegador) |
| Identificadores | UUID generados en el cliente |
| Despliegue local | Docker Compose (API + BD) |

### 3.2 Componentes

Usa esta lista como los **nodos** de tu diagrama.

#### Capa cliente

| # | Componente | Responsabilidad |
| --- | --- | --- |
| C1 | **Web SPA** (v1) | Interfaz responsive para celular y PC. |
| C1.1 | Páginas y componentes | Pantalla principal, formularios, Estadísticas. |
| C1.2 | Estado / hooks | Datos en pantalla, validación de UX (campos vacíos, RN-03). |
| C1.3 | API client (services) | Única capa que habla con la API (`entrenamientosApi`, `metasApi`…). |
| C1.4 | Almacén local (IndexedDB) | Cola de cambios pendientes cuando no hay conexión. |
| C1.5 | Sync manager | Detecta pendientes y los envía con **Subir cambios guardados**. |
| C2 | **App móvil** *(futuro)* | Mismo contrato de API; offline completo. |

#### Capa servidor (API)

| # | Componente | Responsabilidad |
| --- | --- | --- |
| S1 | **Routers** (controladores) | Reciben la petición HTTP, llaman al service y devuelven la respuesta. Sin lógica de negocio. |
| S2 | **Schemas** (DTO, Pydantic) | Definen y validan la forma de los datos de entrada y salida. |
| S3 | **Services** | Lógica de negocio y reglas (RN-01 a RN-04), sincronización, cálculo de estadísticas. |
| S4 | **Repositories** | Único acceso a la BD: consultas y escrituras. |
| S5 | **Models** (ORM, SQLAlchemy) | Tablas representadas como clases. |
| S6 | **Core** (transversal) | Configuración, manejo de errores, logging, CORS. *Futuro: autenticación JWT.* |

#### Capa de datos e infraestructura

| # | Componente | Responsabilidad |
| --- | --- | --- |
| D1 | **PostgreSQL** | Fuente de verdad de los datos. |
| D2 | **Migraciones** (Alembic) | Versionan los cambios del esquema de la BD. |
| I1 | **Hosting frontend** | Sirve los archivos estáticos de la SPA. |
| I2 | **Hosting API** | Ejecuta el contenedor del backend. |
| I3 | **BD administrada** | PostgreSQL en la nube. |
| I4 | **GitHub** | Código fuente; a futuro CI/CD (GitHub Actions). |

### 3.3 Conexiones

Usa esta tabla como las **flechas** de tu diagrama.

| Origen | Destino | Qué viaja | Protocolo |
| --- | --- | --- | --- |
| C1.1 Páginas | C1.2 Estado | Acciones del usuario | Llamada interna |
| C1.2 Estado | C1.3 API client | Datos validados | Llamada interna |
| C1.3 API client | S1 Routers | Petición REST | HTTPS + JSON |
| C1.3 API client | C1.4 IndexedDB | Cambio pendiente (si falla la red) | Llamada interna |
| C1.5 Sync manager | C1.4 IndexedDB | Lee la cola de pendientes | Llamada interna |
| C1.5 Sync manager | S1 Routers | Lote de cambios (`POST /sync`) | HTTPS + JSON |
| C2 App móvil *(futuro)* | S1 Routers | Mismas peticiones REST | HTTPS + JSON |
| S1 Routers | S2 Schemas | Valida entrada / arma salida | Llamada interna |
| S1 Routers | S3 Services | Datos validados | Llamada interna |
| S3 Services | S4 Repositories | Operaciones de datos | Llamada interna |
| S4 Repositories | S5 Models → D1 PostgreSQL | Consultas SQL | TCP (driver de Postgres) |
| D2 Alembic | D1 PostgreSQL | Cambios de esquema | TCP |

**Regla de dependencias:** las flechas solo van hacia abajo. Un router nunca toca un repository, y un repository nunca conoce HTTP.

### 3.4 Contrato de la API (`/api/v1`)

| Recurso | Método y ruta | Caso de uso |
| --- | --- | --- |
| Tipos | `GET /tipos` · `POST /tipos` · `PUT /tipos/{id}` · `DELETE /tipos/{id}` | CU-10 |
| Entrenamientos | `GET /entrenamientos?desde=&hasta=&tipo=` | Listado |
| | `POST /entrenamientos` | CU-01 |
| | `PUT /entrenamientos/{id}` | CU-02 |
| | `DELETE /entrenamientos/{id}` | CU-03 |
| Metas | `GET /metas` · `POST /metas` · `PUT /metas/{id}` · `DELETE /metas/{id}` | CU-04 a CU-06 |
| Estadísticas | `GET /estadisticas/totales?desde=&hasta=&agrupar=semana\|mes\|tipo` | CU-07 |
| | `GET /estadisticas/metas/{id}` | CU-08 |
| Sincronización | `POST /sync` (lote de cambios pendientes) | CU-09 |
| Salud | `GET /health` | Monitoreo |

Documentación automática en `/docs` (Swagger UI, generado por FastAPI).

**Códigos de respuesta:** `200` OK · `201` creado · `204` eliminado · `404` no existe · `409` conflicto de sincronización · `422` validación fallida (RN-02, RN-03).

### 3.5 Flujo de una petición (ej.: CU-01 con internet)

1. El usuario oprime **Guardar** → el formulario valida campos (C1.2).
2. El API client genera el `id` (UUID) y envía `POST /api/v1/entrenamientos` (C1.3).
3. El router recibe y el schema valida el JSON (S1, S2). Si falla → `422`.
4. El service aplica las reglas de negocio (S3).
5. El repository inserta en PostgreSQL (S4 → D1).
6. La API responde `201` con el entrenamiento creado y la pantalla actualiza el listado.

Sin internet, el paso 2 guarda el cambio en IndexedDB (C1.4) y el flujo sigue en la sección 6.

### 3.6 Preparado para crecer

| Futuro | Qué ya lo permite |
| --- | --- |
| App móvil | API-first, versionada y documentada; reglas en el backend, no en la web. |
| Login y multiusuario | Capa Core lista para JWT; agregar `usuario_id` a las tablas. |
| Competencia en línea | Nuevo módulo (router + service + repository) sin tocar los existentes. |
| Analítica / big data | Datos en PostgreSQL + Python; se puede agregar un módulo de análisis o exportar a un data warehouse. |

### 3.7 Estructura del repositorio (monorepo)

```text
app-entrenamientos/
├── docs/                    # Requirements_Gathering.md, Analysis_Design.md, diagramas
├── frontend/
│   └── src/
│       ├── pages/           # Principal, Estadisticas
│       ├── components/      # Formularios, listados, gráficas
│       ├── hooks/           # Estado y validación de UX
│       ├── services/        # API client (único acceso a la API)
│       └── offline/         # IndexedDB + sync manager
├── backend/
│   ├── app/
│   │   ├── routers/         # S1
│   │   ├── schemas/         # S2
│   │   ├── services/        # S3
│   │   ├── repositories/    # S4
│   │   ├── models/          # S5
│   │   ├── core/            # S6: config, errores, CORS
│   │   └── main.py
│   ├── alembic/             # D2: migraciones
│   └── tests/
├── docker-compose.yml       # API + PostgreSQL en local
└── README.md
```

### Diagrama de arquitectura
<img width="2370" height="2377" alt="Sin título(1)" src="https://github.com/user-attachments/assets/036a2e5c-897d-401d-812c-a72b4f356902" />

---

## 4. Stack tecnológico

Las herramientas concretas para cada capa. Recuerda que el stakeholder *desarrollador* quiere aprender tecnologías nuevas: elige al menos una que no domines.

| Capa | Tecnología | Por qué |
| --- | --- | --- |
| Frontend | React + Vite + TypeScript | Estándar del mercado; TypeScript evita errores de tipos con los datos de la API. |
| Estilos (ASCII, M-06) | CSS con fuente monoespaciada | El estilo ASCII es tipografía y bordes; no requiere librería. |
| Gráficas (RF-04, RF-06) | Recharts | Gráficas en React con poco código. |
| Almacenamiento local | IndexedDB con Dexie.js | API sencilla sobre IndexedDB para la cola offline. |
| Backend / API | Python + FastAPI | Validación con Pydantic, docs automáticas y ecosistema de datos. |
| ORM y migraciones | SQLAlchemy + Alembic | Acceso a datos por capas y esquema versionado. |
| Base de datos | PostgreSQL | Relacional, robusta y escalable. |
| Contenedores | Docker + Docker Compose | Mismo entorno en local y en producción. |
| Pruebas | Pytest (backend) · Vitest (frontend) | Pruebas por capa. |
| Hosting | TODO: elegir proveedor de frontend estático, API y Postgres administrado (revisa planes gratuitos vigentes). | — |
| Control de versiones | Git + GitHub | Repositorio del proyecto; a futuro GitHub Actions. |
| App móvil *(futuro)* | TODO: React Native (reutiliza React/TS) o Flutter | — |

---

## 5. Diseño de pantallas (wireframes)

Bocetos de baja fidelidad de cada pantalla. Como la app tendrá estilo ASCII, puedes hacer los wireframes directamente en ASCII: sirven de boceto y de referencia visual.

<img width="1223" height="873" alt="Screenshot 2026-11-19 at 13-28-25 TRAINING LOG — Registro de entrenamiento" src="https://github.com/user-attachments/assets/088237bc-7ca7-48b4-a2b0-b5e9acf22d20" />
<img width="1222" height="1181" alt="Screenshot 2026-11-19 at 13-28-46 TRAINING LOG — Registro de entrenamiento" src="https://github.com/user-attachments/assets/7350f73f-0657-458f-b389-e70df5769521" />
<img width="1225" height="826" alt="Screenshot 2026-11-19 at 13-30-28 TRAINING LOG — Registro de entrenamiento" src="https://github.com/user-attachments/assets/453fb883-faec-4efc-88ae-33f7d99b3d48" />

---

## 6. Flujo offline y sincronización

RNF-02 es el requisito técnico más difícil del MVP. Defínelo ahora para no improvisarlo en el código. *(Si elegiste la opción A de arquitectura, esta sección se reduce a "todo es local").*

**Alcance (sin PWA):** si se pierde la conexión con la app abierta, los cambios se guardan en IndexedDB y se suben con **Subir cambios guardados** (CU-09). La app no abre sin internet.

| Decisión | Respuesta |
| --- | --- |
| Momento de sincronización | Manual, con el botón **Subir cambios guardados** (diagrama de flujo). TODO: decidir si además se intenta automáticamente al volver la conexión. |
| Identificadores | UUID generado en el cliente al crear el registro. |
| Campos técnicos en cada tabla | `created_at`, `updated_at` y `deleted_at` (agrégalos al modelo de la sección 2). |
| Eliminaciones | *Soft delete:* se llena `deleted_at` en lugar de borrar la fila, para que la eliminación también se sincronice. |
| Resolución de conflictos | *Last-write-wins:* gana el cambio con `updated_at` más reciente. Si el servidor tiene uno más nuevo → `409` y se informa al usuario. |
| Indicador visual de pendiente | Botón con el contador de cambios pendientes; se oculta en 0. |

**Formato de un cambio en la cola**

```json
{
  "id": "uuid-del-registro",
  "entidad": "entrenamiento",
  "operacion": "crear | editar | eliminar",
  "datos": { "fecha": "2026-11-19", "hora": "06:00", "tipo_id": "uuid-del-tipo" },
  "updated_at": "2026-11-19T06:05:00-05:00"
}
```

`POST /api/v1/sync` recibe una lista de estos cambios, los procesa en orden y responde el resultado de cada uno (aplicado, rechazado `422` o en conflicto `409`).

---

## 7. Matriz de trazabilidad
Conecta cada requisito con dónde se diseña. Si un requisito no tiene pantalla o tabla, falta diseño; si una pantalla no tiene requisito, sobra.

| Requisito | Caso de uso | Pantalla | Tablas | Prioridad |
| --- | --- | --- | --- | :---: |
| RF-01 | CU-01, CU-02, CU-03  | Principal, Formulario | `entrenamiento` | 🔴 Must |
| RF-02 | CU-02, CU-03  | Principal, Regis Entrenos | `entrenamiento` | 🔴 Must |
| RF-03 | CU-10 | Tipos | `Tipo` | 🔴 Must |
| RF-04 | CU-04 | Principal, Form Metas | `Meta` | 🟠 Should |
| RF-05 | CU-05, CU-06 | Principal, Regis Metas | `Meta` | 🟠 Should |
| RF-06 | CU-07, CU-08 | Stats | `entrenamiento` `Meta` | 🟠 Should |
| RF-07 | CU-09 | Principal, Sincro | `Metas` `entrenamiento` | 🟠 Should |

---

## 8. Plan de implementación

Dividir el MVP en iteraciones cortas que terminen en algo usable. Sigue el orden de MoSCoW: primero los Must.

| Iteración | Objetivo | Incluye (M-xx) | Entregable | Fecha objetivo |
| --- | --- | --- | --- | --- |
| **1** | Registrar entrenamientos | M-01, M-02, M-03 | Puedo registrar desde el celular | Nov 21 |
| **2** | Registrar metas y ver charts | M-04, M-05 | Puedo ver mis estadísticas | Nov 22 |
| **3** | Diseño estilo ASCII | M-06 | Tengo un diseño responsive y agradable | Nov 23 |
