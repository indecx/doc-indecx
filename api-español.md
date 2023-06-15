# GUÍA DE UTILIZACIÓN DE API

La API de IndeCX fue desarrollada para que sea posible realizar diversas integraciones entre su sistema y las funciones disponibles en nuestra plataforma. n esta Versión, la API cuenta con una mejora en el proceso de autenticación y autorización mediante el token Bearer, que es un enfoque seguro, simple, escalable, flexible y estandarizado para garantizar la seguridad en las API.

En esta documentación encontrará ejemplos del uso de cada método.

### INTRODUCCIÓN
La API utiliza autenticación y autorización de token de portador para proteger las rutas de acceso restringido. El Bearer Token debe enviarse en el encabezado de Autorización de todas las solicitudes que requieren autenticación.

### Autenticación
Para autenticarse con la API, debe enviar una solicitud de autenticación con sus credenciales de usuario (**empresa-clave**) enviada a través del encabezado, disponible dentro de la configuración de la cuenta en la plataforma Indecx. La API devolverá un token de portador, que debe usar para todas las solicitudes futuras que requieran autenticación.

### Punto Final/ Obtener 
(Imagen) 

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
