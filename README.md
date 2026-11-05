# Práctica 3: Uso de la Herramienta JFLAP para Convertir Autómatas Finitos Deterministas a Expresiones Regulares y Extensión de Software para Simular Autómatas No Deterministas, AFN con Transiciones Lambda y Minimización de AFD

> **Nota:** Esta práctica continúa en el repositorio del software interactivo de la **Práctica 2**. Las operaciones sobre cadenas y lenguajes (`lenguajes.py`) proceden de la **Práctica 1** y se reutilizan directamente.

## Datos Generales

- **Unidad de Aprendizaje:** Teoría de la Computación
- **Carrera:** Ingeniería en Sistemas Computacionales
- **Profesor:** Gabriel Hurtado Aviles
- **Fecha de entrega:** 4 de noviembre de 2026
- **Etiqueta de Git de la entrega:** [`practica3`](https://github.com/tynon6/Practica-2/tags)

## Nombres de los integrantes del equipo

- **Iker Gael Cruz Vargas**
- **Maria Fernanda Santos Fuentes**
- **Perla Santos Martinez**

---

## Índice

1. [Conversión de AFD a expresiones regulares (JFLAP)](https://github.com/tynon6/Practica-2/blob/main/docs/practica3/01-afd-a-er-jflap.md)
2. [Extensión del software interactivo](https://github.com/tynon6/Practica-2/blob/main/docs/practica3/02-software.md)
3. [Documentación de los algoritmos](https://github.com/tynon6/Practica-2/blob/main/docs/practica3/03-algoritmos.md)

[![Integración Continua - Práctica 3](https://github.com/tynon6/Practica-2/actions/workflows/pruebas.yml/badge.svg)](https://github.com/tynon6/Practica-2/actions/workflows/pruebas.yml)

Proyecto para el procesamiento y análisis de lenguajes formales y autómatas finitos, implementado en Python puro, con pruebas automatizadas mediante GitHub Actions (Python 3.11, 3.12 y 3.13) y contenerización con Docker. Esta práctica añade, sobre el simulador de la Práctica 2, los algoritmos que transforman un AFN con transiciones λ en un AFD mínimo, cada uno mostrado paso a paso en la interfaz.

## Requisitos Previos

- Python 3.11+
- Git
- Docker Desktop (opcional, para ejecutar en contenedor)

## Instalación y Configuración Local

1. Clonar el repositorio:

   ```bash
   git clone https://github.com/tynon6/Practica-2.git
   cd Practica-2
   ```

2. Instalar las dependencias:

   ```bash
   pip install -r requirements.txt
   ```

3. Ejecutar las pruebas:

   ```bash
   pytest -q
   ```

4. Levantar la interfaz (Flet, servida como aplicación web en http://localhost:8550):

   ```bash
   python src/app.py
   ```




### Con contenedores

```bash
docker compose build
docker compose run --rm py311 pytest -q
docker compose run --rm py312 pytest -q
docker compose run --rm py313 pytest -q
docker compose up py312      # abrir http://localhost:8550
```

## Funciones nuevas de la Práctica 3

Todas están en `src/algoritmos.py` (Python puro, sin Flet) y se muestran en la sección «Práctica 3» de la interfaz, al final de la página. Las rutas de los archivos se escriben respecto a la carpeta del repositorio (en el contenedor, `/app`).

### Uso desde la interfaz

| Sección | Qué hace | Cómo se usa |
|---|---|---|
| 0. Cargar y guardar | Carga un autómata desde `.jff`, `.json` o `.xml` | Escriba la ruta y pulse **Cargar**. Con **Guardar último resultado** se exporta el último autómata obtenido; con **Usar último resultado como actual** se encadenan las transformaciones |
| 1. Cerradura λ | λ-clausura de un estado o de todos | Escriba el estado y pulse **λ-clausura del estado**, o pulse **λ-clausura de todos** |
| 2. Simulación directa | Simula el AFN-λ sin convertirlo | Escriba la cadena (`λ` para la vacía) y pulse **Simular paso a paso**. Muestra los estados activos antes y después de la λ-clausura |
| 3. Eliminación de λ | AFN-λ a AFN sin transiciones λ | Pulse **Eliminar λ**. Indica las transiciones que se añaden y los estados que pasan a ser de aceptación |
| 4. Subconjuntos | AFN a AFD, solo estados alcanzables | Pulse **Construir AFD**. Muestra la tabla conforme se genera y por qué cada subconjunto es o no de aceptación |
| 5. Minimización y equivalencia | AFD a AFD mínimo, y comparación de dos autómatas | Pulse **Minimizar AFD** (muestra la partición de cada ronda). Para comparar, escriba la ruta del segundo autómata y pulse **¿Son equivalentes?** |
| 6. Validación de cadenas | Valida un archivo con una cadena por línea | Escriba la ruta del archivo y la del informe, y pulse **Validar y guardar informe** |

Flujo típico: cargar un AFN-λ, **Eliminar λ**, **Usar último resultado como actual**, **Construir AFD**, **Usar último resultado como actual** y **Minimizar AFD**.

### Uso desde Python

```python
from formatos import load_automaton
import algoritmos as alg

m = load_automaton("automatas/practica3/casos-de-prueba/ejemplo.jff")

clausuras, pasos = alg.clausuras_lambda(m)           # λ-clausura de cada estado
r = alg.simulacion_directa(m, "ab")                  # r.aceptada, r.pasos, r.conjuntos
sin_lambda = alg.eliminar_lambda(m)                  # .automata, .transiciones_agregadas, .nuevos_finales
afd = alg.construccion_subconjuntos(sin_lambda.automata)   # .automata, .tabla, .razones_aceptacion
minimo = alg.minimizar(afd.automata)                 # .automata, .rondas, .inaccesibles
alg.equivalentes(m, minimo.automata).equivalentes    # True
alg.validar_archivo(m, "cadenas.txt", "evidencias/practica3/informe.txt")
```

### Formato del archivo de cadenas

Una cadena por línea. La **cadena vacía se escribe `λ`**. Las líneas en blanco se ignoran. Una cadena con símbolos fuera del alfabeto no detiene la validación: se registra como error en el informe.

### Convención para λ en los archivos

La transición lambda se escribe con el símbolo leído vacío (`<read/>` en `.jff`, `"lee": ""` en `.json`, `lee=""` en `.xml`). Al leer, también se aceptan `λ`, `ε`, `lambda` y `epsilon`.

## Pruebas automatizadas

`tests/test_algoritmos.py` comprueba, para cada caso de prueba, que el autómata original, el AFN sin transiciones λ, el AFD de subconjuntos y el AFD mínimo aceptan **exactamente las mismas cadenas** hasta longitud 8, que el AFD mínimo no tiene más estados que el AFD del que procede y que la equivalencia es exacta (autómata producto). Además de los autómatas definidos en el archivo, la prueba recorre automáticamente todos los `.jff` de `automatas/practica3/casos-de-prueba/`.

```bash
pytest -q tests/test_algoritmos.py
```

## Estructura del repositorio

```text
.
├── README.md
├── .gitignore  .dockerignore  Dockerfile  compose.yml  pytest.ini  requirements.txt
├── .github/workflows/pruebas.yml
├── docs/
│   ├── 01-entorno.md  02-JFLAB.md  03-simulador.md  04-investigacion.md  05-articulo.md   # Práctica 2
│   └── practica3/
│       ├── 01-afd-a-er-jflap.md      # Ejercicio 1
│       ├── 02-software.md            # Ejercicio 2, con capturas
│       └── 03-algoritmos.md          # Ejercicio 3
├── automatas/
│   └── practica3/
│       ├── afd-a-er/                 # los cinco .jff y sus expresiones
│       └── casos-de-prueba/          # los .jff de la Lista 3 usados en las pruebas
├── src/
│   ├── lenguajes.py  automata.py  formatos.py  visualizacion.py
│   ├── algoritmos.py                 # Práctica 3: algoritmos paso a paso
│   ├── panel_p3.py                   # Práctica 3: interfaz de los algoritmos
│   └── app.py
├── tests/
│   ├── test_lenguajes.py  test_automata.py
│   └── test_algoritmos.py            # Práctica 3
└── evidencias/
    └── practica3/
```

## Entrega

La entrega se identifica con la etiqueta de Git `practica3`:

```bash
git switch main
git pull
git tag -a practica3 -m "Entrega de la Practica 3"
git push origin practica3
```

---

## Prácticas anteriores

### Práctica 2: Autómatas Finitos No Deterministas (AFND) y Simulación de AFD con Interfaz Gráfica

1. [Entorno de trabajo y flujo de ramas](https://github.com/tynon6/Practica-2/blob/main/docs/01-entorno.md)
2. [AFND y AFND-λ en JFLAP](https://github.com/tynon6/Practica-2/blob/main/docs/02-JFLAB.md)
3. [Simulador de autómatas con interfaz gráfica](https://github.com/tynon6/Practica-2/blob/main/docs/03-simulador.md)
4. [Investigación y estado del arte](https://github.com/tynon6/Practica-2/blob/main/docs/04-investigacion.md)
5. [Del artículo al autómata](https://github.com/tynon6/Practica-2/blob/main/docs/05-articulo.md)
