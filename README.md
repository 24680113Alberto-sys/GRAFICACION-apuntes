POLIGONO EN BLENDER

¿Qué es esta práctica?
Esta práctica consiste en dibujar un polígono 2D usando código en Blender, en lugar de dibujarlo a mano con el mouse.
Usamos Python y la API de Blender (bpy) para:
Crear una malla
Calcular vértices matemáticamente
Conectar los vértices con aristas
Mostrar el polígono en la escena
Al final, el código crea un hexágono (6 lados) plano.

Instalación de Blender en Mac 
Dado que tienes un procesador M4 (Apple Silicon), es crucial descargar la versión optimizada para obtener el máximo rendimiento.
Ve al sitio oficial: Entra a blender.org/download.
Selecciona la versión correcta:
La página suele detectar tu sistema automáticamente, pero asegúrate de que diga macOS Apple Silicon (no Intel).
Si ves un menú desplegable, elige "macOS (Apple Silicon)".
Descargar e Instalar:
Descarga el archivo .dmg.
Ábrelo y arrastra el icono de Blender naranja a la carpeta de Applications (Aplicaciones).


Primer inicio:
Abre Blender desde tus aplicaciones. Al ser la primera vez, macOS te preguntará si confías en la app. Dile que sí ("Open").
En la pantalla de bienvenida, puedes dejar la configuración por defecto y hacer clic en cualquier parte vacía para cerrar el menú.

Preparando el entorno de trabajo
Blender tiene una interfaz compleja, pero para lo que haremos solo necesitas el área de programación.
En la barra superior de menús, verás pestañas como "Layout", "Modeling", etc. Busca y haz clic en la pestaña Scripting.
Verás que la interfaz cambia. La ventana más grande (generalmente a la derecha o al centro) es el Editor de Texto.
Haz clic en el botón New (Nuevo) en la parte superior de ese editor para crear un archivo de texto en blanco donde pegaremos tu código.

Código Explicado
import bpy
import math
¿Qué hace?
bpy: permite controlar Blender con Python
math: permite usar funciones matemáticas (seno, coseno, π)

Función para crear el polígono
def crear_poligono_2d(nombre, lados, radio):
    """
    Crea un polígono regular plano en el origen (0,0,0).
    Parámetros:
      - nombre: Nombre del objeto en Blender.
      - lados: Cantidad de vértices (ej. 6 para hexágono).
      - radio: Distancia del centro a los vértices.
    """
Explicación
Esta función:
nombre: nombre del objeto en Blender
lados: número de lados del polígono
radio: tamaño del polígono
Ejemplo:
3 lados → triángulo
4 lados → cuadrado
6 lados → hexágono

Crear la malla y el objeto
# 1. CREACIÓN DE DATOS (Malla y Objeto)
    # Creamos una malla vacía (la estructura de puntos)
    malla = bpy.data.meshes.new(nombre)
    # Creamos el objeto que contendrá esa malla
    objeto = bpy.data.objects.new(nombre, malla)

Explicación
Malla: estructura geométrica (vértices y aristas)
Objeto: lo que aparece en la escena

Agregar el objeto a la escena
 # 2. VINCULACIÓN
    # Para que el objeto sea visible, debemos ponerlo en la colección de la escena
    bpy.context.collection.objects.link(objeto)
Si no haces esto, el objeto existe pero no se ve.

Listas para vértices y aristas
  vertices = []
    aristas = [] # Las líneas que unen los puntos
vertices: puntos del polígono
aristas: líneas que conectan los puntos


Cálculo de los vértices (parte matemática)
# 3. CÁLCULO MATEMÁTICO (De Polar a Cartesiano)
    # Iteramos tantas veces como lados tenga el polígono
    for i in range(lados):
        # Calculamos el ángulo en radianes para este vértice específico.
        # 2*pi es un círculo completo (360 grados).
        angulo = 2 * math.pi * i / lados

        # Usamos trigonometría básica para encontrar la posición X y Y
        x = radio * math.cos(angulo)
        y = radio * math.sin(angulo)

        # Agregamos la coordenada (x, y, 0). Z es 0 porque es 2D.
        vertices.append((x, y, 0))
Explicación simple
El círculo completo tiene 360° o 2π
Se divide entre el número de lados
Se calculan coordenadas usando:
coseno → eje X
seno → eje Y
Z = 0 → figura plana (2D)
Esto coloca los puntos uniformemente en un círculo


Crear las aristas (líneas)
# 4. DEFINIR CONEXIONES (Aristas)
    # Unimos el vértice actual 'i' con el siguiente '(i+1)'
    # El operador módulo (%) sirve para que el último punto se una con el primero (0)
    for i in range(lados):
        aristas.append((i, (i + 1) % lados))
Explicación
Conecta cada vértice con el siguiente
% lados hace que el último se conecte con el primero
Así se cierra el polígono 


Cargar datos en la malla
# 5. CONSTRUCCIÓN FINAL
    # Le decimos a la malla: "Usa estos vértices y estas aristas"
    # El tercer parámetro [] es para caras (faces), que por ahora dejamos vacío.
    malla.from_pydata(vertices, aristas, [])
    
    # Actualizamos la geometría para que Blender valide los cambios
    malla.update()
Aquí Blender recibe:
Vértices
Aristas
(No hay caras, por eso está vacío [])

Limpiar la escena antes de dibujar
# --- EJECUCIÓN DEL SCRIPT ---

# A. LIMPIEZA DE ESCENA
# Seleccionamos todo lo que existe en la escena actual
bpy.ops.object.select_all(action='SELECT')
# Lo borramos (Cuidado: esto borra todo tu trabajo previo en la escena)
bpy.ops.object.delete()

Esto:
Selecciona todo
Borra todo
Evita que se acumulen figuras

Llamar a la función (crear el polígono)
# B. LLAMADA A LA FUNCIÓN
# Creamos un hexágono (6 lados) de radio 5
crear_poligono_2d("Poligono2D", lados=6, radio=5)
Resultado:
Nombre: Poligono2D
6 lados → hexágono
Tamaño: radio 5

Ejecutar el código
En el Editor de texto
Presiona:
▶ Run Script
o ⌥ Option + P
<img width="836" height="562" alt="image" src="https://github.com/user-attachments/assets/7884495a-7de6-4146-a819-e976296a8fcd" />

Codigo completo:
import bpy
import math

def crear_poligono_2d(nombre, lados, radio):
    """
    Crea un polígono regular plano en el origen (0,0,0).
    Parámetros:
      - nombre: Nombre del objeto en Blender.
      - lados: Cantidad de vértices (ej. 6 para hexágono).
      - radio: Distancia del centro a los vértices.
    """

    # 1. CREACIÓN DE DATOS (Malla y Objeto)
    # Creamos una malla vacía (la estructura de puntos)
    malla = bpy.data.meshes.new(nombre)
    # Creamos el objeto que contendrá esa malla
    objeto = bpy.data.objects.new(nombre, malla)

    # 2. VINCULACIÓN
    # Para que el objeto sea visible, debemos ponerlo en la colección de la escena
    bpy.context.collection.objects.link(objeto)

    vertices = []
    aristas = [] # Las líneas que unen los puntos

    # 3. CÁLCULO MATEMÁTICO (De Polar a Cartesiano)
    # Iteramos tantas veces como lados tenga el polígono
    for i in range(lados):
        # Calculamos el ángulo en radianes para este vértice específico.
        # 2*pi es un círculo completo (360 grados).
        angulo = 2 * math.pi * i / lados

        # Usamos trigonometría básica para encontrar la posición X y Y
        x = radio * math.cos(angulo)
        y = radio * math.sin(angulo)

        # Agregamos la coordenada (x, y, 0). Z es 0 porque es 2D.
        vertices.append((x, y, 0))

    # 4. DEFINIR CONEXIONES (Aristas)
    # Unimos el vértice actual 'i' con el siguiente '(i+1)'
    # El operador módulo (%) sirve para que el último punto se una con el primero (0)
    for i in range(lados):
        aristas.append((i, (i + 1) % lados))

    # 5. CONSTRUCCIÓN FINAL
    # Le decimos a la malla: "Usa estos vértices y estas aristas"
    # El tercer parámetro [] es para caras (faces), que por ahora dejamos vacío.
    malla.from_pydata(vertices, aristas, [])
    
    # Actualizamos la geometría para que Blender valide los cambios
    malla.update()

# --- EJECUCIÓN DEL SCRIPT ---

# A. LIMPIEZA DE ESCENA
# Seleccionamos todo lo que existe en la escena actual
bpy.ops.object.select_all(action='SELECT')
# Lo borramos (Cuidado: esto borra todo tu trabajo previo en la escena)
bpy.ops.object.delete()

# B. LLAMADA A LA FUNCIÓN
# Creamos un hexágono (6 lados) de radio 5
crear_poligono_2d("Poligono2D", lados=6, radio=5)   
