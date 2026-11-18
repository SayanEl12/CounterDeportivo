# Requirements Gathering — App de Entrenamientos

> **Fase 1 · Levantamiento de requisitos**
> Autor: Juan José Palacio Madrid · Versión: 1.0 · Fecha: 2026-11-18 · Estado: ✅ Cerrada

---

## Contenido

0. [Visión](#0-visión)
1. [Usuarios y stakeholders](#1-usuarios-y-stakeholders)
2. [Alcance del MVP](#2-alcance-del-mvp)
3. [Requisitos funcionales](#3-requisitos-funcionales)
4. [Requisitos no funcionales](#4-requisitos-no-funcionales)
5. [Modelo de dominio](#5-modelo-de-dominio)
6. [Reglas de negocio](#6-reglas-de-negocio)
7. [Priorización MoSCoW](#7-priorización-moscow)
8. [Supuestos, riesgos y preguntas abiertas](#8-supuestos-riesgos-y-preguntas-abiertas)
9. [Cierre de la fase](#9-cierre-de-la-fase)

---

## 0. Visión

| | |
| --- | --- |
| **Problema** | No tengo un registro de mis entrenamientos ni información que me ayude a tomar decisiones sobre mi progreso como deportista. |
| **Propuesta** | Una app de uso diario para registrar y almacenar mis entrenamientos. |
| **Caso de éxito** | Uso la app todos los días para registrar mis entrenamientos y tomo decisiones con base en mis datos. |

---

## 1. Usuarios y stakeholders

App de uso personal para un deportista de alto rendimiento que entrena **una o más sesiones al día**.

### Perfil de usuario

| Campo | Descripción |
| --- | --- |
| Dispositivo principal | Celular y PC |
| Contexto de uso | Antes o después de entrenar |
| Frustración actual | No tener control sobre los días que entreno |

### Stakeholders

| Stakeholder | Necesidades |
| --- | --- |
| **Usuario** | Acceso, manejo y registro fáciles para cualquier actividad. Interfaz simple con estilo ASCII. |
| **Desarrollador** | Desarrollo simple, modular y escalable. Proyecto didáctico para aprender y practicar nuevas tecnologías. |

---

## 2. Alcance del MVP

| ✅ v1 — Obligatorio (Must) | 🟡 v1 — Si hay tiempo (Should / Could) | ⏳ Versiones futuras (Won't) |
| --- | --- | --- |
| Registro de entrenamientos (CRUD) | Metas de entrenamientos por *x* días | Detalle por ejercicio: % de carga, reps, pesos y descansos |
| Catálogo de tipos de entrenamiento | Gráfica de totales por semana / mes / tipo | Competir en línea con otros usuarios |
| Diseño adaptado a celular | Gráfica de metas vs. entrenamientos hechos | Widgets para el celular |
| | Temática visual ASCII | |

---

## 3. Requisitos funcionales

Formato: **Como** usuario, **quiero**… · **Criterio de aceptación**: lo que debe pasar para darlo por cumplido.

| ID | Quiero… | Criterio de aceptación |
| --- | --- | --- |
| **RF-01** | Guardar el registro de un entrenamiento. | Un botón **`+`** en la pantalla principal abre el formulario de nuevo entrenamiento. |
| **RF-02** | Agregar y editar la fecha, hora y tipo de un entrenamiento. | El formulario tiene 3 campos obligatorios (fecha, hora, tipo). El mismo formulario sirve para editar desde el listado de entrenamientos. |
| **RF-03** | Eliminar un entrenamiento. | En el formulario de edición hay un botón 🗑️ para eliminarlo. |
| **RF-04** | Ver el total de entrenamientos por semana o mes. | La pestaña **Estadísticas** muestra una gráfica filtrable por semana / mes. |
| **RF-05** | Crear una meta de entrenamientos para *x* días. | La pantalla principal tiene un botón **Agregar meta** que pide la cantidad de entrenamientos por tipo y el rango de días. |
| **RF-06** | Comparar mis metas con los entrenamientos hechos. | **Estadísticas** muestra una gráfica, filtrable por meta, de entrenamientos hechos vs. planeados por tipo. |
| **RF-07** | Editar o eliminar mis metas. | La pantalla principal lista las metas con opciones para editar y eliminar. |

---

## 4. Requisitos no funcionales

| ID | Categoría | Requisito |
| --- | --- | --- |
| **RNF-01** | Plataforma | App web responsive, optimizada para celular. |
| **RNF-02** | Offline | Guarda los datos en local sin conexión y los sincroniza al recuperar internet. |
| **RNF-03** | Rendimiento | Toda acción responde en **< 3 s**. Agregar un entrenamiento es la acción prioritaria: accesible desde la pantalla principal. |
| **RNF-04** | Usabilidad | Interfaz intuitiva y fácil de usar. |
| **RNF-05** | Seguridad | Sin login por ahora: la app es de uso estrictamente personal. |

---

## 5. Modelo de dominio

| Entidad | Definición | Atributos | Relaciones |
| --- | --- | --- | --- |
| **Entrenamiento** | Una sesión de ejercicios con un enfoque. | `fecha`, `hora`, `tipo` | Cada entrenamiento tiene **1** tipo. |
| **Tipo** | Clasificación del entrenamiento. | `nombre` | 1 tipo → **N** entrenamientos · N tipos ↔ **N** metas |
| **Meta** | Cantidad de entrenamientos de uno o varios tipos en un rango de días. | `fecha_inicio`, `fecha_fin`, `cantidad`, `tipo` | 1 meta → **N** tipos |

<img width="1920" height="1080" alt="Entrenamiento" src="https://github.com/user-attachments/assets/bd718d69-5537-475f-a5ac-79aded530bc7" />

---

## 6. Reglas de negocio

| ID | Regla |
| --- | --- |
| **RN-01** | Al crear un entrenamiento, el formulario trae por defecto la fecha y hora actuales. |
| **RN-02** | Todos los campos del entrenamiento son obligatorios para poder guardarlo. |
| **RN-03** | En una meta, `fecha_fin` debe ser posterior a `fecha_inicio`. |

---

## 7. Priorización MoSCoW

| ID | Funcionalidad | Cubre | Prioridad | Esfuerzo |
| --- | --- | --- | :---: | :---: |
| **M-01** | Base de datos de tipos | RF-02 | 🔴 Must | 1 h |
| **M-02** | CRUD de entrenamientos | RF-01, RF-02, RF-03 | 🔴 Must | 2 h |
| **M-03** | Diseño adaptado a celular | RNF-01 | 🔴 Must | 1,5 h |
| **M-04** | CRUD de metas | RF-05, RF-07 | 🟠 Should | 3 h |
| **M-05** | Gráficas de totales y comparación | RF-04, RF-06 | 🟠 Should | 2 h |
| **M-06** | Estilo ASCII | RNF-04 | 🟡 Could | 2 h |
| **M-07** | Detalle por ejercicio | — | ⚪ Won't | 5 h |
| **M-08** | Competencia en línea | — | ⚪ Won't | 5 h |
| **M-09** | App móvil con widgets | — | ⚪ Won't | 5 h |

**Esfuerzo del MVP (Must + Should + Could): 11,5 h**

---

## 8. Supuestos, riesgos y preguntas abiertas

### Supuestos y riesgos

Probabilidad = qué tan probable es que el supuesto **falle**.

| Supuesto | Probabilidad | Impacto si falla | Mitigación |
| --- | :---: | --- | --- |
| Siempre tendré el celular disponible. | Baja | No podré registrar en el momento. | Registrar apenas tenga el celular (la fecha y hora son editables). |
| Solo necesitaré 3 tipos de entrenamiento. | Baja | No podré registrar el tipo que necesito. | Los tipos viven en la BD y se pueden agregar nuevos. |
| No me voy a lesionar. | Media | Dejaré de usar la app, o la necesitaré para registrar la recuperación. | Entrenar de forma inteligente y adaptar la app (p. ej., un tipo "Recuperación"). |
| Siempre tendré acceso a gimnasio e implementos. | Media | No podré hacer algunos ejercicios. | Cambiar a calistenia u otras alternativas. |

### Preguntas abiertas

- [ ] ¿Cuáles son los 3 tipos de entrenamiento iniciales?
- [ ] ¿Cómo se mide RNF-04 (usabilidad)? Propuesta: registrar un entrenamiento en ≤ 3 toques.

---

## 9. Cierre de la fase

- [x] La visión cabe en pocas frases y tiene un caso de éxito.
- [x] El alcance separa v1 de versiones futuras.
- [x] Cada requisito funcional tiene ID y criterio de aceptación.
- [ ] Cada requisito no funcional es medible *(pendiente: RNF-04)*.
- [x] Las entidades del dominio y sus relaciones están definidas.
- [x] Todas las funcionalidades tienen prioridad MoSCoW.
- [ ] No quedan preguntas abiertas que bloqueen un Must *(pendiente: tipos iniciales)*.

➡️ **Siguiente fase:** [Análisis y diseño](./Analysis_Design.md)
