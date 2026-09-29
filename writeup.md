<!-- ============================================================ -->
<!-- CARÁTULA -->
<!-- ============================================================ -->

<div align="center">

# Universidad Católica del Uruguay

### Desarrollo de Software Seguro

# Práctico 3 - OWASP Juice Shop y OWASP crAPI



**Belén Kanas** 

**Profesores:** 

Wiler Alvez

Nicolás Piquerez

Alejandro Piccardo

Leonardo Conde

**10 de Octubre de 2026**

----------
</div>


<div style="page-break-after: always;"></div>

<!-- ============================================================ -->
<!-- Índice -->
<!-- ============================================================ -->

<div align="justify">

## Índice

* [Introducción](#introduccion)
* [Desafíos Juice Shop](#desafíos-juice-shop)
* [Desafíos crAPI](#desafíos-crapi)
* [Conclusiones](#conclusiones)


----------
</div>


<div style="page-break-after: always;"></div>

<a name="introduccion"></a>

## Introducción

El objetivo de este práctico es identificar, explotar y documentar vulnerabilidades representativas del OWASP Top 10 Web y del OWASP Top 10 API, utilizando dos aplicaciones deliberadamente inseguras provistas para tal fin:

* **OWASP Juice Shop**: una aplicación web de e-commerce que contiene, de forma intencional, vulnerabilidades representativas de las categorías del OWASP Top 10 Web (inyección, control de acceso roto, diseño inseguro, entre otras).
* **OWASP crAPI ("Completely Ridiculous API")**: una aplicación basada en microservicios con un uso extensivo de APIs REST, pensada específicamente para ilustrar las debilidades recogidas en el OWASP API Security Top 10.

De acuerdo con la consigna, se seleccionaron cinco desafíos de cada aplicación. En Juice Shop se trabajó sobre:

* **J2 – Login Admin**
* **J4 – Admin Registration**
* **J5 – User Credentials**
* **J6 – View Basket**
* **J7 – Christmas Special**

En crAPI se trabajó sobre:

* **C1 – Find the location of another user's vehicle**
* **C2 – Reset the password of a different user**
* **C3 – Get an item for free**
* **C6 – NoSQL Injection**
* **C7 – DoS**

Para cada uno de estos desafíos se describe en qué consiste el defecto que lo hace posible, se lo clasifica dentro de la categoría del OWASP Top 10 (Web o API, según corresponda) que se considera más apropiada, y se detalla el enfoque general para su explotación.

> **Aclaración:** Para la realización de este práctico se recomienda haber completado previamente la instalación del entorno de trabajo detallada en el [Práctico 1](https://github.com/belenkanas/dsws-practico1.git). Puntualmente, se trabaja desde una máquina virtual Kali Linux (desplegada en Oracle VirtualBox), utilizando BurpSuite como proxy de intercepción del tráfico del navegador, y contenedores Docker (orquestados con Docker Compose) para levantar las aplicaciones OWASP Juice Shop y OWASP crAPI.

---
</div>

<!-- ============================================================ -->
<!-- Desafíos Juice Shop -->
<!-- ============================================================ -->


<div style="page-break-after: always;"></div>

<a name="desafíos-juice-shop"></a>

## Desafíos Juice Shop

Para esta parte, se explotarán y documentarán cinco debilidades representativas del OWASP Top 10 Web. 

Especialemente, se trabajará con las siguientes debilidades:

* [J2 - Login Admin](#j2---login-admin)
* [J4 - Admin Registration](#j4---admin-registration)
* [J5 - User Credentials](#j5---user-credentials)
* [J6 - View Basket](#j6---view-basket)
* [J7 - Christmas Special](#j7---christmas-special)

<div style="page-break-after: always;"></div>

<a name="j2---login-admin"></a>

## J2 - Login Admin

### Descripción

El desafío consiste en iniciar sesión como el usuario administrador de la tienda sin conocer su contraseña, explotando una falla de inyección SQL presente en el formulario de login.

### Clasificación OWASP Top 10 Web

**A03:2021 – Injection.** El backend concatena directamente el valor ingresado por el usuario en la consulta SQL utilizada para validar las credenciales, en lugar de emplear consultas parametrizadas, lo que permite alterar la lógica de la cláusula `WHERE`.

### Enfoque de explotación

Una vez levantado el contenedor correspondiente a OWASP Juice Shop (con el comando `docker run --detach -p 3000:3000 bkimminich/juice-shop`) y con el proxy de intercepción en ejecución, se realizaron los siguientes pasos:

1. Interceptar con BurpSuite la petición `POST /rest/user/login` que dispara el formulario de inicio de sesión.

Una vez ingresado en JuiceShop, intentar hacer login con una cuenta arbiraria, esto dispará un HTTP Request al proxy de intercepción.

![Login de JuiceShop](images/image1.png)

Desde Burpsuite, se selecciona dicho request para modificarlo (Click Derecho --> Send to Repeater):
![HTTPRequest en Burpsuite](images/image2.png)

2. En Repeater, en el campo `email` del Request, insertar un payload de inyección SQL que fuerce la condición a verdadera y comente el resto de la sentencia, utilizando la condición solicitada en la consigna (`' or 22=22--`).
3. Dejar el campo `password` con un valor arbitrario, dado que la parte de la consulta que lo evalúa quedará neutralizada por el comentario SQL.
4. Reenviar la petición y verificar que la respuesta contiene un token de sesión válido asociado a la cuenta de administrador (`admin@juice-sh.op`), dado que suele ser el primer registro de la tabla de usuarios.

![Vulnerabilidad explotada](images/image3.png)

### Recomendaciones

Algunas recomendaciones para la corrección de dicha vulnerabilidad podría incluir...

---
</div>

<div style="page-break-after: always;"></div>

<a name="j4---admin-registration"></a>

## J4 - Admin Registration

### Descripción

El desafío consiste en registrar un nuevo usuario, llamado "ernesto" según la consigna, con privilegios de administrador, algo que no está previsto desde la interfaz pública de registro.

### Clasificación OWASP Top 10 Web

**A01:2021 – Broken Access Control**, en su variante de *mass assignment*: el servidor no restringe qué campos del modelo de usuario puede fijar un cliente no autenticado al momento del registro.

### Enfoque de explotación

1. Interceptar con Burp la petición `POST /api/Users` que dispara el formulario público de registro.
2. Completar los campos obligatorios del registro, utilizando `ernesto` como nombre de usuario, tal como exige la consigna.
3. Agregar manualmente al cuerpo JSON de la petición un campo adicional `"role": "admin"`, que no está expuesto en el formulario visible.
4. Reenviar la petición y confirmar el privilegio obtenido iniciando sesión con las credenciales recién creadas y verificando el acceso a funcionalidades exclusivas de administrador.

### Recomendaciones

Algunas recomendaciones para la corrección de dicha vulnerabilidad podría incluir...

---
</div>

<div style="page-break-after: always;"></div>

<a name="j5---user-credentials"></a>

## J5 - User Credentials

### Descripción

El desafío consiste en recuperar el listado completo de credenciales (usuarios y hashes de contraseña) almacenadas en la base de datos, explotando una inyección SQL en la barra de búsqueda de productos.

### Clasificación OWASP Top 10 Web

**A03:2021 – Injection**, específicamente una inyección SQL de tipo *UNION-based*.

### Enfoque de explotación

1. Determinar, mediante prueba y error con cláusulas `ORDER BY`, la cantidad de columnas que devuelve la consulta original de búsqueda de productos.
2. Construir un payload que agregue una cláusula `UNION SELECT` apuntando a la tabla de usuarios en lugar de a la de productos, alineando el tipo y la cantidad de columnas.
3. Enviar dicho payload desde el campo de búsqueda de productos, o directamente interceptando y modificando la petición `GET /rest/products/search`.
4. Inspeccionar la respuesta JSON devuelta para extraer los pares de correo electrónico y hash de contraseña de los usuarios registrados.

### Recomendaciones

Algunas recomendaciones para la corrección de dicha vulnerabilidad podría incluir...

---
</div>

<div style="page-break-after: always;"></div>

<a name="j6---view-basket"></a>

## J6 - View Basket

### Descripción

El desafío consiste en visualizar el contenido de la cesta de compras de un usuario distinto al que se encuentra autenticado.

### Clasificación OWASP Top 10 Web

**A01:2021 – Broken Access Control**, en su variante de IDOR (*Insecure Direct Object Reference*).

### Enfoque de explotación

1. Iniciar sesión con una cuenta propia, agregar al menos un producto a la cesta e identificar el patrón del endpoint correspondiente (`GET /rest/basket/{id}`).
2. Interceptar dicha petición con Burp.
3. Modificar manualmente el valor `{id}` por otro identificador numérico, correspondiente a la cesta de otro usuario.
4. Reenviar la petición modificada y verificar que la respuesta contiene los productos de una cesta que no pertenece al usuario autenticado.

### Recomendaciones

Algunas recomendaciones para la corrección de dicha vulnerabilidad podría incluir...

---
</div>

<div style="page-break-after: always;"></div>

<a name="j7---christmas-special"></a>

## J7 - Christmas Special

### Descripción

El desafío consiste en comprar la oferta especial de Navidad de la edición 2014, un producto descontinuado que ya no figura en el catálogo visible de la tienda.

### Clasificación OWASP Top 10 Web

**A01:2021 – Broken Access Control**, con un componente de **A04:2021 – Insecure Design**: ocultar un producto del catálogo visual no equivale a revocar su identificador a nivel de API, que sigue siendo válido para el backend.

### Enfoque de explotación

1. Identificar el identificador (`ProductId`) del producto oculto "Christmas Special", por ejemplo revisando respuestas anteriores del endpoint `/rest/products`, el código fuente de la aplicación o referencias históricas a la promoción de 2014.
2. Construir o interceptar una petición `POST /api/BasketItems` indicando dicho `ProductId`, aunque el producto no aparezca listado en la interfaz.
3. Reenviar la petición y confirmar que el producto fue agregado exitosamente a la cesta pese a no estar disponible en el catálogo visible.
4. Completar el flujo de compra para validar el desafío.

### Recomendaciones

Algunas recomendaciones para la corrección de dicha vulnerabilidad podría incluir...

---
</div>

---
</div>

<!-- ============================================================ -->
<!-- Desafíos crAPI -->
<!-- ============================================================ -->

<div style="page-break-after: always;"></div>

<a name="desafíos-crapi"></a>

## Desafíos crAPI

Para esta parte, se explotarán y documentarán cinco debilidades representativas del OWASP API Security Top 10.

* [C1 - Find the location of another user's vehicle](#c1---find-the-location-of-another-users-vehicle)
* [C2 - Reset the password of a different user](#c2---reset-the-password-of-a-different-user)
* [C3 - Get an item for free](#c3---get-an-item-for-free)
* [C6 - NoSQL Injection](#c6---nosql-injection)
* [C7 - DoS](#c7---dos)

<div style="page-break-after: always;"></div>

<a name="c1---find-the-location-of-another-users-vehicle"></a>

## C1 - Find the location of another user's vehicle

### Descripción

El desafío consiste en obtener información sensible de ubicación (geolocalización) del vehículo de otro usuario de la plataforma.

### Clasificación OWASP Top 10 API

**API1:2023 – Broken Object Level Authorization (BOLA).** El endpoint que devuelve la ubicación de un vehículo confía en un identificador (VIN) provisto por el propio cliente, sin validar del lado servidor que ese vehículo pertenezca al usuario autenticado.

### Enfoque de explotación

1. Iniciar sesión y capturar con Burp la petición legítima que consulta la ubicación del propio vehículo, identificando el microservicio y el formato de la petición.
2. Obtener el VIN de otro usuario, por ejemplo a través de la funcionalidad de foro/comunidad de crAPI, donde suelen compartirse datos de vehículos entre usuarios.
3. Reemplazar el VIN propio por el de la víctima en la petición interceptada.
4. Reenviar la petición modificada y confirmar que la respuesta contiene la ubicación del vehículo ajeno.

### Recomendaciones

Algunas recomendaciones para la corrección de dicha vulnerabilidad podría incluir...

---
</div>

<div style="page-break-after: always;"></div>

<a name="c2---reset-the-password-of-a-different-user"></a>

## C2 - Reset the password of a different user

### Descripción

El desafío consiste en forzar el cambio de contraseña de la cuenta de otro usuario, abusando del flujo de recuperación de contraseña.

### Clasificación OWASP Top 10 API

**API2:2023 – Broken Authentication.** El flujo de recuperación no vincula correctamente el código de verificación (OTP) con el correo electrónico sobre el cual se solicita el cambio, y/o no limita los intentos de verificación de dicho código.

### Enfoque de explotación

1. Disparar el flujo de "Forgot Password" dos veces desde Burp: una con el correo propio y otra con el correo de la víctima, comparando ambas peticiones.
2. Analizar cómo se relacionan, en el cuerpo de la petición de confirmación, el campo del correo electrónico y el del OTP recibido.
3. Ajustar la petición de confirmación de cambio de contraseña para que el campo de correo corresponda al de la víctima, aprovechando la falla identificada en la verificación del OTP.
4. Reenviar la petición y confirmar el cambio iniciando sesión con la nueva contraseña sobre la cuenta de la víctima.

### Recomendaciones

Algunas recomendaciones para la corrección de dicha vulnerabilidad podría incluir...

---
</div>

<div style="page-break-after: always;"></div>

<a name="c3---get-an-item-for-free"></a>

## C3 - Get an item for free

### Descripción

El desafío consiste en obtener un reembolso o crédito superior al monto realmente abonado al procesar la devolución de un producto.

### Clasificación OWASP Top 10 API

**API6:2023 – Unrestricted Access to Sensitive Business Flows.** El flujo de devolución/reembolso no valida límites de cantidad ni impide que una misma operación se repita para la misma orden.

### Enfoque de explotación

1. Realizar una compra y capturar con Burp la petición `POST` correspondiente al flujo de devolución de dicho producto.
2. Analizar los parámetros de cantidad y monto presentes en el cuerpo de la petición.
3. Reenviar la misma petición en repetidas ocasiones (*replay*) y/o alterar el valor de cantidad, observando si el sistema acumula reembolsos sin control.
4. Verificar en el saldo o cupón de la cuenta que el monto obtenido supera al efectivamente pagado.

### Recomendaciones

Algunas recomendaciones para la corrección de dicha vulnerabilidad podría incluir...

---
</div>

<div style="page-break-after: always;"></div>

<a name="c6---nosql-injection"></a>

## C6 - NoSQL Injection

### Descripción

El desafío consiste en obtener cupones de descuento válidos sin conocer un código real, explotando una inyección en la consulta que valida el cupón contra la base de datos NoSQL (MongoDB).

### Clasificación OWASP Top 10 API

**API8:2019 – Injection**, categoría explícita en la edición 2019 del OWASP API Security Top 10 (en la edición 2023 el aspecto de validación insegura de la entrada se asocia principalmente a **API8:2023 – Security Misconfiguration**). El parámetro que recibe el código de cupón se pasa directamente a una consulta MongoDB sin sanitizar, permitiendo inyectar operadores propios de NoSQL en lugar de una cadena literal.

### Enfoque de explotación

1. Interceptar con Burp la petición que aplica un cupón durante el checkout de crAPI.
2. Probar primero con el código indicado por la consigna, `1893Carbonero First`, para confirmar el comportamiento normal ante un cupón inexistente.
3. Reemplazar el valor del campo del código de cupón por un objeto u operador de inyección NoSQL (por ejemplo, forzando una condición que la base de datos evalúe siempre como verdadera) en lugar de una cadena literal.
4. Reenviar la petición y verificar que el cupón es aceptado como válido sin necesidad de conocer un código real.

### Recomendaciones

Algunas recomendaciones para la corrección de dicha vulnerabilidad podría incluir...

---
</div>

<div style="page-break-after: always;"></div>

<a name="c7---dos"></a>

## C7 - DoS

### Descripción

El desafío consiste en provocar una denegación de servicio de capa 7 (aplicación) utilizando la funcionalidad de "contact mechanic".

### Clasificación OWASP Top 10 API

**API4:2023 – Unrestricted Resource Consumption.** El endpoint que procesa el mensaje al mecánico no implementa límites de frecuencia (*rate limiting*) ni de tamaño de payload, y dispara una operación costosa en el backend en cada invocación.

### Enfoque de explotación

1. Capturar con Burp la petición `POST` correspondiente al envío de un mensaje de contacto al mecánico.
2. Identificar si el cuerpo de la petición admite payloads de gran tamaño (por ejemplo, un campo de texto sin límite definido) y si existe algún control de frecuencia de envíos.
3. Automatizar el reenvío masivo de dicha petición, eventualmente con payloads de gran tamaño, utilizando herramientas como Burp Intruder o un script propio.
4. Monitorear el estado del servicio (tiempos de respuesta, disponibilidad, uso de recursos) para verificar la degradación o caída provocada.

### Recomendaciones

Algunas recomendaciones para la corrección de dicha vulnerabilidad podría incluir...

---
</div>

---
</div>

</div>

<div style="page-break-after: always;"></div>

<a name="conclusiones"></a>

## Conclusiones

---
</div>

> Belén Kanas | Desarrollo de Software Seguro 2026
