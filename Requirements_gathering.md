# Requirements Gathering
**Problema**: No existe un registro actual de mis entrenamientos ni informacion que me sirva para tomar acciones y descisiones sobre mi progreso como deportista.
**Propuesta**: Diseñar una App diseñada para el uso diario con el fin de registrar y almacenar informacion sobre mis entrenamientos.
**Caso de exito**: Uso la aplicacion diariamente para requistrar mis entrenamientos, y tomar desciciones en base a mis datos.

## 1. Usuarios y Stakeholders
La app de uso personal está diseñada para un deportista de alto rendimiento con 1 o mas sesiones diarias.
**Perfil de Usuario**.
Dispositivo principal: Celular y PC
Contexto de uso: Antes o después de entrenar
Frustraciones actuales: No tener un control sobre los días de entrenamiento.

**Stakeholders**.
Desarrollador.
- Un desarrollo simple, dinámico, modulado, escalable y con el objetivo de aprender y practicar nuevas tecnologías.
- Iniciando en el desarrollo, por lo que va a ser un proyecto altamente didáctico.
Usuario.
- Solicita una aplicacion de facil acceso, facil manejo, y facil registro para cualquiera de sus actividades.
- Necesita simplicidad y un estilo ASCII.

## 2. Alcance del MVP
| v1     | Futuras Versiones |
| -------- | ------- |    
Registro de entrenamientos   | Widgets para el celular |
Total de entrenamientos por semana/mes/tipo | Entrar a detalle con el ejercicio, % de carga, reps, pesos y tiempos de descanso.  |
|  | Competir online con otros usuarios |
|  | Tematica tipo ASCII |
|  | Subir metas a x dias |
|  | Comparar metas con entrenamientos hechos en graficas |

## 3. Requisitos funcionales
| ID    | Regla | Entonces |
| -------- | ------- | ------- |
| RF-01 | Quiero guardar registros de mis entrenamientos | Va a haber un boton '+' para añadir un nuevo entrenamiento |
| RF-02 | Quiero añadir/editar fecha/hora/tipo de algún entrenamiento | Cuando se agregue un nuevo entrenamiento, va a haber un formulario con 3 campos obligatorios antes de enviar; este mismo formulario debería servir para cuando se quiera editar la información del entrenamiento en un listado de la app de los entrenamientos hechos. |
| RF-03 | Quiero eliminar algun entrenamiento si lo necesito | en la misma pestaña de editar entrenamiento va a haber un boton de basura para eliminarlo |
| RF-04 | Quiero ver el total de entrenamientos de la semana/mes | Entonces va ahabaer una pestaña de estadisticas con una grafica filtrable con el total de entrenamientos por mes / semana |
| RF-05 | Quiero poder subir una meta de entrenamientos por x cantidad de días | En la pestaña principal va a haber un botón de agregar meta, donde va a poner la cantidad de entrenos por tipo, y la cantidad de días |
| RF-06 | Quiero ver una comparacion entre metas y entrenamientos | Entonces en la pestaña de estadisticas va a haber una grafica que se va a filtrar por meta, y va a comparar la cantidad de entrenameintos hechos x tipo con los planeados |
| RF-07 | Quiero editar mis metas o eliminarlas | En la misma pestañas principal se va a poder listar las metas y se van a poder editar / eliminar |

## 4. Requisitos no funcionales
| ID    | Categ | Requisito |
| -------- | ------- | ------- |
| RNF-01 | Plataforma | Una App Web Responsive optimizada |
| RNF-02 | Offline | Debe almenos guardar en local y subir cuando tengamos internet |
| RNF-03 | Rendimiento | La funcion de agregar entreno debe de ser particularmente rapida y accesible, el resto tambien, per ono tan prioritorio, menos de 3seg de respuesta |
| RNF-04 | Usabilidad | Debe de ser intuitiva y facil de usar |
| RNF-05 | Seguridad | No necesita login o seguridad de momento porque es plenamente personal |

## 5. Glosario
| Entidad | Def | Atributos | Relaciones |
| -------- | ------- | ------- |------- |
| Entrenamiento | Una sesion de ejercicios enfocados | Hora / Dia / Tipo | 1 Entreno a 1 tipo |
| Meta | Cant de entrenamientos en x dias de uno o varios tipos | fecha inicio / fecha final / cant / tipo | 1 meta a N tipos |
| Tipo | Clasificacion del Entrenamiento | Nombre | 1 Tipo a N Entrenamientos y N Tipos a N Metas |
<img width="1920" height="1080" alt="Entrenamiento" src="https://github.com/user-attachments/assets/bd718d69-5537-475f-a5ac-79aded530bc7" />

## 6. Reglas de negocio
| ID | Regla | 
| -------- | ------- |
| RN-01 | Por defecto cuando se cree un nuevo Entreno debe de salir el formulario con el dia y fecha actual |
| RN-02 | Para guardar un Entrenamiento hay que tener todos los campos llenos, son obligatorios |
| RN-03 | Para una meta, la Fecha_fin debe de ser superior a la Fecha_ini |

## 7. MoSCoW
| ID | Requisito | Prioridad | Esfuerzo estimado |
| -------- | ------- | -------- | ------- |
| RF-01 | BD de Tipos | Must | 1h |
| RF-02 | CRUD Entrenamientos | Must | 2h |
| RF-03 | Adaptado para celular | Must | 1.30h |
| RF-04 | CRUD de Metas | Should | 3h |
| RF-05 | Graficas de sumas y comparacion | Should | 2h |
| RF-06 | Estilo ASCII | Could | 2h |
| RF-07 | Entrar a detalle con los Ejercicios | Won't | 5h |
| RF-08 | Competencia Online | Won't | 5h |
| RF-09 | App para celular con widgets | Won't | 5h |

## 8. Supuestos, Riesgos y preguntas abiertas

| Riesgo | Probabilidad | Impacto | Mitigacion |
| -------- | ------- | -------- | ------- |
| Siempre voy a tener el celular disponible | Bajo | No voy a poder registrar | Registro apenas tenga disponibilidad |
| Solo voy a tener 3 categorias para siempre | Bajo | No podre ver el Tipo que necesito | Se puede programas en bs |
| No me voy a lesionar | Media | No necesitare la App, o tal vez si para el proceso de recuperacion | Entrenamientos inteligentes y adaptar la App |
| Siempre voy a tener acceso a gimnasio e implementos | Media | Algunos ejercicios no los voy a poder hacer | Cambianos a ejercicos de calistenia o demas |

## 9. Cierrer de la fase





