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
