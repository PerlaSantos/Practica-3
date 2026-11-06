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

## Contenido de la entrega

- [Punto 1.1: conversiones de los ejercicios 151–155](docs/practica3/01-afd-a-er-jflap.md)
- [Punto 1.2: cálculos de la recurrencia](docs/practica3/02-calculos-Rij.md)
- [Capturas y comprobaciones](evidencias/practica3/afd-a-er/README.md)

## Archivos de autómatas

Los AFD utilizados y los NFA obtenidos en JFLAP están en `automatas/practica3/afd-a-er/`. Las capturas de los pasos de eliminación, expresiones y comparaciones están organizadas por ejercicio en `evidencias/practica3/afd-a-er/`. Los archivos CSV reúnen las cadenas revisadas y la comprobación exacta de equivalencia.

## Ejecución y pruebas del simulador

El repositorio conserva los archivos base del simulador y sus pruebas automatizadas. Para ejecutarlas, instala las dependencias con `pip install -r requirements.txt` y ejecuta `pytest -q`.
