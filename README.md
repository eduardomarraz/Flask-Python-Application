# Sistema de Gestión de Empleados

## Descripción
Este proyecto es una práctica para crear un sistema de gestión de empleados utilizando la programación orientada a objetos en Python. El sistema contiene clases que representan diferentes tipos de empleados, como empleados regulares y gerentes, y explora conceptos como herencia, polimorfismo y sobrecarga de métodos. También incluye una interfaz web construida con Flask para calcular los salarios de los empleados.

## Estructura del Proyecto
- `calcular_salario.py`: Contiene las clases `Empleado` y `Gerente` y métodos para calcular salarios.
- `app.py`: Aplicación web creada con Flask.
- `templates/`: Carpeta que contiene los archivos HTML `formulario.html` y `resultado.html`.
- `clases.py`: Archivo con las definiciones de las clases `Empleado` y `Gerente` utilizadas en la aplicación web.

## Pasos para la Implementación

### Paso 1: Definición de Clases
Se definen las clases `Empleado` y `Gerente`:
```python
class Empleado:
    def __init__(self, nombre, edad, salario_base):
        self._nombre = nombre
        self._edad = edad
        self._salario_base = salario_base

    @property
    def nombre(self):
        return self._nombre

    @nombre.setter
    def nombre(self, nuevo_nombre):
        self._nombre = nuevo_nombre

    @property
    def edad(self):
        return self._edad

    @edad.setter
    def edad(self, nueva_edad):
        self._edad = nueva_edad

    @property
    def salario_base(self):
        return self._salario_base

    @salario_base.setter
    def salario_base(self, nuevo_salario):
        self._salario_base = nuevo_salario

    def calcular_salario(self):
        return self._salario_base

class Gerente(Empleado):
    def __init__(self, nombre, edad, salario_base, bono):
        super().__init__(nombre, edad, salario_base)
        self._bono = bono

    @property
    def bono(self):
        return self._bono

    @bono.setter
    def bono(self, nuevo_bono):
        self._bono = nuevo_bono

    def calcular_salario(self):
        return super().calcular_salario() + self._bono
```

Paso 2: Implementación de Propiedades
Se agregan propiedades para los atributos en las clases Empleado y Gerente.

Paso 3: Prueba del Sistema
Crear instancias y probar los métodos:
```python
empleado1 = Empleado("Juan", 30, 50000)
gerente1 = Gerente("Ana", 35, 60000, 10000)

print(empleado1.calcular_salario())  # Salida esperada: 50000
print(gerente1.calcular_salario())   # Salida esperada: 70000
```

Paso 4: Uso de Polimorfismo
Definir una función que acepte un objeto Empleado y llame al método calcular_salario:

```python
def calcular_y_mostrar_salario(empleado):
    print(f"El salario de {empleado.nombre} es: {empleado.calcular_salario()}")

calcular_y_mostrar_salario(empleado1)  # Salida esperada: El salario de Juan es: 50000
calcular_y_mostrar_salario(gerente1)   # Salida esperada: El salario de Ana es: 70000
```

Paso 5: Probar el Código
Para probar el código, ejecuta:
```python
python3 calcular_salario.py
```

Paso 6: Crear Aplicación Web
Instalación de Flask
```python
pip install Flask
```

Código de la Aplicación Web (app.py)
```python
from flask import Flask, render_template, request
from clases import Empleado, Gerente

app = Flask(__name__)

@app.route('/')
def formulario():
    return render_template('formulario.html')

@app.route('/calcular', methods=['POST'])
def calcular():
    nombre = request.form['nombre']
    edad = int(request.form['edad'])
    salario_base = float(request.form['salario_base'])
    tipo_empleado = request.form['tipo_empleado']

    if tipo_empleado == 'gerente':
        bono = float(request.form['bono'])
        empleado = Gerente(nombre, edad, salario_base, bono)
    else:
        empleado = Empleado(nombre, edad, salario_base)

    salario_mensual = empleado.calcular_salario()
    return render_template('resultado.html', nombre=nombre, salario=salario_mensual)

if __name__ == '__main__':
    app.run(debug=True, host='0.0.0.0', port=8080)
```

Formularios HTML
templates/formulario.html:
```python
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Calculadora de Salario</title>
</head>
<body>
    <h1>Calculadora de Salario</h1>
    <form action="/calcular" method="post">
        <label for="nombre">Nombre:</label><br>
        <input type="text" id="nombre" name="nombre"><br>
        <label for="edad">Edad:</label><br>
        <input type="number" id="edad" name="edad"><br>
        <label for="salario_base">Salario Base:</label><br>
        <input type="number" id="salario_base" name="salario_base"><br>
        <label for="tipo_empleado">Tipo de Empleado:</label><br>
        <select id="tipo_empleado" name="tipo_empleado">
            <option value="empleado">Empleado</option>
            <option value="gerente">Gerente</option>
        </select><br>
        <label for="bono">Bono (solo para gerentes):</label><br>
        <input type="number" id="bono" name="bono" disabled><br>
        <input type="submit" value="Calcular">
    </form>
    <script>
        document.getElementById('tipo_empleado').addEventListener('change', function() {
            var bonoInput = document.getElementById('bono');
            bonoInput.disabled = this.value !== 'gerente';
        });
    </script>
</body>
</html>
```

templates/resultado.html:
```python
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Resultado</title>
</head>
<body>
    <h1>Resultado</h1>
    <p>El salario de {{ nombre }} es: ${{ salario }}</p>
    <a href="/">Volver al formulario</a>
</body>
</html>
```

Ejecutar la Aplicación Web
Ejecuta la aplicación con:
```python
python3 app.py
```

Accede a la IP de la instancia EC2 en el puerto 8080 para ver el formulario y calcular salarios.

Documentación
Clases y Objetos
Clases: Empleado y Gerente son plantillas para crear objetos que representan empleados y gerentes.
Atributos: nombre, edad, salario_base, y bono son características de los objetos.
Métodos: __init__ inicializa los objetos, calcular_salario calcula el salario.
Herencia
La clase Gerente hereda de Empleado, obteniendo sus atributos y métodos y añadiendo el atributo bono.
Polimorfismo
Se utiliza cuando el método calcular_salario se comporta de manera diferente según el tipo de objeto (Empleado o Gerente).
Sobrecarga de Métodos
El método calcular_salario se redefine en la clase Gerente para incluir el bono adicional.
Propiedades
Utilizamos métodos get y set para acceder y modificar los atributos de las clases.
Despliegue en EC2
Crea una instancia EC2 y abre el puerto 8080.
Instala Flask y ejecuta la aplicación:

```python
pip install Flask
python3 app.py
```

Accede a la aplicación desde el navegador usando la IP pública de tu EC2 en el puerto 8080.

