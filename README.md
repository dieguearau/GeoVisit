# GeoVisit

**Agenda inteligente de visitas en campo mediante geolocalización.**

GeoVisit es una aplicación web liviana desarrollada en **HTML, CSS y
JavaScript** que permite planificar visitas desde Excel, registrar la
ubicación real desde el dispositivo y generar una base estructurada para
su posterior análisis.

El proyecto nace como una prueba de concepto orientada a transformar una
actividad de campo en información trazable y explotable, sin necesidad
de desarrollar una aplicación móvil nativa ni implementar un backend
para esta versión.

------------------------------------------------------------------------

## ¿Qué problema busca resolver?

En procesos comerciales, técnicos u operativos es habitual trabajar con
agendas de visitas y posteriormente consolidar los resultados en
planillas.

GeoVisit propone un flujo simple:

**Planificación → Ejecución → Geolocalización → Validación →
Exportación**

La aplicación permite comparar el punto que debía visitarse con la
ubicación desde la cual se registró efectivamente la visita.

------------------------------------------------------------------------

## Funcionalidades principales

### Agenda de visitas

Se puede importar desde Excel una agenda con los puntos planificados.

Para cada visita se pueden incluir:

-   ID de contrato.
-   Cliente.
-   Dirección planificada.
-   Latitud planificada.
-   Longitud planificada.

La aplicación clasifica automáticamente la agenda entre **Pendientes** y
**Realizadas**.

### Registro georreferenciado

Al registrar una visita, GeoVisit solicita la ubicación del dispositivo
y almacena:

-   Latitud real.
-   Longitud real.
-   Precisión informada por el dispositivo.
-   Fecha.
-   Hora.
-   Dirección aproximada asociada a las coordenadas.
-   Resultado de la visita.

Los resultados disponibles están estandarizados:

-   **Visitado**
-   **Requiere seguimiento**
-   **No localizado**

Esto evita valores libres y facilita el posterior análisis de los datos.

### Validación contra el punto planificado

Para las visitas provenientes de la agenda, la aplicación calcula la
distancia entre:

**Georreferencia planificada ↔ Georreferencia registrada**

El cálculo se realiza en el navegador mediante distancia Haversine y se
expresa en metros.

### Visitas no planificadas

También se puede registrar una visita que no forme parte de la agenda.

En este caso se solicita el ID del contrato y el resultado de la visita,
mientras que la ubicación, fecha y hora se capturan desde el
dispositivo.

### Historial

Todas las visitas efectivamente registradas se muestran en un historial.

La agenda pendiente no forma parte del historial hasta que la visita se
registra.

### Exportación a Excel

GeoVisit exporta únicamente las visitas efectivamente realizadas.

El archivo contiene:

-   ID de contrato.
-   Cliente.
-   Estado de la visita.
-   Resultado.
-   Dirección planificada.
-   Latitud y longitud planificadas.
-   Latitud y longitud reales.
-   Distancia al punto planificado.
-   Precisión.
-   Fecha.
-   Hora.
-   Dirección real.
-   Enlace al mapa.

Las visitas pendientes de la agenda no se incluyen en la exportación.

------------------------------------------------------------------------

## Arquitectura

GeoVisit funciona principalmente del lado del cliente:

``` text
                    GeoVisit
                       │
          ┌────────────┼────────────┐
          │            │            │
       Excel       Navegador      GPS
          │            │            │
          ▼            ▼            ▼
       Agenda     Persistencia   Coordenadas
                       │
                       ▼
               Registro de visita
                       │
            ┌──────────┴──────────┐
            ▼                     ▼
      Historial local      Exportación Excel
```

No se utiliza una base de datos central ni un backend para almacenar las
visitas en esta versión.

------------------------------------------------------------------------

## Tecnologías utilizadas

  Tecnología                  Uso
  --------------------------- ----------------------------------------------
  HTML5                       Estructura de la aplicación
  CSS3                        Diseño responsive e interfaz
  JavaScript                  Lógica de negocio y funcionamiento
  Geolocation API             Obtención de la ubicación del dispositivo
  SheetJS / XLSX              Importación y exportación de archivos Excel
  localStorage                Persistencia local de la información
  OpenStreetMap / Nominatim   Geocodificación inversa
  Google Maps                 Visualización de las coordenadas registradas

------------------------------------------------------------------------

# Manual de uso

## 1. Importar una agenda

Presionar **Importar agenda** y seleccionar un archivo Excel.

El archivo debe contener como mínimo las siguientes columnas:

  ID Contrato     Latitud planificada   Longitud planificada
  ------------- --------------------- ----------------------
  10001                    -34.901200             -56.164500
  10002                    -34.895400             -56.151200

Opcionalmente puede incluir:

-   `Cliente`
-   `Dirección planificada`

Una vez cargado el archivo, GeoVisit mostrará las visitas dentro de la
agenda.

------------------------------------------------------------------------

## 2. Consultar el estado de la agenda

En la parte superior se muestran tres indicadores:

-   **Planificadas**
-   **Pendientes**
-   **Realizadas**

La agenda también puede filtrarse mediante:

**Todas / Pendientes / Realizadas**

------------------------------------------------------------------------

## 3. Registrar una visita planificada

Localizar el contrato dentro de la agenda y presionar:

**Registrar visita**

La aplicación solicitará seleccionar uno de los siguientes resultados:

-   Visitado
-   Requiere seguimiento
-   No localizado

Después se solicitará permiso para acceder a la ubicación del
dispositivo.

Una vez obtenida la posición, GeoVisit registra la visita y calcula
automáticamente la distancia respecto al punto planificado.

La visita pasa de **Pendiente** a **Realizada**.

------------------------------------------------------------------------

## 4. Registrar una visita no planificada

En la sección **Visita no planificada**:

1.  Ingresar el ID del contrato.
2.  Seleccionar el resultado.
3.  Presionar **Registrar ubicación**.
4.  Autorizar el acceso a la ubicación cuando el navegador lo solicite.

La visita se incorpora directamente al historial.

------------------------------------------------------------------------

## 5. Consultar el historial

La sección **Historial de visitas** contiene todas las visitas
efectivamente registradas.

Los resultados se identifican visualmente:

-   🟢 **Visitado**
-   🟡 **Requiere seguimiento**
-   🔴 **No localizado**

También se muestra la precisión de la geolocalización y, cuando
corresponde, la distancia respecto a la ubicación planificada.

El enlace **Abrir mapa** permite visualizar la coordenada registrada.

------------------------------------------------------------------------

## 6. Exportar los resultados

Presionar **Exportar Excel**.

La exportación contiene exclusivamente las visitas que aparecen en el
historial.

Por lo tanto:

``` text
Visita realizada       → se exporta
Visita no planificada  → se exporta
Visita pendiente       → no se exporta
```

Esto permite utilizar el archivo resultante directamente como una base
de visitas realizadas.

------------------------------------------------------------------------

## Privacidad y almacenamiento

GeoVisit no utiliza un backend propio para almacenar las visitas.

La información de trabajo se conserva localmente en el navegador
mediante `localStorage`.

La publicación de la aplicación en un servicio estático como GitHub
Pages distribuye el código de la herramienta, pero los registros
almacenados localmente no se incorporan automáticamente al repositorio.

### Consideración sobre la dirección

Las coordenadas se obtienen mediante la API de geolocalización del
navegador.

Para transformar esas coordenadas en una dirección legible, esta versión
consulta el servicio de geocodificación inversa **Nominatim /
OpenStreetMap**. Por este motivo, esa operación requiere conexión a
Internet y las coordenadas consultadas son enviadas a dicho servicio.

------------------------------------------------------------------------

## Precisión de la ubicación

GeoVisit conserva la precisión reportada por el dispositivo.

La calidad de la ubicación puede variar según:

-   dispositivo utilizado;
-   disponibilidad de GPS;
-   señal disponible;
-   permisos del navegador;
-   entorno físico;
-   método utilizado por el sistema operativo para determinar la
    posición.

La precisión mostrada debe interpretarse como una estimación
proporcionada por el dispositivo y no como una certificación de
presencia física.

------------------------------------------------------------------------

## Ejecución mediante GitHub Pages

GeoVisit puede publicarse como un sitio web estático mediante GitHub
Pages.

Para ello:

1.  Renombrar el archivo principal como `index.html`.
2.  Subirlo al repositorio.
3.  Abrir **Settings → Pages**.
4.  Seleccionar **Deploy from a branch**.
5.  Seleccionar la rama `main` y la carpeta `/ (root)`.
6.  Guardar la configuración.

El uso mediante HTTPS permite que los navegadores compatibles soliciten
acceso a la geolocalización.

------------------------------------------------------------------------

## Alcance actual

GeoVisit se encuentra planteado como un **MVP / prueba de concepto**.

El objetivo actual no es reemplazar una plataforma corporativa de
gestión de fuerza de campo, sino demostrar un flujo completo y liviano:

``` text
Importar planificación
        ↓
Ejecutar visita
        ↓
Capturar geolocalización
        ↓
Comparar ubicación
        ↓
Estructurar resultados
        ↓
Exportar información
```

------------------------------------------------------------------------

## Posibles evoluciones

Algunas extensiones posibles del proyecto:

-   Persistencia mediante IndexedDB.
-   Funcionamiento como PWA.
-   Operación offline.
-   Autenticación de usuarios.
-   Sincronización con un backend.
-   Dashboard de visitas y cumplimiento.
-   Visualización de agenda sobre mapa.
-   Optimización de rutas.
-   Reglas configurables de validación por distancia.
-   Integración con CRM o sistemas de gestión.

------------------------------------------------------------------------

## Casos de uso potenciales

El concepto puede adaptarse a diferentes procesos de trabajo en campo,
por ejemplo:

-   visitas comerciales;
-   instalaciones;
-   mantenimientos;
-   inspecciones;
-   auditorías;
-   relevamientos;
-   verificaciones de puntos;
-   servicios técnicos.

------------------------------------------------------------------------

## Nota

Los datos utilizados en demostraciones, capturas o ejemplos del
repositorio deben ser ficticios.

Antes de utilizar una solución de este tipo con información real en un
entorno corporativo se deben evaluar las políticas de privacidad,
seguridad, tratamiento de geolocalización, servicios externos utilizados
y requisitos internos de la organización.

------------------------------------------------------------------------
