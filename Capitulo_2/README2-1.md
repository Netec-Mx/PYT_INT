# Laboratorio 2.1 – Paquetes

## Objetivo de la práctica:
Al finalizar la práctica, serás capaz de:
- Reafirmar la creación de paquetes y subpaquetes en Python.
- Utilizar `*args` para escribir funciones que reciben un número variable de argumentos.
- Organizar código relacionado dentro de una jerarquía de carpetas y módulos.

## Objetivo Visual
La siguiente estructura de carpetas es la que construiremos en esta práctica:

![Imagen 01](../images/imagen01.png)

## Duración aproximada:
- 20 minutos.

## Tabla de ayuda:
| Herramienta | Versión / Detalle | Notas |
| --- | --- | --- |
| Python | 3.10 o superior | |
| Visual Studio Code | Última versión estable | |
| Carpeta de trabajo | `ws_curso` | Todo el paquete `MiMath` se crea dentro de esta carpeta |
| Paquete a crear | `MiMath` | |
| Subpaquete a crear | `operaciones_basicas` | |
| Módulo a crear | `ope.py` | |

## Instrucciones

### Tarea 1. Crear la estructura de paquete y subpaquete
Paso 1. Dentro de `ws_curso`, crear una carpeta de nombre `MiMath`. Esta carpeta será tu **paquete**.

Paso 2. Dentro de `MiMath`, crear un archivo vacío de nombre `__init__.py`.

> **¿Qué hace este archivo?** `__init__.py` es lo que le indica a Python que una carpeta debe tratarse como un **paquete** importable (y no como una carpeta común). Puede estar vacío o contener código de inicialización del paquete; en esta práctica lo dejaremos vacío.

Paso 3. Dentro de `MiMath`, crear una segunda carpeta de nombre `operaciones_basicas`. Esta será tu **subpaquete**.

Paso 4. Dentro de `operaciones_basicas`, crear también un archivo vacío de nombre `__init__.py`.

> **¿Por qué cada carpeta necesita su propio `__init__.py`?** Cada nivel de la jerarquía (el paquete y cada subpaquete) debe marcarse individualmente como importable. Si `operaciones_basicas` no tuviera su `__init__.py`, Python no podría reconocerlo como parte del paquete `MiMath` en versiones de Python anteriores a la 3.3, y aunque las versiones más recientes lo permiten mediante "paquetes de espacio de nombres", es una buena práctica seguir incluyéndolo de forma explícita para mayor claridad y compatibilidad.

### Tarea 2. Crear el módulo con las funciones de operaciones
Paso 1. Dentro de `operaciones_basicas`, crear un archivo de nombre `ope.py`. Este será tu **módulo**.

Paso 2. En `ope.py`, definir una función `suma()` que reciba un número variable de argumentos enteros y devuelva su suma.

```python
def suma(*args):
    total = 0
    for numero in args:
        total += numero
    return total
```

> **¿Qué hace `*args`?** El asterisco antes de `args` le indica a Python que empaquete **cualquier cantidad** de argumentos posicionales que reciba la función dentro de una tupla llamada `args`. Así, `suma(1, 2, 3)` recibe internamente `args = (1, 2, 3)`.
>
> **¿Por qué se usa este enfoque?** Sin `*args`, tendríamos que definir un número fijo de parámetros (`def suma(a, b, c)`), lo que obligaría a quien use la función a pasar siempre esa misma cantidad de valores. Con `*args`, la función es flexible y acepta desde cero hasta cualquier cantidad de números.
>
> **Resultado esperado al recorrer `args` con el ciclo `for`:** cada elemento de la tupla se suma de manera acumulativa a `total`, que inicia en `0`.

Paso 3. En el mismo archivo `ope.py`, definir una función `multi()` que reciba un número variable de argumentos enteros y devuelva su multiplicación.

```python
def multi(*args):
    resultado = 1
    for numero in args:
        resultado *= numero
    return resultado
```

> **¿Por qué `resultado` inicia en `1` y no en `0`?** Porque `0` es el elemento neutro de la suma (cualquier número más `0` no cambia), mientras que `1` es el elemento neutro de la multiplicación (cualquier número por `1` no cambia). Si `resultado` iniciara en `0`, el producto final sería siempre `0`, sin importar los valores recibidos.

### Tarea 3. Probar las funciones
Paso 1. Al final de `ope.py`, agregar código de prueba que invoque ambas funciones con los valores `1, 2, 3` y despliegue el resultado.

```python
print(suma(1, 2, 3))
print(multi(1, 2, 3))
```

Paso 2. Ejecutar el archivo `ope.py` directamente desde la terminal para verificar el resultado.

```bash
python ope.py
```

> **Resultado esperado:**
> ```
> 6
> 6
> ```
> **Explicación:** `suma(1,2,3)` da `1+2+3 = 6`, y `multi(1,2,3)` da `1*2*3 = 6` — coinciden numéricamente en este caso particular, pero por razones distintas (suma vs. producto).
>
> 💡 **Buena práctica:** en la práctica 2.2 importarás estas funciones desde otro archivo. Cuando eso ocurra, estas dos líneas de prueba se ejecutarían también automáticamente y "contaminarían" la salida del programa que importe el módulo. Más adelante protegerás este código de prueba usando `if __name__ == "__main__":`, un patrón que verás en la siguiente práctica.

### Resultado esperado
Tu estructura final de carpetas y archivos debe verse así:

```
ws_curso/
└── MiMath/
    ├── __init__.py
    └── operaciones_basicas/
        ├── __init__.py
        └── ope.py
```

Y al ejecutar `python ope.py` desde la carpeta `operaciones_basicas`, la consola debe mostrar:

```
6
6
```
