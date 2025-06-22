# FORMATOS DE TRANSFERENCIA DE DATOS EN INFORMÁTICA

En informática, los formatos de transferencia de datos son las estructuras o maneras en que se organizan los datos cuando se envían de un lugar a otro.
Cada vez que un sistema envía, recibe o guarda información (ya sea un texto, imagen, archivo o mensaje), necesita usar un formato estandarizado para que el receptor pueda interpretarlo correctamente.

> Hablar el mismo idioma es esencial para que dos programas puedan entenderse.

Estos formatos permiten que:

- Sistemas distintos puedan intercambiar información (por ejemplo, una app de celular con un servidor en la nube).
- Los datos puedan ser leídos, almacenados o procesados fácilmente.
- Se logre una mayor eficiencia, compatibilidad y seguridad en las transferencias.

## 1. FORMATOS DE INTERCAMBIO DE DATOS

Usados comúnmente en APIs, sistemas distribuidos y almacenamiento estructurado.


### JSON (JavaScript Object Notation)

- Ligero, legible, ampliamente usado en APIs web  
- Ideal para enviar objetos y listas desde servidores a navegadores o apps móviles


```json
{
  "name": "María",
  "age": 28,
  "skills": ["Python", "SQL", "Docker"]
}
```

Características

- Claves y valores separados por `:`
- Estructura basada en objetos (`{}`) y listas (`[]`)
- Solo admite tipos de datos simples: string, número, booleano, array, objeto, null



### XML (eXtensible Markup Language)

- Más verboso que JSON, pero permite validación con esquemas (XSD, DTD)
- Usado en Web Services antiguos (SOAP), industria financiera, configuración


```xml
<user>
  <name>María</name>
  <age>28</age>
  <skills>
    <skill>Python</skill>
    <skill>SQL</skill>
    <skill>Docker</skill>
  </skills>
</user>
```

Características

- Usa etiquetas de apertura/cierre como HTML  
- Permite anidamiento profundo  
- Admite comentarios y metadatos más estructurados  
- Más pesado y menos legible para humanos  


### YAML (YAML Ain’t Markup Language)

- Legible para humanos, muy usado en DevOps (Docker, Ansible, GitHub Actions)  
- Ideal para configuración y plantillas


```yaml
name: María
age: 28
skills:
  - Python
  - SQL
  - Docker
```

Características:

- Muy limpio visualmente (no usa llaves ni comillas obligatorias)
- Usa indentación con espacios para representar jerarquías
- Admite comentarios con `#`
- Sensible a errores de espacio

### CSV (Comma Separated Values)

Usado para datos tabulares, como hojas de cálculo o bases de datos. Ideal para exportar e importar desde Excel, Google Sheets o SQL

**Ejemplo:**

```csv
name,age,skills
María,28,"Python;SQL;Docker"
Juan,32,"Java;C++;HTML"
```

Características:

- Cada fila es un registro
- Las columnas están separadas por comas (,), aunque a veces se usa ; o tabulación
- No tiene jerarquía: todo es plano
- Muy fácil de leer con software de hoja de cálculo

### TOML e INI (archivos de configuración)

Comunes en archivos de configuración (ej: pyproject.toml, settings.ini). Útiles para proyectos pequeños o configuración de herramientas.

**Ejemplo TOML:**

```toml
name = "María"
age = 28
skills = ["Python", "SQL", "Docker"]


[server]
host = "localhost"
port = 8080
````

**Ejemplo INI:**

```ini
[DEFAULT]
name = María
age = 28

[server]
host = localhost
port = 8080
```

Características:

- TOML es más moderno, usado en Python y Rust
- INI es más antiguo, pero todavía útil (por ejemplo, en configuración de Windows)
- Ambos separan secciones por encabezados [sección]
- Muy legibles y fáciles de editar

## 2. FORMATOS BINARIOS PARA TRANSFERENCIA EFICIENTE

A diferencia de los formatos de texto como JSON o XML, que son legibles por humanos, los formatos binarios están optimizados para que las máquinas los lean más rápido y ocupen menos espacio. Piensa en un formato binario como una versión comprimida y cifrada de un paquete: no se entiende a simple vista, pero viaja más rápido y más seguro.

**Son ideales para:**

- Comunicaciones entre microservicios o sistemas distribuidos  
- Dispositivos con recursos limitados (IoT)  
- Streaming de datos en tiempo real  
- Casos donde la legibilidad humana no es necesaria  

### Protocol Buffers (Protobuf)

- Desarrollado por Google. Muy compacto y extremadamente rápido.
- Uso común: servicios backend, microservicios en Kubernetes, apps móviles

**Ventajas:**

- Requiere definir un esquema `.proto` antes de enviar/recibir datos.  
- Los datos se serializan y deserializan con librerías en distintos lenguajes.  
- Compatible con evolución de versiones (se pueden agregar campos sin romper lo anterior).

**Ejemplo de esquema `.proto`:**

```proto
message User {
  string name = 1;
  int32 age = 2;
  repeated string skills = 3;
}
```

Representación binaria resultante (no legible directamente):

```yaml
0a05 4d61 7269 6110 1c1a 0650 7974 686f 6e1a 0353 514c
```

Al ser binario, es más rápido y más pequeño que JSON.

### Apache Avro

Usado en Big Data y sistemas como Apache Kafka. Su uso común es: transmisión de millones de eventos por segundo, análisis en Hadoop

Ventajas:

- Usa un esquema, pero lo incluye dentro del archivo, lo que facilita el auto-descubrimiento.
- Soporta evolución de datos (añadir o eliminar campos sin romper el sistema).
- Perfecto para ambientes donde la compatibilidad entre versiones es crítica.

Ejemplo (esquema + datos):

```json
{
  "type": "record",
  "name": "User",
  "fields": [
    {"name": "name", "type": "string"},
    {"name": "age", "type": "int"}
  ]
}
````

Salida binaria (compacta):

```yaml
06 4d 61 72 69 61 38
```


### MessagePack

Similar a JSON pero en formato binario. Su uso común: APIs móviles o IoT, donde cada byte cuenta

Ventajas:

- Serializa objetos como JSON, pero en menos espacio
- No necesita esquema separado
- Puede ser leído por humanos si se convierte (hay viewers online)

Ejemplo de JSON:

```json
{"name": "María", "age": 28}
````

Codificación MessagePack:

```r
82 a4 6e 61 6d 65 a5 4d 61 72 69 61 a3 61 67 65 1c
```

Ideal cuando JSON es demasiado pesado.

### CBOR (Concise Binary Object Representation)

Variante binaria de JSON, muy útil en IoT y dispositivos de bajo consumo. Su uso común: sensores, sistemas embebidos, web tokens.

Ventajas:

- Representa datos estructurados como JSON pero binarizados
- Tiene buena compatibilidad con tipos complejos (fechas, mapas)
- Usa poca memoria y CPU

Ejemplo en JSON:

```json
{"temp": 22.5, "unit": "C"}
````

Codificación CBOR:

```yaml
a2 64 74 65 6d 70 fb 4036 0000 0000 00 64 75 6e 69 74 61 43
```

Compacto y eficiente para dispositivos que miden clima, por ejemplo.

## 3. FORMATOS DE EMPAQUETADO DE ARCHIVOS

Estos formatos se utilizan para agrupar varios archivos en uno solo, a menudo aplicando compresión para reducir su tamaño. Son muy comunes cuando queremos enviar carpetas completas o hacer respaldos (backups).

- **ZIP**: El más común. Comprime y agrupa archivos. Compatible con casi todos los sistemas.  
- **TAR**: Muy usado en Linux. Agrupa archivos y suele combinarse con compresión (por ejemplo, `.tar.gz`).  
- **RAR**: Ofrece una alta tasa de compresión, pero es un formato propietario.  
- **7z**: Libre y muy eficiente, útil para grandes volúmenes de archivos.  

## 4. FORMATOS MULTIMEDIA Y DE DOCUMENTOS

Estos formatos son fundamentales cuando trabajamos con contenido visual, sonoro o documentos de oficina. No estructuran datos como JSON, pero también forman parte de la transferencia de datos diaria.

### Documentos

- **PDF**: Documentos fijos  
- **DOCX**: Texto editable  
- **XLSX**: Hojas de cálculo  

### Imágenes

- **JPEG**: Fotografías  
- **PNG**: Transparencias  
- **GIF**: Animaciones cortas  
- **SVG**: Gráficos vectoriales  

### Audio y Video

- **MP3**: Audio comprimido  
- **MP4**: Video moderno  
- **AVI** y **WEBM**: Formatos de video alternativos  


## 5. FORMATOS DE TRANSFERENCIA EN RED (ENCAPSULADOS)

Estos formatos corresponden a los protocolos y estructuras que permiten enviar datos a través de redes, como Internet.

- **HTTP/HTTPS**: Protocolos que permiten transferir páginas web y datos de APIs. HTTPS es la versión segura.  
- **FTP/SFTP**: Se utilizan para subir o descargar archivos entre un cliente y un servidor. SFTP cifra la conexión, por lo que es más seguro.  
- **SCP**: Forma segura de copiar archivos entre sistemas remotos, especialmente en entornos Linux.  

### MIME Types (Formatos usados en HTTP)

Cuando se transfieren datos por HTTP, se indican los formatos mediante los llamados **MIME Types**. Algunos ejemplos comunes son:

- `application/json` → Para datos en formato JSON  
- `text/html` → Para páginas web  
- `image/png` → Para imágenes  


## CASO PRÁCTICO: DATOS DE TRANSFERENCIA EN FETCH

Cuando tus aplicaciones web consumen datos de una API, estás usando formatos de transferencia en acción. Supongamos que haces esta llamada:

```javascript
fetch('https://api.ejemplo.com/user/42')
  .then(response => console.log(response));
```

En este punto, lo que se imprime en consola es un objeto Response, que representa toda la respuesta del servidor, incluyendo:

- El código de estado HTTP (por ejemplo, 200 OK)
- Los encabezados de la respuesta
- El cuerpo de la respuesta (aún sin procesar)

Este objeto es una instancia de la clase Response, que forma parte de la API Fetch del navegador. No es directamente legible como JSON, por eso no debes tratarlo como si ya fueran los datos, sino como un sobre cerrado que aún debes abrir.

### ¿Cómo se obtienen los datos reales?
Debes transformar el cuerpo de la respuesta según el formato MIME que el servidor haya enviado. Si la API responde con JSON, entonces debes convertirlo así:

```javascript
fetch('https://api.ejemplo.com/user/42')
  .then(response => response.json())  // Convierte el cuerpo a JSON
  .then(data => console.log(data));   // Accede a los datos reales
```

Esto es equivalente a decir: “OK, ya tengo el paquete, ahora desempaquétalo como JSON".

### ¿Qué contiene la respuesta del servidor?

Un servidor que responde en JSON enviará algo como:

```pgsql
HTTP/1.1 200 OK
Content-Type: application/json
```

Y el cuerpo de la respuesta (que tú accedes con .json()) podría verse así:

```json
{
  "id": 42,
  "name": "María González",
  "email": "maria@example.com"
}
```

Aquí estás viendo:

- **Formato de datos:** JSON → el cuerpo de la respuesta
- **Protocolo de red:** HTTP → transporte del mensaje
- **Tipo MIME:** application/json → le indica al navegador cómo interpretar el cuerpo