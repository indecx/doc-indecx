# GUÍA DE UTILIZACIÓN DE API

![Badge](https://img.shields.io/badge/integrations-v3-blue)

La API de IndeCX fue desarrollada para que sea posible realizar diversas integraciones entre su sistema y las funciones disponibles en nuestra plataforma. n esta Versión, la API cuenta con una mejora en el proceso de autenticación y autorización mediante el token Bearer, que es un enfoque seguro, simple, escalable, flexible y estandarizado para garantizar la seguridad en las API.

En esta documentación encontrará ejemplos del uso de cada método.

## Introducción
La API utiliza autenticación y autorización de token de portador para proteger las rutas de acceso restringido. El Bearer Token debe enviarse en el encabezado de Autorización de todas las solicitudes que requieren autenticación.

## Autenticación
Para autenticarse con la API, debe enviar una solicitud de autenticación con sus credenciales de usuario (**empresa-clave**) enviada a través del encabezado, disponible dentro de la configuración de la cuenta en la plataforma Indecx. La API devolverá un token de portador, que debe usar para todas las solicitudes futuras que requieran autenticación.

## Punto Final/ Obtener 
![Badge](https://img.shields.io/badge/GET-authorization--token-orange) 
```bash
https://indecx.com/v3/integrations/authorization/token
```
| parámetros	| Descripción |
| ------------- | ------------- |
| company-key  | $2b$10$BLV4CJAYKSTkktvkJTCVj.dM4H3lHKyiSjoRt3npXGxcNljXXXXX  |

### Ejemplo de solicitud 
(Imagen)

### Ejemplo de respuesta 
(imagen)

### Ejemplo de encabezado de autorización
El Bearer Token debe enviarse en el encabezado de Autorización de todas las solicitudes que requieren autenticación. El token es válido por 30 minutos.

(IMAGEN)

### Desencadenador de invitación POST transaccional 
Imagine una situación en la que desea activar una encuesta de satisfacción para cada nueva transacción de cliente dentro de su sistema, ya sea prospección, ventas, interacción, etc. El método de activación de invitación transaccional es posible y muy fácil.

La comunicación se realizará a través del siguiente URL:

(Imagen)

Las acciones deben crearse en la plataforma app-indecx.com y los envió se pueden realizar a través de la plataforma (manual) o a través de la API. Cuando se hace la activación a través de la API, debe enviar un JSON a través del cuerpo a la URL mencionada anteriormente y su autenticación se realizará a través de la clave de empresa proporcionada y enviada a través de HEADER. Es importante que se envíe el identificador de la acción y que siga un patrón de envío del JSON a través del cuerpo.

### Configuración JSON

**Importante:** El correo electrónico y el teléfono son campos obligatorios, según la configuración del tipo de activador. Consulta la configuración realizada al crear la acción. Para poder enviar datos a las columnas adicionales (indicadores) es necesario crear los indicadores dentro de la acción previamente y usar exactamente los mismos nombres creados dentro de la plataforma.  En nuestro ejemplo a continuación, las columnas adicionales son: Sucursal, Área, Región y Tipo.

### PEDIDO

(IMAGEN)

### RESPUESTA

(IMAGEN)

Para tomas programadas, la invitación tendrá un estado **programado** en el índice y, si es necesario, también es posible cancelar la toma directamente a través de la plataforma en el menú Tomas>>Agendamientos

**Importante:** El campo “programación” utiliza el estándar UTC, por lo que es posible programar tomas con horarios locales en otros países. El parámetro de ejemplo **"2020-12-16T20:20:00.000"** se activará en los siguientes horarios: Brasil 17:20:00 (UTC - 3), EE. UU. Nueva York 16:20:00 (UTC - 5)

### POST envíos de invitación por lotes 

(IMAGEN)

Las acciones deben crearse en la plataforma app-indecx.com y los envíos se pueden realizar a través de la plataforma o a través de la API. Cuando se activa a través de API, debe enviar un JSON a través del cuerpo a la URL mencionada anteriormente y su autenticación se realizará a través de la clave proporcionada a la empresa y enviada a través de HEADER. Es importante que se envíe el identificador de la acción y que siga un patrón de envío del JSON a través del cuerpo.

### Configuración JSON
**Importante** El correo electrónico y el teléfono son campos obligatorios, según la configuración del tipo de activador. Consulta la configuración realizada al crear la acción. Para poder enviar datos a las columnas adicionales (indicadores) es necesario crear los indicadores dentro de la acción previamente y usar exactamente los mismos nombres creados dentro de la plataforma. **Esta API tiene un límite máximo de 1.000 clientes por solicitud.**

(Imagen)
### Respuesta 

(Imagen)

Posibles devoluciones 

| Código | Descripción |
| ------------- | ------------- |
| 200	| ¡Envíos configurados con éxito! |
| 400	| Acción no encontrada. |
| 400	| Falta la clave API. |
| 400	| Informe al menos a un cliente. |
| 401	| No se encontró la compañía para esta clave API. |
| 401	| Clave API no válida. |
| 401	| Esta acción está bloqueada para integraciones. |
| 401	| Esta acción está en pausa. |
| 402	| Saldo de correo electrónico insuficiente. |
| 402	| Saldo de mensaje insuficiente. |
| 500	| Error Interno del Servidor |

### OBTENER Recopilar respuestas 
(imagen/link)

También puede integrar las respuestas recopiladas a través de indecx en su sistema. Para ello, deberá utilizar el método "Recopilar respuestas" para acceder a todas las respuestas y detalles de los clientes.

La comunicación se realizará a través de la siguiente URL:

(link)

### Parámetros de consultas 

| parámetros	| Descripción |
| ------------- | ------------- |
| página	| Devuelve los resultados de una página específica | 
| límite	| Devuelve un valor límite por página |
| fecha de inicio	| Fecha de inicio del parámetro |
| fecha final	| Fecha de finalización del parámetro |
| tipo de fecha	| Tipo de fecha createdAt (creación de búsqueda) o updatedAt (actualización de búsqueda) |
| correo electrónico	| Parámetro de correo electrónico del encuestado |
| teléfono	| Parámetro del teléfono del encuestado |

Nota: Para el valor [Action_Identifier], el parámetro "/all" se puede usar para devolver todas las respuestas de todas las acciones disponibles.

Ejemplo de consulta: 
(Imagen)

### Respuesta

(Imagen)

### Comprender los campos de devolución 

| parámetros |	Descripción |
| ------------- | ------------- |
| `_Identificación` |	ID de respuesta |
| activo |	estado |
| Fecha	| Fecha y hora de respuesta |
| Fecha de respuesta	| Fecha y hora de respuesta |
| Respuesta anónima	| Identificación si el cliente quiere identificarse o no. |
| Identificación del cliente	| Identificador único del cliente |
| Nombre	| Nombre del cliente |
| Correo electrónico	| Correo electrónico del cliente |
| Teléfono	| Teléfono del cliente |
| Comentario	| Comentario realizado por el cliente en el momento de la encuesta |
| categorías	| Categorización de comentarios (feedback) |
| Canal	| Canal de recogida de respuestas |
| ID de la compañía	| ID de la compañía |
| ID de acción	| identificador de acción |
| ID de invitación	| identificación de invitación |
| ID de detalles	| ID de detalles de importación |
| Texto	| Pregunta métrica clave |
| Revisar	|Puntuación asignada en la métrica principal |
| Métrico	| métrica principal |
| Creado en	| Fecha de creación de la respuesta |
| Actualizar en	| Fecha de actualización de la respuesta |
| Eliminado	| Comprueba si la respuesta fue eliminada |

### Preguntas adicionales

| parámetros |	Descripción |
| ------------- | ------------- |
| `_Identificación`	| identificación de la pregunta |
| Tipo	| Tipo de pregunta RESEÑAS, ME GUSTA/NO ME GUSTA, CSAT, EMOCIÓN, MÚLTIPLES, ENTRADA |
| Texto	| Pregunta Descripción |
| revisar	| grado asignado |
| Responder texto	| Retorno de texto en caso de pregunta adicional de tipo **INPUT** |
| multipleValues	| Devolución de respuestas múltiples en caso de tipo de pregunta adicional **MÚLTIPLE** |

### Indicadores
| parámetros |	Descripción |
| ------------- | ------------- |
| `_identificación` |	identificación del indicador |
| Columna	| nombre del indicador |
| Valor	| valor del indicador |

### sentimientoAnalyzeGCP
| parámetros	| Descripción | 
| ------------- | ------------- |
| puntaje	| puntaje de sentimiento |
| frases clave	| Palabras claves identificadas en los comentarios de los clientes |

### Tratos
| parámetros	| Descripción |
| ------------- | ------------- |
| estado	| Estado de negociación |
| patrocinador	| Nombre de la persona a cargo |
| comentarios	| Comentario registrado por el responsable |
| Fecha	| fecha de negociación |
| categorías	| Categorías incluidas en las respuestas |

### OBTENER Recopilar informaciones de invitación

(LINK)

Puede acceder a la lista de todos los clientes importados a la plataforma con estado si la invitación ya fue respondida o no.

La comunicación se realizará a través de la siguiente URL:

(LINK)

### Parámetros de consulta
| parámetros	| Descripción |
| ------------- | ------------- |
| página	| Devuelve los resultados de una página específica |
| límite	| Devuelve un valor límite por página |
| fecha de inicio	| Fecha de inicio del parámetro |
| fecha final	| Fecha de finalización del parámetro |
| tipo de fecha	| Tipo de fecha createdAt (creación de invitación) o updatedAt (actualización de invitación) |
| correo electrónico	| Parámetro de correo electrónico de invitación |
| teléfono	| Parámetro de teléfono de invitación |

Nota: Para el valor [Identifier_da_acao] se puede utilizar el parámetro "/all" para devolver todas las invitaciones de todas las acciones disponibles.

Ejemplo de consulta:

(IMAGEN/LINK)

### RESPUESTA

(IMAGEN)

### Comprender los campos de devolución
| parámetros	| Descripción |
| ------------- | ------------- |
| `_identificación`	| identificación de invitación |
| contestada	| Comprueba si la invitación fue respondida |
| activo	| Comprueba si la invitación está activa |
| ID de acción	| identificador de acción |
| nombre	| Nombre del cliente |
| correo electrónico	| correo electrónico del cliente |
| teléfono	| teléfono del cliente |
| URL corta	| Dirección del enlace de la encuesta |
| email valido	| validador de correo electrónico |
| teléfono válido	| validador de teléfono |
| Creado en	| Fecha de creación de la invitación |
| actualizar en	| Fecha de actualización de la invitación |
| ID de respuesta	| ID de respuesta |
| Eliminado/Respuesta	| Comprueba si la respuesta fue eliminada |

### GET recopilación de clientes que no responden 
(LINK)

A través de este método, puede acceder a una lista de todos los clientes que aún no han respondido a su encuesta. La comunicación se realizará a través de la siguiente URL:
(link)

### Parámetros de Consulta 
| parámetros	| Descripción |
| ------------- | ------------- |
| página	| Devuelve los resultados de una página específica |
| límite	| Devuelve un valor límite por página |
| fecha de inicio	| Fecha de inicio del parámetro |
| fecha final	| Fecha de finalización del parámetro |
| correo electrónico	| Buscar correo electrónico de un usuario en específico |

Ejemplo de consulta:

(imagen)

recopilación de respuestas con un límite de paginación y un retorno de carga útil máximo de 1000 registros.

### Respuesta

(imagen)

### Comprender los campos de devolución 
| parámetros	| Descripción |
| ------------- | ------------- |
| `_identificación`	| ID de respuesta |
| contestada	| estado de respuesta |
| Reenviar/Fechas	| Lista de todas las fechas para las que se reenvió la invitación |
| activo	| estado de la invitación |
| Recordatorio/Fechas	| Lista de todas las fechas para las que hubo recordatorios |
| ID de control	| Identificador de control de invitación |
| actionId.nombre	| Nombre de la acción asignada a la invitación |
| actionId.controlId	| Identificador de control de acción |
| nombre	| Nombre del cliente |
| correo electrónico	| correo electrónico del cliente |
| teléfono	| teléfono del cliente |
| URL corta	| enlace de encuesta al cliente |
| email valido |	Validador de campo de correo electrónico |
| teléfono válido |	validador de campo de teléfono |
| indicadores	| Lista de indicadores asignados a la invitación |
| métrico	| Métrica de invitación clave |
| Creado en	| ? |
| página	| paginación de retorno |
| límite	| Límite máximo de devolución de consultas |
| total	| Total, de registros consultados |

### WEBHOOK recopilar respuestas

También puedes registrar tu endpoint dentro de la plataforma Indecx para que cada nueva respuesta sea enviada automáticamente a tu sistema. Ingresando a la plataforma Indecx, vaya a: **Configuración >> Integraciones >> Recopilación de respuestas**

Con cada nueva respuesta recibirá una devolución con toda la información del cliente y las respuestas asignadas por él, como se muestra en el siguiente ejemplo:

### Respuesta

(imagen)

### POST generación de un enlace de encuesta 
(link)

Con la ruta a continuación, se podrá integrar con la plataforma Indecx, donde el objetivo es crear una invitación y devolver el enlace en la respuesta de llamada, para que pueda ser utilizado dentro de los medios de contacto con el cliente, este enlace será único para cada nueva invitación generada.

La comunicación se realizará a través de la siguiente URL:

(link)	

las acciones deben crearse en la plataforma app-indecx.com y los envíos se pueden realizar a través de la plataforma o a través de la API. Cuando se active a través de API, debe enviar un JSON a través del cuerpo a la URL mencionada anteriormente y su autenticación se realizará a través de la clave proporcionada a la empresa y enviada a través de HEADER. Es importante que se envíe el identificador de la acción y que siga un patrón de envío del JSON a través del cuerpo

### Pedido 

(imagen)

### RESPUESTA

(imagen)

### Generación de un enlace de encuesta con URL de devolución de llamadas 

También es posible enviar un disparador y obtener el enlace a través de la API registrada en la callbackurl.
ruta API **callbackurl**
**maxmsg** Retorno máximo de registros en JSON. Ejemplo: si activa 1000 clientes con maxmsg = 100, recibirá 10 solicitudes con 100 registros cada una.

### Pedido

(imagen)

### RESPUESTA

(imagen)

### URL de devolución de llamadas y respuestas 

(imagen)

### POST Enviar transacciones de respuestas a Indecx 

 (lINK)

Con la ruta a continuación, será posible enviar una respuesta recopilada en su APP o sistema de manera transaccional al sistema Indecx. De esta forma, es posible centralizar todos los análisis y respuestas dentro de Indecx y analizar los resultados con los informes disponibles en la plataforma.
La comunicación se realizará a través de la siguiente URL:

(LINK)

Para que este recibo por parte de Indecx sea posible, es necesario tener una acción creada con configuraciones de cuestionario compatibles con lo que fue respondido por el cliente.

### Pedido

(imagen)

### RESPUESTA

(imagen)

### Comprender los campos de solicitud
| Parámetros	| Descripción |
| ------------- | ------------- |
| ID de invitación	| identificación de invitación |
| nombre	| nombre del encuestado |
| correo electrónico	| correo electrónico del encuestado |
| teléfono	| teléfono del encuestado |
| revisar	| Puntuación asignada a la métrica principal de la encuesta |
| comentario	| Campo de comentario del cliente |
| preguntas adicionales/tipo	| Tipo de pregunta adicional, por ejemplo: RESEÑAS, CSAT, ME GUSTA/NO ME GUSTA, EMOCIÓN, MÚLTIPLE, ENTRADA |
| preguntas adicionales/texto	| Descripción adicional de la pregunta |
| Indicadores/columna	| nombre del indicador |
| Indicadores/valor	| valor del indicador |

***Importante:** El campo inviteId solo se usa cuando usamos una combinación de API de generación de enlaces + API de respuesta transaccional, y no es obligatorio para casos de inclusión de respuesta simple. Para obtener más información sobre cómo usar el inviteId, comuníquese con el equipo de soporte de Indecx.

### Comprender las opciones de respuesta por tipo
| Parámetros	| Descripción |
| ------------- | ------------- |
| RESEÑAS	| Escala de 5 puntos (1 a 5) en forma de "estrellas" | 
| CSAT	| Escala de 10 puntos (1 a 10) en forma de escala de evaluación |
| GUSTAR DISGUSTAR	| Escala booleana (0 y 1) en forma de icono positivo y negativo |
| ME GUSTA	| Escala de 5 puntos (1 a 5) donde 1 = "Muy Insatisfecho", 2 = "Insatisfecho", 3 = "Indiferente", 4 = "Satisfecho" y 5 = "Muy Satisfecho" |
| EMOCIÓN	| Escala booleana (0 y 1) en forma de icono emoji positivo y negativo |
| MÚLTIPLE	| Escala de selección única o de opción múltiple. |
| APORTE	| Campo abierto para la inclusión de la respuesta. |

***Importante:** Los valores recibidos deben estar de acuerdo con las opciones de respuesta disponibles en cada métrica para que se pueda realizar la integración.

### Lista de posibles errores
| Código	| Descripción |
| ------------- | ------------- |
| 400	| "mensaje": "Tipo de métrica no válido. Permitido: RESEÑAS, CSAT, ME GUSTA/NO ME GUSTA, EMOCIÓN, MÚLTIPLE, ENTRADA, CES" |
| 400	| "mensaje": "Respuesta inválida para la pregunta XXXX..." |
| 400	| "mensaje": "La invitación ya ha sido respondida" |
| 500	| "mensaje": "Error interno del servidor" |

### GET Lista de acciones activas en Indecx
(link)
Para acceder a la lista de todas las acciones activas disponibles en la cuenta, use la ruta a continuación.

(imagen)

### RESPUESTA

(imagen)

### OBTENER Recopilar información de la encuesta
(link)

Acceder a la estructura del cuestionario programado dentro de Indecx. La comunicación se realizará a través de la siguiente URL:

(imagen)

### RESPUESTA

(imagen)

### Comprender los campos de devolución
| Parámetros	| Descripción |
| ------------- | ------------- |
| ´_identificación´	| identificador de acción |
| ID de control	| Identificador de control de acción |
| Reenviar | Fechas	Lista de todas las fechas de reenvío| 
| tipo de encuesta	| tipo de cuestionario |
| nombre	| Nombre del cuestionario |
| tipo	| Métrica principal del cuestionario |
| pregunta	| Pregunta métrica clave |
| Habilitar Anónimo	| Opción de anonimato de datos habilitada |
| preguntas adicionales/tipo	| Tipo de pregunta adicional, por ejemplo: RESEÑAS, CSAT, ME GUSTA/NO ME GUSTA, EMOCIÓN, MÚLTIPLE, ENTRADA |
| preguntas adicionales/texto	| Descripción adicional de la pregunta |
| preguntas adicionales/tipo múltiple |	Tipo de selección única (radio) u opción múltiple (casilla de verificación) |
| preguntas adicionales/opciones	| Opciones de elección para múltiples tipos. |
| todas las respuestas Requeridas	| Respuestas de tipo obligatorio. |

### Comprender las opciones de respuesta por tipo
| Parámetros	| Descripción |
| ------------- | ------------- |
| RESEÑAS	| Escala de 5 puntos (1 a 5) en forma de "estrella" |
| CSAT	| Escala de 10 puntos (1 a 10) en forma de escala de evaluación |
| GUSTAR DISGUSTAR	| Escala booleana (0 y 1) en forma de icono positivo y negativo |
| ME GUSTA	| Escala de 5 puntos (1 a 5) donde 1 = "Muy Insatisfecho", 2 = "Insatisfecho", 3 = "Indiferente", 4 = "Satisfecho" y 5 = "Muy Satisfecho" |
| EMOCIÓN	| Escala booleana (0 y 1) en forma de icono emoji positivo y negativo |
| MÚLTIPLE	| Escala de selección única o de opción múltiple. |
| APORTE	| Campo abierto para la inclusión de la respuesta. |

### POST Enviar cliente a la lista de bloqueo
(link)

Si desea agregar una lista de clientes a la lista de bloqueo para evitar que este correo electrónico reciba nuevos contactos, puede usar esta ruta.
La comunicación se realizará a través de la siguiente URL:
(Link)

### Pedido

(imagen)

### RESPUESTA

(Imagen)

### OBTENER Recopilar información de la lista de bloqueo (no quiero recibir más contactos)

(LINK)

También puede tener acceso a la lista de todos los clientes que ingresaron al blaclist:

(imagen)

### Parámetros de consulta     

| Parámetros	| Descripción |
| ------------- | ------------- |
| página	| Devuelve los resultados de una página específica |
| límite	| Devuelve un valor límite por página |

### RESPUESTA

(Imagen)

### Comprender los campos de devolución
| Parámetros	| Descripción |
| ------------- | ------------- |
| actionName	| Compartir nombre |
| actionControlId	| identificador de acción |
| ID de invitación	| identificación de invitación |
| razón	| Motivo de la inclusión en la lista de bloqueo |
| correo electrónico	| correo electrónico del cliente |
| Creado en	| Fecha de inclusión del registro en la lista de bloqueo |

### GET Recopilar una lista de respuestas categorizadas
(link)

También puede acceder a la lista de todas las categorizaciones asignadas.

(Imagen)

### Parámetros de consulta
| Parámetros	| Descripción |
| ------------- | ------------- |
| página	| Devuelve los resultados de una página específica |
| límite	| Devuelve un valor límite por página |
| fecha de inicio	| Fecha de inicio del parámetro |
| fecha final	| Fecha de finalización del parámetro |
| tipo de fecha	| creado en o actualizado en |

Nota: Para el valor [Identifier_da_acao] se puede utilizar el parámetro "/all" para devolver todas las invitaciones de todas las acciones disponibles.

Ejemplo de consulta:
(imagen)

### RESPUESTA

(Imagen)

### Comprender los campos de devolución
| Parámetros	| Descripción |
| -- | -- |
| ID de respuesta	| ID de respuesta |
| ID de invitación	| identificación de invitación |
| categoría	| Nombre de categoría asignado |
| subcategoría	| Nombre de subcategoría asignado |
| Creado en	| Fecha de inclusión de la respuesta |
| actualizado en	| Fecha de actualización de la respuesta |
| tipo de fecha	| Fecha de inclusión de la respuesta |

### Sucursal de registro POST (IH1)
(link/imagen)

También puede registrar una sucursal o unidad de negocio a través de la API. Esta rama, denominada Ih1 en la plataforma, se utilizará para separar datos y niveles de acceso por usuario.
(imagen)

### Solicitar json

(Imagen)

### RESPUESTA

(Imagen)

### Comprender los campos de devolución
| Parámetros	| Descripción |
| - | - |
| ´_identificación´	| identificación de la sucursal |
| activo	| estado |
| nombre corporativo	| nombre de la sucursal |
| tipo	| Tipo de sucursal |

### GET Lista de Sucursales Registradas (IH1)
(link/imagen)

La API permite el acceso a la lista de todas las unidades registradas en la plataforma.

(imagen)
### RESPUESTA

(Imagen)

Gracias💚
¿Existe alguna ruta que te haga el día a día más fácil?? ¡Ponte en contacto con el equipo de Indecx CX que desarrollamos para ti! =) Actualizado el 01/01/2023
