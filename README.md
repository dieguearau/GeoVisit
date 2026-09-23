# 📍 GeoVisit

### De una agenda de visitas a datos georreferenciados listos para analizar

**GeoVisit** es una aplicación web liviana para **planificar, registrar
y validar visitas en campo mediante geolocalización**, con importación y
exportación a Excel.

<p><img src="https://img.shields.io/badge/HTML5-E34F26?style=flat&logo=html5&logoColor=white" alt="HTML5"> <img src="https://img.shields.io/badge/CSS3-1572B6?style=flat&logo=css3&logoColor=white" alt="CSS3"> <img src="https://img.shields.io/badge/JavaScript-F7DF1E?style=flat&logo=javascript&logoColor=black" alt="JavaScript"> <img src="https://img.shields.io/badge/Geolocation_API-4285F4?style=flat&logo=googlemaps&logoColor=white" alt="Geolocation API"> <img src="https://img.shields.io/badge/SheetJS-217346?style=flat&logo=microsoftexcel&logoColor=white" alt="SheetJS"> <img src="https://img.shields.io/badge/OpenStreetMap-7EBC6F?style=flat&logo=openstreetmap&logoColor=white" alt="OpenStreetMap"></p>

------------------------------------------------------------------------

## 💡 ¿Qué problema busca resolver?

Una visita en campo genera información que después puede ser difícil de
estructurar:

- 📍 ¿Dónde se realizó?
- 🕐 ¿Cuándo se registró?
- 🎯 ¿Coincide con el punto que debía visitarse?
- 📋 ¿Cuál fue el resultado?
- 📊 ¿Cómo se transforma todo esto en una base lista para analizar?

GeoVisit propone un flujo simple:

``` text
Excel
  ↓
Agenda de visitas
  ↓
Ejecución en campo
  ↓
Geolocalización
  ↓
Validación contra el punto planificado
  ↓
Historial estructurado
  ↓
Excel para análisis
```

La aplicación busca conectar una **operación de campo** con un **proceso
de datos**, sin necesidad de una aplicación móvil nativa ni de un
backend para esta versión.

------------------------------------------------------------------------

## ✨ ¿Qué permite hacer?

### 📅 Planificar visitas desde base importada

GeoVisit permite importar una agenda con puntos previamente definidos.

Cada visita puede contener:

- ID de contrato.
- Cliente.
- Dirección planificada.
- Latitud planificada.
- Longitud planificada.

La aplicación clasifica automáticamente la agenda entre:

**Planificadas · Pendientes · Realizadas**

También permite filtrar rápidamente las visitas según su estado.

------------------------------------------------------------------------

### 📍 Registrar una visita desde el dispositivo

Al presionar **Registrar visita**, el usuario selecciona un resultado
estandarizado:

- 🟢 **Visitado**
- 🟡 **Requiere seguimiento**
- 🔴 **No localizado**

Luego GeoVisit solicita acceso a la ubicación y registra:

- Latitud real.
- Longitud real.
- Precisión informada por el dispositivo.
- Fecha.
- Hora.
- Dirección aproximada.
- Resultado de la visita.

El uso de opciones predefinidas evita resultados escritos de distintas
formas y mejora la calidad de los datos generados.

------------------------------------------------------------------------

### 🎯 Comparar lo planificado con lo realizado

Cuando la visita proviene de una agenda, GeoVisit compara:

``` text
Ubicación planificada  ↔  Ubicación registrada
```

y calcula automáticamente la **distancia en metros** entre ambos puntos
mediante la fórmula de Haversine.

Esto permite transformar la geolocalización en una variable cuantitativa
que luego puede analizarse.

------------------------------------------------------------------------

### ➕ Registrar visitas no planificadas

No todas las visitas tienen que formar parte de una agenda.

GeoVisit permite registrar una visita adicional mediante:

1.  ID del contrato.
2.  Resultado de la visita.
3.  Ubicación obtenida desde el dispositivo.

La visita se incorpora directamente al historial junto con las visitas
planificadas realizadas.

------------------------------------------------------------------------

### 📊 Generar un historial estructurado

El historial representa únicamente las visitas que efectivamente fueron
registradas.

Una visita pendiente permanece en la agenda, pero **no se considera una
visita realizada**.

Esto mantiene separados dos conceptos:

``` text
AGENDA      = trabajo previsto
HISTORIAL   = trabajo registrado
```

------------------------------------------------------------------------

### 📥 Exportar los resultados a Excel

La exportación contiene exclusivamente el historial de visitas.

| Campo                 | Descripción                                    |
|:----------------------|:-----------------------------------------------|
| ID Contrato           | Identificador del punto visitado               |
| Cliente               | Cliente asociado, si corresponde               |
| Estado                | Planificada / No planificada                   |
| Resultado             | Resultado seleccionado                         |
| Dirección planificada | Dirección proveniente de la agenda             |
| Latitud planificada   | Coordenada esperada                            |
| Longitud planificada  | Coordenada esperada                            |
| Latitud real          | Coordenada registrada                          |
| Longitud real         | Coordenada registrada                          |
| Distancia (m)         | Diferencia entre ambos puntos                  |
| Precisión (m)         | Precisión informada por el dispositivo         |
| Fecha                 | Fecha del registro                             |
| Hora                  | Hora del registro                              |
| Dirección real        | Dirección obtenida por geocodificación inversa |
| Mapa                  | Enlace para visualizar la ubicación            |

Por diseño:

``` text
Visita planificada + realizada   → se exporta
Visita no planificada realizada  → se exporta
Visita pendiente                 → no se exporta
```

------------------------------------------------------------------------

# 🧩 ¿Qué demuestra este proyecto?

El proyecto integra en una sola herramienta:

**Data Wrangling**  
Importación, validación, transformación y estructuración de información
proveniente de Excel.

**Automatización**  
Reducción de registros manuales y generación automática de variables
como fecha, hora, ubicación, precisión y distancia.

**Geolocalización**  
Uso de las capacidades del dispositivo para transformar una actividad
física en información digital.

**Diseño de procesos**  
Separación entre planificación, ejecución e historial.

**Calidad de datos**  
Resultados estandarizados y validaciones para reducir inconsistencias.

**Analítica**  
Generación de una base estructurada que puede utilizarse posteriormente
en Excel, Power BI, R, Python u otras herramientas.

El objetivo del proyecto es demostrar cómo una solución web
relativamente simple puede **digitalizar un proceso operativo y
convertirlo en datos analizables**.

------------------------------------------------------------------------

# 🏗️ Arquitectura

GeoVisit funciona principalmente del lado del cliente:

``` text
                         GeoVisit
                            │
             ┌──────────────┼──────────────┐
             │              │              │
           Excel        Navegador         GPS
             │              │              │
             ▼              ▼              ▼
           Agenda      Persistencia    Coordenadas
                            │
                            ▼
                    Registro de visita
                            │
                  ┌─────────┴─────────┐
                  ▼                   ▼
          Validación geográfica   Dirección
                  │                   │
                  └─────────┬─────────┘
                            ▼
                         Historial
                            │
                            ▼
                      Exportación Excel
```

Para esta versión:

- No existe una base de datos central.
- No existe un backend propio.
- Los registros de trabajo se almacenan localmente en el navegador.
- GitHub Pages se utiliza para distribuir la aplicación mediante
  HTTPS.

------------------------------------------------------------------------

# 🛠️ Stack tecnológico

| Tecnología                    | Función                            |
|:------------------------------|:-----------------------------------|
| **HTML5**                     | Estructura de la aplicación        |
| **CSS3**                      | Diseño responsive e interfaz       |
| **JavaScript Vanilla**        | Lógica de negocio                  |
| **Geolocation API**           | Captura de ubicación               |
| **SheetJS / XLSX**            | Importación y exportación de Excel |
| **localStorage**              | Persistencia local                 |
| **Haversine**                 | Cálculo de distancia geográfica    |
| **OpenStreetMap / Nominatim** | Geocodificación inversa            |
| **Google Maps**               | Visualización de coordenadas       |

------------------------------------------------------------------------

# 📖 Manual rápido

## 1. Importar una agenda

Presionar **Importar agenda** y seleccionar un archivo Excel.

El archivo debe contener como mínimo:

| ID Contrato | Latitud planificada | Longitud planificada |
|:------------|--------------------:|---------------------:|
| 10001       |          -34.901200 |           -56.164500 |
| 10002       |          -34.895400 |           -56.151200 |

También puede contener:

- `Cliente`
- `Dirección planificada`

Una vez importado, los registros aparecerán dentro de **Agenda de
visitas**.

------------------------------------------------------------------------

## 2. Consultar la agenda

Los indicadores superiores resumen:

**Planificadas · Pendientes · Realizadas**

Los botones:

**Todas · Pendientes · Realizadas**

permiten filtrar rápidamente el trabajo.

------------------------------------------------------------------------

## 3. Registrar una visita planificada

Desde la agenda:

1.  Localizar el contrato.
2.  Presionar **Registrar visita**.
3.  Seleccionar el resultado.
4.  Autorizar el acceso a la ubicación.
5.  Esperar la confirmación del registro.

GeoVisit obtiene la ubicación real y, si existe una georreferencia
planificada, calcula la distancia entre ambos puntos.

La visita pasa automáticamente de **Pendiente** a **Realizada**.

------------------------------------------------------------------------

## 4. Registrar una visita no planificada

En **Visita no planificada**:

1.  Ingresar el ID del contrato.
2.  Seleccionar el resultado.
3.  Presionar **Registrar ubicación**.
4.  Autorizar el acceso a la ubicación.

El registro aparecerá directamente en el historial.

------------------------------------------------------------------------

## 5. Revisar el historial

**Historial de visitas** concentra el trabajo efectivamente registrado.

Los resultados se identifican visualmente:

🟢 **Visitado**  
🟡 **Requiere seguimiento**  
🔴 **No localizado**

También se muestran las coordenadas, precisión, distancia, fecha, hora y
dirección.

El enlace **Abrir mapa** permite visualizar el punto registrado.

------------------------------------------------------------------------

## 6. Exportar

Presionar **Exportar Excel**.

GeoVisit genera una base con todas las visitas registradas y excluye las
visitas pendientes.

El archivo puede utilizarse directamente para análisis posteriores.

------------------------------------------------------------------------

# 🔐 Privacidad y almacenamiento

La arquitectura actual prioriza la simplicidad.

Los datos de trabajo se almacenan localmente mediante `localStorage`.
GitHub Pages distribuye el código de la aplicación, pero **los registros
almacenados en el navegador no se incorporan automáticamente al
repositorio**.

### Geocodificación inversa

El GPS proporciona coordenadas, no una dirección postal.

Para transformar:

``` text
Latitud + Longitud → Dirección
```

GeoVisit utiliza el servicio de geocodificación inversa de
**OpenStreetMap / Nominatim**.

Por este motivo, esa funcionalidad requiere conexión a Internet y las
coordenadas utilizadas para la consulta son enviadas al servicio
externo.

------------------------------------------------------------------------

# 📐 Sobre la precisión

La precisión mostrada por GeoVisit corresponde al valor informado por el
dispositivo mediante la API de geolocalización.

Puede variar según:

- dispositivo;
- disponibilidad de GPS;
- señal;
- permisos;
- entorno físico;
- sistema operativo;
- método utilizado para determinar la ubicación.

Por este motivo, GeoVisit debe interpretarse como una herramienta de
registro y apoyo operativo, no como un mecanismo de certificación de
presencia física.

------------------------------------------------------------------------

# 💼 Casos de uso

Aunque el MVP utiliza contratos como identificador, el concepto puede
adaptarse a distintos procesos de trabajo en campo:

- visitas comerciales;
- instalaciones;
- mantenimientos;
- inspecciones;
- auditorías;
- relevamientos;
- verificaciones;
- servicios técnicos.

El identificador puede representar un contrato, cliente, orden de
trabajo, establecimiento, activo o cualquier otro punto que deba
visitarse.

------------------------------------------------------------------------

# ⚠️ Alcance del proyecto

Antes de utilizar una solución de este tipo con información real en un
entorno corporativo deben evaluarse las políticas aplicables de:

- privacidad;
- tratamiento de geolocalización;
- seguridad;
- retención de información;
- servicios externos;
- acceso de usuarios.

------------------------------------------------------------------------

# 👤 Autor

**Diego Araujo**

`Data Analytics` · `Automatización` · `Optimización de procesos` ·
`Geolocalización` · `Herramientas de negocio`

------------------------------------------------------------------------

### GeoVisit

**Planificar. Registrar. Validar. Analizar.**
