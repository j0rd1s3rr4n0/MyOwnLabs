
```json
##### Caso GEO

Un archivo de imagen se ha encontrado en un dispositivo digital secuestrado durante una investigación de delitos cibernéticos. La fotografía puede ser crucial para entender el contexto de un incidente. Tu tarea es determinar dónde y cuándo se tomó esta fotografía.  
  
¿Dónde fue tomada la fotografía?  
  
FORMATO:  CTF{NAME_ADDRESS}  

Ejemplo:  
30º 63' 43.80" N, 21º 37' 10.02" W  > CTF{30_63_43_80_N_21_37_10_02_W}  

Can you find the password? Enter the password as flag in the following form: CTF{passwordhere}
```

Procederemos a Descargar el archivo y guardarlo en una carpeta donde podamos localizarlo de manera ordenada, haciendo *hovering* vemos que se trata de una imagen `.jpg`
![[Pasted image 20250517085907.png]]
![[Pasted image 20250517090201.png]]:


![[Pasted image 20250517085313.png]]
A continuación abrimos Autopsy: esta plataforma forense gráfica integra extracción de metadatos EXIF, análisis hexadecimal y mapeo geolocalizado en un único entorno visual. Aunque podríamos optar por `exiftool` para una extracción rápida de metadatos en línea de comandos, Autopsy agiliza el flujo de trabajo al combinar estas funciones con un visor de contenido y módulos de ingestión configurables.

![[Pasted image 20250517090022.png]]
Procederemos a Generar el espacio de trabajo donde vamos a realizar el análisis del archivo.
Formulario de creación de caso: definimos `Forensic Case 1` y establecemos el directorio raíz. 

![[Pasted image 20250517090439.png]]
Nota: en este análisis omitimos intencionadamente los campos opcionales de Autopsy como número de caso, nombre del examinador, teléfono, correo electrónico, notas adicionales y organización, porque no se generará un reporte formal.

![[Pasted image 20250517090917.png]]
Opción `Generate new host name based on data source name`: genera un alias único a partir del nombre del archivo. Esto facilita el mapeo de resultados cuando se importan múltiples fuentes con nombres similares. Al automatizar la generación del nombre de host, evitamos colisiones manuales y garantizamos consistencia en los registros. Cada artefacto importado se asocia inequívocamente con su fuente original, lo que mejora la trazabilidad, simplifica la auditoría y reduce errores humanos en entornos multi-caso.

![[Pasted image 20250517091003.png]]
En este menú de ingestión deseleccionamos otros módulos de análisis de disco completo y dejamos marcada únicamente la opción **Logical Files**. De esta forma, Autopsy se enfocará exclusivamente en los archivos lógicos dentro de la imagen importada —en nuestro caso, picture.jpg— lo que reduce el tiempo de procesamiento y evita cargar artefactos innecesarios como registros de sistema o particiones completas.

![[Pasted image 20250517091150.png]]
Ventana de "**Add Data Source**": al hacer clic en `Add`, vinculamos el archivo físico al caso forense. Seleccionar `picture.jpg` aquí es crucial pues define el alcance de los módulos de ingestión posteriores. Esta acción asegura que Autopsy indexe y procese exclusivamente los datos de interés, evitando la inclusión involuntaria de archivos no relacionados.

![[Pasted image 20250517091314.png]]
Opciones de ingestión configuradas al 100%: dejamos todas las casillas marcadas. Mantener todos los módulos activos garantiza que no perdamos ningún artefacto forense relevante, aunque incrementa el tiempo de procesamiento. En entornos de alto volumen se pueden desactivar módulos secundarios para optimizar rendimiento, pero en un caso puntual como este, la exhaustividad es prioritaria.

![[Pasted image 20250517091409.png]]
Confirmación y lanzamiento del proceso haciendo clic en `Finish`: este paso envía todas las configuraciones de ingestión al motor de Autopsy, iniciando el análisis automático. Autopsy comenzará a escanear el archivo, ejecutar cada módulo seleccionado y almacenar los resultados en la base de datos del caso. Es crucial no interrumpir esta fase para garantizar que todos los artefactos se indexen correctamente.

![[Pasted image 20250517091922.png]]
En el caso en que no este listando nada, como a mi. Será necesario Expandir en la parte izquierda de Autopsy el Menu de `Data Sources` ir al `LogicalFileSet` y una vez nos salga eso tendremos que  presionar clic derecho y presionar la opción de `Run Ingest Modules`, quizás puede dar error de licencia en `Cyber Triage Malware Scanner` así que en este caso vamos a desactivarlo. Una vez pasado el menú Autopsy procedfera a realizar la ingesta de datos

![[Pasted image 20250517092146.png]]
Si Autopsy no lista ningún artefacto tras la ingestión inicial, abrimos el panel izquierdo en `Data Sources` y expandimos el nodo `LogicalFileSet`. A continuación, hacemos clic derecho sobre `LogicalFileSet` y seleccionamos `Run Ingest Modules`. Este paso fuerza la re-ejecución de todos los módulos configurados.

En la esquina inferior derecha de la interfaz aparece un indicador de estado: un icono **rojo** señala un error, uno **amarillo** indica que aún se está procesando o indexando, y uno **azul** refleja que el proceso ha finalizado correctamente. Al hacer clic en ese círculo se despliega información detallada sobre el módulo que generó el estado.

En mi caso, el icono se volvió rojo y, al expandirlo, identifiqué un error de licencia en el módulo `Cyber Triage Malware Scanner`. Desactivamos dicho módulo desde el diálogo de configuración y reejecutamos los restantes, asegurando que la extracción de metadatos y el análisis de archivos se completaran sin bloqueos.

![[Pasted image 20250517092741.png]]
Tras completarse la ingestión de datos, navegamos a la sección `Images/Videos` en el panel izquierdo. Aquí Autopsy muestra en una vista central todas las miniaturas de los archivos multimedia extraídos, permitiéndonos filtrar por tipo (imagen, vídeo), ordenar por fecha de creación o tamaño, y acceder rápidamente a metadatos resumidos sin abrir cada archivo individualmente

![[Pasted image 20250517092957.png]]
Una vez en `Images/Videos`, Autopsy muestra la miniatura de `picture.jpg` junto a un panel de detalles (derecha) con atributos clave:

- **MD5 Hash (cfac731cf50a20fde2b97c9d10271fec)**: permite verificar integridad y detectar modificaciones o duplicados.
- **Camera Make y Model**: indica que la imagen fue capturada con un Apple iPhone 4s, lo cual aporta contexto al dispositivo involucrado.
- **Created Time / Modified Time**: fechas de procesamiento por Autopsy, no confundir con la fecha EXIF de captura.
- **Path**: confirma la fuente (`/LogicalFileSet2`), útil al correlacionar artefactos en casos con múltiples data sources.
- **Tags & Categorías**: en la parte inferior izquierda, Autopsy propone estas categorías predeterminadas para clasificar imágenes:
    - CGI/Animation – Child Exploitive
    - Child Abuse Material (CAM)
    - Child Exploitive (Non-CAM) Age Difficult
    - Comparison Images
    - Non-Pertinent

_Este menú nos permite no solo visualizar información esencial de un vistazo (hash, origen, tipo de archivo), sino también asignar_ **_Tags_** _y_ **_Categorías_** _para organizar el análisis de manera estructurada._

![[Pasted image 20250517093357.png]]
Si hacemos clic derecho sobre la miniatura y seleccionamos **Show Content Viewer**, se despliega un conjunto de pestañas para examinar la imagen desde varias perspectivas:

- **Hex**: inspección hexadecimal para confirmar firmas (`FF D8 FF`), ubicar segmentos EXIF y detectar posibles datos ocultos o incrustaciones maliciosas.    ![[Pasted image 20250517093713.png]]
    
- **Text**: ejecución de `strings` para extraer fragmentos legibles (rutas, URLs, etiquetas XMP), ideal para hallar referencias internas.   ![[Pasted image 20250517093723.png]]
    
- **Application**: información sobre el dispositivo o software creador (modelo de cámara, versión de firmware), que aporta contexto sobre el origen del archivo.![[Pasted image 20250517093732.png]]
    
- **File Metadata**: metadatos del sistema de archivos (tamaño, fechas de creación/modificación por Autopsy, ruta local), distintos de los EXIF originales.    ![[Pasted image 20250517093740.png]]
    
- **Analysis Results**: consolidación de metadatos EXIF más críticos, como fecha y hora de captura, coordenadas GPS y altitud.    ![[Pasted image 20250517093747.png]] _Ejemplo: fecha de captura_ `2012-12-03 13:26:00` _y GPS Lat_ `15.658166667`_, Lon_ `-88.992166667`_._
    
- **Annotations**: espacio para notas del investigador; en este caso ninguna anotación previa.    ![[Pasted image 20250517093758.png]]
    
- **Other Occurrences**: lista todas las instancias detectadas de este archivo en otros data sources o casos, facilitando la trazabilidad de duplicados.
![[Pasted image 20250517093830.png]]

Cada pestaña complementa las demás, proporcionando desde datos en crudo hasta metadatos estructurados que agilizan el análisis forense.
![[Pasted image 20250517095243.png]]  
Al acceder al menú **Geolocation**, Autopsy renderiza un globo terráqueo interactivo con un pin en la ubicación aproximada basada en los metadatos GPS extraídos. En esta vista podemos rotar y acercar el globo para comprobar si las coordenadas apuntan a una zona terrestre, costera o marítima, lo que ayuda a validar la fiabilidad de los datos.

Copiar las coordenadas decimales: `15.658166666666666, -88.99216666666666`.

![[Pasted image 20250517095023.png]]  
En la esquina superior derecha aparece un panel con los detalles de **Latitude** y **Longitude** en formato decimal. Desde aquí copiamos cómodamente los valores para usarlos en herramientas externas como Google Maps o scripts de conversión. Además, se muestran otros metadatos de geolocalización (altitud, precisión, datum) que permiten evaluar la exactitud de la posición.


**Verificación en Google Maps**
![[Pasted image 20250517095023.png]]
![[Pasted image 20250517095500.png]]  
El panel inferior revela el Plus Code (_`M255+748 Río Dulce, Guatemala`_), que actúa como un identificador de ubicación preciso incluso en zonas sin direcciones formales. La conversión a DMS proporcionada por Google (visible junto al buscador) se puede usar directamente para el reto o como comprobación antes de usar scripts de conversión. Además, la vista de calle (Street View) ayuda a validar visualmente características del entorno captado.

Google maps nos ayuda a pasar de Formato **grados decimales** DD a DMS pero en el caso de que no disponamos de Google Maps nos podriamos montar un script como el siguiente:

```python
# Coordenadas decimales proporcionadas
lat_decimal = 15.658166666666666  # Latitud en formato decimal
lon_decimal = -88.99216666666666  # Longitud en formato decimal

# Función para convertir coordenadas decimales a grados, minutos y segundos (DMS)
def decimal_to_dms(decimal):
    degrees = int(decimal)
    minutes_decimal = abs(decimal - degrees) * 60
    minutes = int(minutes_decimal)
    seconds = (minutes_decimal - minutes) * 60
    return degrees, minutes, seconds

# Convertir la latitud y longitud de decimal a DMS
lat_dms = decimal_to_dms(lat_decimal)  # Convertir latitud
lon_dms = decimal_to_dms(lon_decimal)  # Convertir longitud

# Determinar la dirección para la latitud (N o S) y longitud (E o W)
lat_direction = "N" if lat_decimal >= 0 else "S"
lon_direction = "E" if lon_decimal >= 0 else "W"

# Crear una función para alinear y mostrar la tabla
def print_table():
    # Definir los datos de la tabla
    headers = ["Coordenada", "Grados", "Minutos", "Segundos", "Dirección"]
    lat_data = ["Latitud", f"{lat_dms[0]}", f"{lat_dms[1]}", f"{lat_dms[2]:.1f}", lat_direction]
    lon_data = ["Longitud", f"{lon_dms[0]}", f"{lon_dms[1]}", f"{lon_dms[2]:.1f}", lon_direction]
    
    # Calcular el ancho máximo de cada columna
    column_widths = [max(len(str(item)) for item in col) for col in zip(headers, lat_data, lon_data)]
    
    # Función para imprimir una fila de la tabla
    def print_row(row):
        print("| " + " | ".join([str(item).ljust(column_widths[i]) for i, item in enumerate(row)]) + " |")
    
    # Calcular la línea de separación
    separator = "+" + "+".join(["-" * (width + 2) for width in column_widths]) + "+"
    
    # Imprimir la tabla
    print(separator)
    print_row(headers)
    print(separator)
    print_row(lat_data)
    print_row(lon_data)
    print(separator)

# Llamar a la función para mostrar la tabla
print_table()

# Construir el formato de coordenadas: 15°39'29.4"N 88°59'31.8"W
lat_formatted = f"{lat_dms[0]}°{lat_dms[1]}'{lat_dms[2]:.2f}\"{lat_direction}"
lon_formatted = f"{lon_dms[0]}°{lon_dms[1]}'{lon_dms[2]:.2f}\"{lon_direction}"

# Imprimir el resultado final con las coordenadas en formato adecuado
print(f"\nCoordenadas: {lat_formatted} {lon_formatted}\n")
```

**Output:**
```r
+------------+--------+---------+----------+-----------+
| Coordenada | Grados | Minutos | Segundos | Dirección |
+------------+--------+---------+----------+-----------+
| Latitud    | 15     | 39      | 29.4     | N         |
| Longitud   | -88    | 59      | 31.8     | W         |
+------------+--------+---------+----------+-----------+

Coordenadas: 15°39'29.40"N 88°59'31.80"W
```

_Comentario:_ Obsérvese que en el resultado final **los segundos pasan a dos decimales** (`29.40"` y `31.80"`) y las coordenadas negativas se transforman en direcciones cardinales (**W** para longitud). Estas transformaciones son necesarias para cumplir con el formato del reto.


#### Formateo Final de la Flag

Una vez obtenidos los valores DMS con dos decimales y direcciones cardinales:

1. Eliminamos los símbolos (`°`, `'`, `"`) y las comas, reemplazándolos por guiones bajos `_`.
2. Aseguramos que cada componente (grados, minutos, segundos con dos decimales, dirección) esté separado por un único guión bajo.
3. Construimos la cadena bajo el formato `CTF{...}`.

```json
15°39'29.40"N 88°59'31.80"W → 15_39_29_40_N_88_59_31_80_W
15_39_29_40_N_88_59_31_80_W → CTF{15_39_29_40_N_88_59_31_80_W}
```

> [!note] **Nota:** 
> No incluimos signos negativos; la dirección cardinal sustituye al signo. Cada número debe ocupar al menos dos dígitos si es posible (00–59 para minutos/segundos). El orden exacto es: lat_grados_lat_min_lat_seg_lat_dir_lon_grados_lon_min_lon_seg_lon_dir.

**Flag final:** `CTF{15_39_29_40_N_88_59_31_80_W}` *(`29.40"` y `31.80"`) y las coordenadas negativas se transforman en direcciones cardinales (**W** para longitud). Estas transformaciones son necesarias para cumplir con el formato del reto.