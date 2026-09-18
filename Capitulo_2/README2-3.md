# Laboratorio 2.3 – Paquetes distribuibles

## Objetivo de la práctica:
Al finalizar la práctica, serás capaz de:
- Crear un paquete distribuible a partir del paquete `MiMath` de las prácticas anteriores.
- Instalar dicho paquete con `pip`, de modo que sea importable desde cualquier carpeta del sistema.
- Comprender por qué Python no encuentra un paquete que no está instalado ni en la ruta de búsqueda.

## Objetivo Visual
```mermaid
flowchart LR
    A["Paquete MiMath (código fuente)"] --> B["setup.py"]
    B --> C["python setup.py sdist"]
    C --> D["dist/MiPaqueteBB-1.0.tar.gz"]
    D --> E["pip install MiPaqueteBB-1.0.tar.gz"]
    E --> F["Instalado en site-packages"]
    F --> G["Test.py funciona desde cualquier carpeta"]
```

## Duración aproximada:
- 25 minutos.

## Tabla de ayuda:
| Herramienta | Versión / Detalle | Notas |
| --- | --- | --- |
| Python | 3.10 o superior | |
| pip | Incluido desde Python 3.4 | Verifica con `pip -V` |
| setuptools | Se instala junto con pip en la mayoría de distribuciones | Necesario para `setup.py` |
| Carpeta de trabajo | `ws_curso` | Contiene `MiMath/` y `Test.py` de las prácticas 2.1 y 2.2 |
| Nombre del paquete distribuible | `MiPaqueteBB` | Definido en `setup.py` |

## Instrucciones

### Tarea 1. Reproducir el problema: ejecutar el programa fuera de su carpeta
Paso 1. Crear un nuevo directorio llamado `test`, **fuera** de `ws_curso` (por ejemplo, como carpeta hermana de `ws_curso`).

Paso 2. Copiar el archivo `Test.py` (de la práctica 2.2) dentro de la carpeta `test` recién creada. **No** copies la carpeta `MiMath`.

Paso 3. Cambiar a ese directorio en la terminal y ejecutar el archivo:

```bash
cd test
python Test.py
```

> **Resultado esperado:**
> ```
> Traceback (most recent call last):
>   File "Test.py", line 1, in <module>
>     from MiMath.operaciones_basicas.ope import suma, multi
> ModuleNotFoundError: No module named 'MiMath'
> ```
>
> **¿Por qué Python no encontró el paquete?** Cuando ejecutas un script, Python busca los módulos a importar en una lista de ubicaciones (`sys.path`): la carpeta del script en ejecución, las rutas indicadas en la variable de entorno `PYTHONPATH`, y las carpetas de paquetes instalados (`site-packages`). Como `MiMath` no está en ninguna de esas ubicaciones desde la carpeta `test`, la importación falla.
>
> **¿Cómo se corrige este problema?** Existen varias formas (copiar `MiMath` junto al script, agregar su ruta a `PYTHONPATH`), pero la solución profesional y escalable es **instalar** el paquete con `pip`, de modo que quede disponible en `site-packages` y sea importable desde cualquier ubicación — que es justamente lo que haremos en las siguientes tareas.

### Tarea 2. Describir el paquete distribuible con `setup.py`
Paso 1. En el directorio **padre** de la carpeta `MiMath` (es decir, dentro de `ws_curso`, al mismo nivel que `MiMath/` y `Test.py`), crear un archivo de nombre `setup.py`.

Paso 2. Capturar el siguiente contenido, que describe los metadatos del paquete a distribuir:

```python
# Archivo del paquete distribuible

from setuptools import setup

setup(
    name="MiPaqueteBB",              # Nombre del paquete
    version="1.0",                   # Versión del paquete
    description="Paquete creado para la práctica 2.3 y ver el uso de paquetes distribuibles",
    author="Netec",                  # Autor
    author_email="operaciones@netec.com",  # Correo del autor
    url="https://www.netec.com",     # Sitio del autor
    packages=["MiMath", "MiMath.operaciones_basicas"],  # Paquetes y subpaquetes

    # Para más variables del setup ir a la documentación oficial
    # docs.python.org
)
```

> **¿Qué hace `setup()`?** Es la función de `setuptools` que describe un proyecto Python como paquete distribuible: su nombre, versión, autor y, muy importante, la lista de `packages` que deben incluirse al construir el paquete.
>
> ⚠️ **Corrección importante:** fíjate que la lista `packages` usa `"MiMath.operaciones_basicas"`, **no** `"MiMath.ope"` — debe coincidir exactamente con el nombre del **subpaquete** (la carpeta) creado en la práctica 2.1, no con el nombre del módulo (`ope.py`) que vive dentro de él. Un error común es confundir el nombre del subpaquete con el del módulo; si `packages` está mal escrito, el subpaquete completo (junto con `ope.py`) no se incluirá en el paquete distribuible.

### Tarea 3. Construir el paquete distribuible
Paso 1. En una terminal, posicionarse en el directorio donde está `setup.py` (es decir, `ws_curso`).

Paso 2. Ejecutar la siguiente instrucción para construir el paquete:

```bash
python setup.py sdist
```

> **¿Qué hace este comando?** `sdist` (*source distribution*) empaqueta el código fuente de tu proyecto —siguiendo la descripción de `setup.py`— en un único archivo comprimido, listo para distribuirse e instalarse con `pip`.
>
> 💡 **Nota:** en proyectos modernos, `python setup.py sdist` se considera una forma antigua de construir paquetes; la alternativa recomendada actualmente es `python -m build`. Sin embargo, usaremos el comando clásico en esta práctica porque `setuptools` sigue soportándolo y es el que ilustra de forma más directa el propósito de `setup.py`.

Paso 3. Verificar que se crearon dos nuevos elementos junto a `setup.py`: la carpeta `dist/` y la carpeta `MiPaqueteBB.egg-info/`.

Paso 4. Dentro de `dist/`, localizar el archivo `MiPaqueteBB-1.0.tar.gz`.

> **¿Qué son los archivos `.tar.gz`?** `tar` es un formato de empaquetado (agrupa varios archivos y carpetas en uno solo) originario de sistemas Unix, y `.gz` indica que ese paquete además fue comprimido con el algoritmo gzip. Es el formato estándar para distribuir código fuente de paquetes Python. Si tienes dudas adicionales sobre el formato, consúltalo con tu instructor.

### Tarea 4. Instalar y verificar el paquete distribuible
Paso 1. Cambiar al directorio `dist`.

```bash
cd dist
```

Paso 2. Listar los paquetes instalados actualmente con `pip`, antes de instalar el nuevo.

```bash
pip list
```

> **¿Qué hace este comando?** Muestra, en orden alfabético, el nombre y la versión de cada paquete instalado en el entorno de Python activo. Es útil ejecutarlo antes y después de instalar algo, para confirmar el cambio.

Paso 3. Instalar el paquete distribuible recién creado.

```bash
pip install MiPaqueteBB-1.0.tar.gz
```

> **Resultado esperado:** un mensaje similar a `Successfully installed MiPaqueteBB-1.0`.

Paso 4. Ejecutar nuevamente `pip list` y confirmar que `MiPaqueteBB` ahora aparece en la lista, en su posición alfabética correspondiente.

### Tarea 5. Confirmar que el paquete es importable desde cualquier carpeta
Paso 1. Regresar al directorio `test` creado en la Tarea 1.

```bash
cd ../../test
```

Paso 2. Ejecutar nuevamente `Test.py`.

```bash
python Test.py
```

> **Resultado esperado:**
> ```
> Suma: 41
> Multiplicación: 30030
> ```
> **Explicación:** ahora que `MiMath` está instalado como paquete en `site-packages` (una ubicación que Python siempre incluye en su búsqueda de módulos), `Test.py` puede importarlo sin importar desde qué carpeta se ejecute.

Paso 3. Desinstalar el paquete para comprobar el efecto contrario.

```bash
pip uninstall MiPaqueteBB
```

> Este comando solicitará una confirmación (`Proceed (Y/n)?`); confirma con `Y`.

Paso 4. Ejecutar `Test.py` una vez más desde la carpeta `test`.

```bash
python Test.py
```

> **Resultado esperado:** el mismo error `ModuleNotFoundError: No module named 'MiMath'` de la Tarea 1, ya que al desinstalar el paquete, Python vuelve a no encontrarlo en ninguna ubicación de búsqueda.

### Resultado esperado
Al concluir la práctica, deberás haber observado tres comportamientos distintos para el mismo `Test.py`, sin modificarlo en ningún momento:

| Momento | Comando | Resultado |
| --- | --- | --- |
| Antes de crear el paquete distribuible | `python Test.py` (desde `test/`) | `ModuleNotFoundError` |
| Después de `pip install` | `python Test.py` (desde `test/`) | `Suma: 41` / `Multiplicación: 30030` |
| Después de `pip uninstall` | `python Test.py` (desde `test/`) | `ModuleNotFoundError` nuevamente |
