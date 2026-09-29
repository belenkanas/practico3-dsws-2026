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

El objetivo de este práctico es identificar, explotar y documentar vulnerabilidades representativas del OWASP Top 10 Web y del OWASP Top 10 API que se encuentran en las aplicaciones web de OWASP Juice Shop y OWASP crAPI, respectivamente.

Para realizarlo, se eligen cinco desafíos para explorar en cada aplicación, explicando en cada uno el defecto que lo ocasiona de forma general, su clasificación en la categoría del ranking OWASP, los pasos para su explotación y una recomendación de corrección para dicha vulnerabilidad.

> **Aclaración:** Para la realización de este práctico, es sumamente recomendado seguir los pasos de la instalación del entorno especificado en el [práctico 1](https://github.com/belenkanas/dsws-practico1.git). Específicamente se trabaja desde la máquina virtual Kali Linux (instalada desde Oracle VirtualBox), proxy de navegación BurpSuite...


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

El siguiente desafío consiste en...

Log in with the administrator’s user account. Usar or 22=22 en la respuesta

### Clasificación OWASP Top 10 Web

### Explotación - PoC

### Recomendaciones

Algunas recomendaciones para la corrección de dicha vulnerabilidad podría incluir...

---
</div>

<div style="page-break-after: always;"></div>


<a name="j4---admin-registration"></a>

## J4 - Admin Registration

### Descripción

El siguiente desafío consiste en...

Register as a user with administrator privileges. El usuario se debe llamar ernesto

### Clasificación OWASP Top 10 Web

### Explotación - PoC

### Recomendaciones

Algunas recomendaciones para la corrección de dicha vulnerabilidad podría incluir...

---
</div>

<div style="page-break-after: always;"></div>


<a name="j5---user-credentials"></a>

## J5 - User Credentials

### Descripción

El siguiente desafío consiste en...

Retrieve a list of all user credentials via SQL Injection.

### Clasificación OWASP Top 10 Web

### Explotación - PoC

### Recomendaciones

Algunas recomendaciones para la corrección de dicha vulnerabilidad podría incluir...

---
</div>

<div style="page-break-after: always;"></div>

<a name="j6---view-basket"></a>

## J6 - View Basket

### Descripción

El siguiente desafío consiste en...

View another user's shopping basket.

### Clasificación OWASP Top 10 Web

### Explotación - PoC

### Recomendaciones

Algunas recomendaciones para la corrección de dicha vulnerabilidad podría incluir...

---
</div>

<div style="page-break-after: always;"></div>

<a name="j7---christmas-special"></a>

## J7 - Christmas Special

### Descripción

El siguiente desafío consiste en...

Order the Christmas special offer of 2014

### Clasificación OWASP Top 10 Web

### Explotación - PoC

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

Para esta parte, se explotarán y documentarán cinco debilidades representativas del OWASP Top 10 API. Especialemente, se trabajará con las siguientes debilidades:

* [C1 - Find the location of another user's vehicle](#c1---find-the-location-of-another-users-vehicle)
* [C2 - Reset the password of a different user](#c2---reset-the-password-of-a-different-user)
* [C3 - Get an item for free](#c3---get-an-item-for-free)
* [C6 - NoSQL Injection](#c6---nosql-injection)
* [C7 - DoS](#c7---dos)


<div style="page-break-after: always;"></div>

<a name="c1---find-the-location-of-another-users-vehicle"></a>

## C1 - Find the location of another user's vehicle

### Descripción

El siguiente desafío consiste en...

Leak sensitive information of another user’s vehicle

### Clasificación OWASP Top 10 API

### Explotación - PoC

### Recomendaciones

Algunas recomendaciones para la corrección de dicha vulnerabilidad podría incluir...

---
</div>

<div style="page-break-after: always;"></div>

<a name="c2---reset-the-password-of-a-different-user"></a>

## C2 - Reset the password of a different user

### Descripción

El siguiente desafío consiste en...

Abuse the password recovery flow process.

### Clasificación OWASP Top 10 API

### Explotación - PoC

### Recomendaciones

Algunas recomendaciones para la corrección de dicha vulnerabilidad podría incluir...

---
</div>

<div style="page-break-after: always;"></div>

<a name="c3---get-an-item-for-free"></a>

## C3 - Get an item for free

### Descripción

El siguiente desafío consiste en...

Return a product.

### Clasificación OWASP Top 10 API

### Explotación - PoC

### Recomendaciones

Algunas recomendaciones para la corrección de dicha vulnerabilidad podría incluir...

---
</div>

<div style="page-break-after: always;"></div>

<a name="c6---nosql-injection"></a>

## C6 - NoSQL Injection

### Descripción

El siguiente desafío consiste en...

Find a way to get free coupons without knowing the coupon code. Try the coupon code.

### Clasificación OWASP Top 10 API

### Explotación - PoC

### Recomendaciones

Algunas recomendaciones para la corrección de dicha vulnerabilidad podría incluir...

---
</div>

<div style="page-break-after: always;"></div>

<a name="c7---dos"></a>

## C7 - DoS

### Descripción

El siguiente desafío consiste en...

Perform a layer 7 DoS using ‘contact mechanic’ feature.

### Clasificación OWASP Top 10 API

### Explotación - PoC

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
