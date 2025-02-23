# PPS-Unidad2Actividad1-SergioMorato

## Índice

- [PPS-Unidad2Actividad1-SergioMorato](#pps-unidad2actividad1-sergiomorato)
  - [Índice](#índice)
  - [Introducción](#introducción)
  - [Objetivos](#objetivos)
  - [Descripción de la Vulnerabilidad](#descripción-de-la-vulnerabilidad)
  - [Impacto](#impacto)
  - [Información sobre patrones de ataque](#información-sobre-patrones-de-ataque)
    - [Prerequisitos para este ataque](#prerequisitos-para-este-ataque)
    - [Habilidades requeridas](#habilidades-requeridas)
    - [Consecuencias](#consecuencias)
    - [Mitigaciones](#mitigaciones)
  - [Registro CVE de la vulnerabilidad](#registro-cve-de-la-vulnerabilidad)
  - [Conclusiones](#conclusiones)

---

## Introducción

Este documento describe la vulnerabilidad **CVE-2024-0204**, una grave omisión de autenticación en **GoAnywhere MFT**. En él, se detallará su impacto, las posibles mitigaciones y las fuentes oficiales donde se puede obtener más información.

Esta vulnerabilidad es particularmente crítica, ya que permite a un atacante sin autenticación **crear un usuario administrador** en el portal de administración, comprometiendo así la seguridad del sistema.

---

## Objetivos

El objetivo de este documento es:

- Proporcionar información clara y estructurada sobre **CVE-2024-0204**.
- Explicar el impacto que puede tener en las organizaciones que utilizan **GoAnywhere MFT**.
- Detallar las medidas de mitigación y actualización recomendadas.
- Ofrecer fuentes confiables para ampliar el conocimiento sobre esta vulnerabilidad.

---

## Descripción de la Vulnerabilidad

**CVE-2024-0204** es una vulnerabilidad crítica de **omisión de autenticación** en **GoAnywhere MFT**, identificada en enero de 2024. Esta vulnerabilidad permite a un atacante no autenticado **crear un usuario administrador** a través del portal de administración, lo que le otorga acceso total al sistema.

- **Producto afectado**: GoAnywhere MFT
- **Versiones vulnerables**: Versiones anteriores a **7.4.1**
- **Tipo de vulnerabilidad**: Omisión de autenticación
- **Fecha de divulgación**: Enero 2024
- **Nivel de severidad**: Crítico (CVSS: 9.8)

[📌 Vulnerabilidad](https://nvd.nist.gov/vuln/detail/CVE-2024-0204)

<p align="center">
    <img src="imagenes\NIST1.png" alt="Config Nano">
    <img src="imagenes\NIST2.png" alt="Config Nano">
</p>

En esta captura podemos ver que el CVSS de 9.8 de criticidad está compuesto por un valor de 3.9 en las métricas de explotabilidad:

- **El vector de ataque se produce desde la red.**  
- **La complejidad de ataque es baja.**  
- **No se necesitan privilegios en el sistema para explotar la vulnerabilidad.**  
- **No se requiere interacción con el usuario, lo que permite la automatización del ataque.**  
- **El alcance no se modifica**, lo que implica que la vulnerabilidad no afecta otros sistemas con controles de seguridad diferentes. Este parámetro reduce ligeramente la criticidad.

Las métricas de impacto suman 5.9 puntos:

- **Impacto en Confidencialidad**: Alto.  
- **Impacto en Integridad**: Alto.  
- **Impacto en Disponibilidad**: Alto.  

Este CVSS indica una vulnerabilidad crítica con un alto impacto en los sistemas afectados y una facilidad significativa de explotación.

---

## Impacto

En la siguiente imagen podemos ver las diferentes formas de explotación de esta vulnerabilidad:

<p align="center">
    <img src="imagenes\NIST3.png" alt="Config Nano">
</p>

Este enlace nos redirige a la página de [MITRE](https://cwe.mitre.org/data/definitions/425.html):

<p align="center">
    <img src="imagenes\MITRE.png" alt="Config Nano">
</p>

---

> **CWE-425: Direct Request ('Forced Browsing')**  
> Esta debilidad ocurre cuando una aplicación web no aplica correctamente los controles de acceso en URLs, scripts o archivos restringidos. Los atacantes pueden acceder directamente a estos recursos manipulando las rutas, eludiendo así la autorización.  
> **Consecuencias**: Acceso no autorizado a datos, modificación de datos, ejecución de código no autorizado.  
> **Mitigación**: Implementar controles de acceso en todas las rutas y usar marcos como Struts para mejorar la seguridad.  
> **Ejemplo**: CVE-2022-29238, donde un sistema de colaboración web permite acceso directo a archivos ocultos.  

---

## Información sobre patrones de ataque

En la siguiente sección, podremos obtener más detalles sobre los patrones de ataque relacionados:

<p align="center">
    <img src="imagenes\MITRE2.png" alt="Config Nano">
</p>

Este enlace nos redirige a una página con más información:

<p align="center">
    <img src="imagenes\CAPEC1.png" alt="Config Nano">
</p>

**CAPEC-127: Directory Indexing** es un patrón de ataque muy conocido. En este caso, un adversario explora el contenido de un directorio sin autorización al solicitar una ruta que termina en un nombre de directorio. Esto puede exponer archivos sensibles como copias de seguridad, archivos temporales y otros que pueden ser explotados para obtener más información y vulnerar aún más la seguridad del sistema.

El flujo de ejecución de este patrón de ataque incluye fases de **exploración**, **experimentación** y **explotación**:

1. **Exploración**: El atacante realiza solicitudes para encontrar directorios en el servidor.
2. **Experimentación**: Intenta acceder al directorio eludiendo las ACLs mediante técnicas como fuzzing o manipulación avanzada de URLs.
3. **Explotación**: Si el atacante tiene éxito en eludir las restricciones de acceso, puede leer archivos no destinados a ser públicos.

### Prerequisitos para este ataque

- La configuración del servidor debe permitir que los directorios sean accesibles mediante una solicitud que termine en un nombre de directorio.
- El atacante debe tener control sobre la ruta solicitada.
- El servidor debe carecer de medidas de protección adecuadas.

### Habilidades requeridas

- **Bajas habilidades**: Realizar solicitudes sin especificar un nombre de archivo.
- **Altas habilidades**: Eludir las protecciones de acceso mediante técnicas como fuzzing o manipulación avanzada de URLs.

### Consecuencias

- **Confidencialidad**: El atacante puede acceder a archivos sensibles, comprometiendo la seguridad y privacidad de los datos.

### Mitigaciones

1. **Uso de un archivo `index.html` vacío**: Para prevenir que los directorios sean listados.
2. **Configuración de `.htaccess` en servidores Apache**: Usar la directiva `Options -Indexes` para evitar la indexación.
3. **Desactivar mensajes de error**: Configurar los mensajes de error 403 para que no revelen información adicional sobre los directorios.

Este tipo de ataque está relacionado con debilidades como la **protección inadecuada de rutas alternas** y la **falta de control adecuado sobre los permisos**.

---

## Registro CVE de la vulnerabilidad

Para finalizar con la trazabilidad de esta vulnerabilidad, descargaremos el archivo JSON con toda la información relevante. Puedes acceder al registro completo en el siguiente enlace:

[📌 CVE.org](https://www.cve.org)

Una vez dentro, podrás ver la información detallada en formato JSON:

<p align="center">
<img src="imagenes\CVE.png" alt="Config Nano">
</p>

Y el archivo JSON:

<p align="center">
    <img src="imagenes\JSON.png" alt="Config Nano">
</p>

---

## Conclusiones

La vulnerabilidad **CVE-2024-0204** representa una grave amenaza para los sistemas que utilizan **GoAnywhere MFT**, ya que permite a un atacante sin autenticación crear un usuario administrador, comprometiendo el control total del sistema. Este tipo de omisión de autenticación subraya la importancia de mantener una configuración rigurosa en los sistemas de autenticación y control de acceso.

La mitigación de esta vulnerabilidad debe incluir la actualización inmediata a las versiones no vulnerables, la implementación de medidas de control de acceso estrictas y el uso de prácticas de seguridad como la configuración adecuada de archivos de directorio y la limitación de los mensajes de error.

Es esencial que las organizaciones adopten un enfoque proactivo en la gestión de vulnerabilidades para minimizar los riesgos de explotación. La información proporcionada sobre esta vulnerabilidad y las medidas de mitigación deben ser consideradas para reforzar la seguridad de los sistemas afectados.

---
**Autor**
Sergio Morato Prieto
