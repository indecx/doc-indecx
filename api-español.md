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
