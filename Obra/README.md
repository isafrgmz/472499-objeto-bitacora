### Título obra

Comprobante o Comprobante: Brote emitido

### Integrantes

Isabela Gómez Güenul y Jonathan Soto Norambuena 

### Statement de la obra

''Comprobante'' (o ''Comprobante: Brote emitido'') es una instalación interactiva que mediante la interfaz de una impresora de boletas modificada se reemplaza la lógica mercantil de compra por la distribución gratuita de conocimiento botánico ancestral. Girando una perilla el usuario navega por un inventario de plantas que entrelaza la práctica de vida de Violeta Parra, la huerta del museo y la medicina natural, transformando un símbolo del capitalismo en un dispositivo de difusión de información.

### Imagen de referencia del trabajo

Dimensiones: 30x40x15cm (aprox, aún por definir)

### Listado de plantas Huerta Violeta Parra

- Acelga
- Ajo
- Alcachofa
- Amaranto
- Apio
- Artemisa
- Arvejas
- Berenjena
- Berros
- Betarraga
- Brócoli
- Capuchina
- Cartucho enano o Perrito
- Cebollín
- Cilantro
- Espinaca
- Flor de Ajo
- Girasol
- Grosella
- Habas
- Kale
- Lechuga
- Llantén
- Malvas 
- Menta
- Mizuna Morada
- Mizuna Verde
- Oregano
- Papas
- Puerro
- Rabanito
- Repollo
- Romero
- Ruda
- Tagetes
- Topinambur
- Zinnias

### Referencia de diseño de la boleta

================================

     COMPROBANTE DE CONSUMO     
    SABERES BOTÁNICOS EXENTO    


    FECHA: 18/06/2026  VALOR: $0 CLP

    [NOMBRE DE LA PLANTA]

    ([Nombre Científico])

    [PROPIEDADES / LO BUENO]:

    [Texto corto de los beneficios
      médicos o farmatológicos].

     [ADVERTENCIA / LO MALO]:

     [Texto corto de contraindica-
      ciones o precauciones].
  
     [SABIDURÍA TRANSMITIDA]:

    "[Relato en primera persona de 
    la abuela sobre el uso cotidiano
    de la planta en el campo]"

      Abuela [Nombre] ([Edad] años).

    "[Datos entregados por Rosa, huertera
    del Museo Violeta Parra]"
 
     Rosa Diaz  ([Edad] años).

    [ECOS DE VIOLETA]:

    "[Cita de canción, verso o
    resonancia poética vinculada
    a la planta o a la tierra]"

================================



## CÓDIGO EXITOSO

```
import serial
import time

# Configuración del adaptador USB
PUERTO = '/dev/ttyUSB0' 
BAUDIOS = 9600

try:
    # 1. Abrimos la conexión con la impresora
    print("Conectando con la impresora...")
    impresora = serial.Serial(PUERTO, BAUDIOS, timeout=1)
    time.sleep(1) # Pequeña pausa para estabilizar
    
    # 2. Mandamos el texto usando .encode('cp437') para soportar español
    impresora.write("================================\n".encode('cp437'))
    impresora.write("   MUSEO VIOLETA PARRA - TEST   \n".encode('cp437'))
    impresora.write("================================\n".encode('cp437'))
    impresora.write("\n".encode('cp437'))
    impresora.write("¡Hola! Si estas leyendo esto,\n".encode('cp437'))
    impresora.write("la Raspberry Pi y la impresora\n".encode('cp437'))
    impresora.write("ya son mejores amigas.\n".encode('cp437'))
    impresora.write("\n".encode('cp437'))
    impresora.write("Hardware superado!\n".encode('cp437'))
    
    # 3. Mandamos unos saltos de línea extra para que el papel salga
    impresora.write("\n\n\n\n\n".encode('cp437'))
    
    # 4. Cerramos la conexión
    impresora.close()
    print("¡Texto enviado con éxito!")

except Exception as e:
    print(f"Error de conexión: {e}")
```

