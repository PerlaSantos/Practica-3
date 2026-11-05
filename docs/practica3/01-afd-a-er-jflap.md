# Ejercicio 1. AFD a expresiones regulares

**Alcance.** Se seleccionaron los ejercicios 151–155 de la Lista 3. Las tablas fuente se transcribieron de `T3-TC (1).pdf`; los `.jff` acompañantes se reconstruyeron desde esas tablas. El repositorio de Práctica 2 contiene otros `.jff` con esos nombres que no corresponden a las tablas del PDF, por lo que no se reutilizaron.

**Notación.** `+` denota unión, la yuxtaposición concatenación, `*` cerradura de Kleene y `λ` la cadena vacía. Las expresiones de eliminación conservan el inicio A, agregan un único final auxiliar y conectan los finales originales con λ a ese auxiliar. Al eliminar `k`, cada arco `i→j` se actualiza como `Rij + Rik(Rkk)*Rkj`. El orden escogido determina la forma; otro orden puede producir una expresión distinta pero equivalente. El método es el de Hopcroft, Motwani y Ullman (2007), §3.2.2.

## 1.1 Cinco conversiones

### Ejercicio 151

Estado inicial A; finales C, E, F; alfabeto `{0,1}`. Hay ciclos A–C–A con `0`, bucles en B/E y ciclos más largos a través de G, I y J. Las ramas desde A y los estados finales múltiples hacen esperar una unión de caminos. El orden de eliminación empleado fue `B,D,G,H,J,I,C,E,F` (se conserva A como estado inicial).

| Estado | 0 | 1 |
|---|---|---|
| → A | C | E |
| B | F | E |
| * C | A | G |
| D | C | E |
| * E | I | B |
| * F | D | H |
| G | F | I |
| H | C | J |
| I | J | E |
| J | I | E |

**Expresión resultante:**

```text
(00 + (010 + (1 + 011(00)*(1 + 01))(11 + 0(00)*(1 + 01))*10)((00 + 10)10 + (01 + 111 + 110(00)*(1 + 01) + (00 + 10)11(00)*(1 + 01))(11 + 0(00)*(1 + 01))*10)*(00 + 10)0)*(0 + (1 + 011(00)*(1 + 01))(11 + 0(00)*(1 + 01))* + (010 + (1 + 011(00)*(1 + 01))(11 + 0(00)*(1 + 01))*10)((00 + 10)10 + (01 + 111 + 110(00)*(1 + 01) + (00 + 10)11(00)*(1 + 01))(11 + 0(00)*(1 + 01))*10)*(λ + 00 + 10 + (01 + 111 + 110(00)*(1 + 01) + (00 + 10)11(00)*(1 + 01))(11 + 0(00)*(1 + 01))*))
```

### Ejercicio 152

Inicial A; finales D y E. Tiene ciclos A–C–A con `0` y B–D–B / B–E–F–B, y ramas de A hacia E y de C hacia D. Orden: `C,F,B,D,E` (se conserva A como estado inicial).

| Estado | 0 | 1 |
|---|---|---|
| → A | C | E |
| B | D | E |
| C | A | D |
| * D | F | B |
| * E | F | B |
| F | D | B |

**Expresión resultante:**

```text
(00)*(1 + 01)(00 + 010 + 011 + 10 + 11)*
```

### Ejercicio 153

Inicial A; finales D, F y G. D tiene un bucle en `0`, G un bucle en `1`; desde C y E hay ramas hacia F/G. Orden: `B,C,D,E,F,G` (se conserva A como estado inicial).

| Estado | 0 | 1 |
|---|---|---|
| → A | B | C |
| B | D | E |
| C | F | G |
| * D | D | E |
| E | F | G |
| * F | E | D |
| * G | F | G |

**Expresión resultante:**

```text
000* + (10 + (01 + 000*1)0)((0 + 10*1)0)*(λ + 10*) + (11 + (01 + 000*1)1 + (10 + (01 + 000*1)0)((0 + 10*1)0)*(0 + 10*1)1)(1 + 0((0 + 10*1)0)*(0 + 10*1)1)*(λ + 0((0 + 10*1)0)*(λ + 10*))
```

### Ejercicio 154

Inicial A; finales E, G, H y J. El estado J es inaccesible según el análisis de la Lista 3; su presencia en el autómata original no cambia el lenguaje. Ciclos principales: A–D–C, C–I–G, e I–K–F–B–H. Orden: `J,D,F,H,B,C,G,E,I,K` (se conserva A como estado inicial).

| Estado | 0 | 1 |
|---|---|---|
| → A | D | K |
| B | H | K |
| C | I | E |
| D | C | G |
| * E | K | I |
| F | B | E |
| * G | C | I |
| * H | C | B |
| I | G | K |
| * J | F | B |
| K | C | F |

**Expresión resultante:**

```text
001 + 01(λ + 01) + (000 + 0011 + 01(1 + 00 + 011))(0(1 + 00 + 011))*0(λ + 01) + (1 + 0010 + 01010 + (000 + 0011 + 01(1 + 00 + 011))(0(1 + 00 + 011))*(1 + 0010))(10(01)*1 + (11 + (0 + 10(01)*00)1)0 + ((0 + 10(01)*00)0 + (11 + (0 + 10(01)*00)1)1)(0(1 + 00 + 011))*(1 + 0010))*(10(01)*0 + 11 + (0 + 10(01)*00)1 + ((0 + 10(01)*00)0 + (11 + (0 + 10(01)*00)1)1)(0(1 + 00 + 011))*0(λ + 01))
```

### Ejercicio 155

Inicial A; finales G y H. J es inaccesible. Ciclos principales pasan por K–F–I–G y por B–C–F; H se alcanza desde E y también retorna a C/B. Orden: `J,D,H,B,F,I,C,K,E,G` (se conserva A como estado inicial).

| Estado | 0 | 1 |
|---|---|---|
| → A | D | K |
| B | C | G |
| C | F | G |
| D | B | E |
| E | H | K |
| F | I | E |
| * G | C | I |
| * H | C | B |
| I | G | K |
| J | F | B |
| K | C | F |

**Expresión resultante:**

```text
(01 + 00001 + (1 + 000001)(101 + 0001)*(11 + 001))((00 + 010)01 + (1 + (00 + 010)001)(101 + 0001)*(11 + 001))*0 + (001 + 000(1 + 000) + (1 + 000001)(101 + 0001)*(100 + 0(1 + 000)) + (01 + 00001 + (1 + 000001)(101 + 0001)*(11 + 001))((00 + 010)01 + (1 + (00 + 010)001)(101 + 0001)*(11 + 001))*(011 + (00 + 010)(1 + 000) + (1 + (00 + 010)001)(101 + 0001)*(100 + 0(1 + 000))))(10 + 0(1 + 000) + (11 + 0001)(101 + 0001)*(100 + 0(1 + 000)) + (001 + (11 + 0001)(101 + 0001)*(11 + 001))((00 + 010)01 + (1 + (00 + 010)001)(101 + 0001)*(11 + 001))*(011 + (00 + 010)(1 + 000) + (1 + (00 + 010)001)(101 + 0001)*(100 + 0(1 + 000))))*(λ + (001 + (11 + 0001)(101 + 0001)*(11 + 001))((00 + 010)01 + (1 + (00 + 010)001)(101 + 0001)*(11 + 001))*0)
```

### Simplificación algebraica

La expresión 152 muestra una simplificación concreta: D y E son finales con las mismas transiciones, de modo que se agrupan como X; el sistema resultante reduce el resultado a `(00)*(1+01)(00+010+011+10+11)*`. Para las demás expresiones se pueden aplicar las leyes `R + ∅ = R`, `Rλ = λR = R`, `R + R = R`, asociatividad/conmutatividad de `+`, `R* = λ + RR* = λ + R*R` y distributividad. Estas leyes se justifican con la definición de expresiones regulares y operaciones sobre lenguajes (Hopcroft et al., 2007, §3.1). No se fuerza una forma mínima única: no existe una única expresión regular simplificada.

**Archivos de entrada:** `../automatas/afd-a-er/ejercicio_151.jff` a `ejercicio_155.jff`.

## Verificación de cadenas para 1.1

Cada fila contiene dos rechazos y tres aceptaciones. Además, las cinco expresiones se contrastaron por enumeración contra el AFD en todas las cadenas de longitud 0 a 8, sin discrepancias; esto es una comprobación acotada, no sustituye `Compare Equivalence`. Las cadenas de la tabla son el mínimo sugerido para reproducir en JFLAP (`λ` representa la cadena vacía).

| Ejercicio | Cadena: resultado esperado |
|---|---|
| 151 | λ: rechaza; 00: rechaza; 0: acepta; 1: acepta; 000: acepta |
| 152 | λ: rechaza; 0: rechaza; 1: acepta; 01: acepta; 001: acepta |
| 153 | λ: rechaza; 0: rechaza; 00: acepta; 10: acepta; 11: acepta |
| 154 | λ: rechaza; 0: rechaza; 01: acepta; 001: acepta; 101: acepta |
| 155 | λ: rechaza; 0: rechaza; 001: acepta; 010: acepta; 101: acepta |

La construcción por eliminación preserva el lenguaje en cada eliminación, pues `Rij + Rik(Rkk)*Rkj` enumera los caminos que evitan `k` o pasan por `k` una o más veces. Esta es una justificación matemática de equivalencia. Aún falta el `Compare Equivalence` solicitado en JFLAP.

## 1.2 Desarrollo algebraico de los dos casos

Se eligieron 152 y 153. En el método de Arden, `Xq` representa las cadenas que llevan desde `q` a un estado final. A continuación se escriben los sistemas completos; las ecuaciones de los finales incluyen λ.

**Ejercicio 152:**

```text
XA = 0 XC + 1 XE
XB = 0 XD + 1 XE
XC = 0 XA + 1 XD
XD = 0 XF + 1 XB + λ
XE = 0 XF + 1 XB + λ
XF = 0 XD + 1 XB
```

**Ejercicio 153:**

```text
XA = 0 XB + 1 XC
XB = 0 XD + 1 XE
XC = 0 XF + 1 XG
XD = 0 XD + 1 XE + λ
XE = 0 XF + 1 XG
XF = 0 XE + 1 XD + λ
XG = 0 XF + 1 XG + λ
```

Al resolver el sistema de 152, `XD = XE = X` y `XB = (0 + 1)X`; así `XF = (0 + 10 + 11)X` y `X = λ + (00 + 010 + 011 + 10 + 11)X`. Arden da `X = (00 + 010 + 011 + 10 + 11)*`. Luego `XC = 0XA + 1X` y `XA = 0XC + 1X`, por lo que `XA = (00)*(1 + 01)X`. El resultado es la expresión compacta de 152 impresa arriba.

Para 153, defínanse las abreviaturas `P = 0 + 10*1`, `Q = 0 + 11*0` y `T = (PQ)*(λ + 10* + P11*)`. Arden aplicado a las ecuaciones de D, G, E y F da `XD = 0*(λ + 1(11* + QT))`, `XG = 1*(λ + 0T)`, `XE = 11* + QT` y `XF = T`. Por tanto, el resultado desde A es `XA = 00XD + 01XE + 10XF + 11XG`. Estas definiciones forman una expresión regular completa sin desplegar las subexpresiones repetidas.

Se aplica Arden a cada término autorreferente `X = AX + B`, sustituyendo `A*B`, con `λ ∉ L(A)`. La resolución anterior da para 152 la expresión simplificada de 30 caracteres y para 153 la expresión factorizada con P, Q y T (207 caracteres al expandirse).

Los resultados completos de `Rᵢⱼ⁽ᵏ⁾` están en [02-calculos-Rij.md](02-calculos-Rij.md). Para ese método se inicializa `Rᵢⱼ⁽⁰⁾` con las etiquetas directas y λ en la diagonal; en la ronda k se usa `Rᵢⱼ⁽ᵏ⁾ = Rᵢⱼ⁽ᵏ⁻¹⁾ + Rᵢₖ⁽ᵏ⁻¹⁾(Rₖₖ⁽ᵏ⁻¹⁾)*(Rₖⱼ⁽ᵏ⁻¹⁾)`. Tras seis rondas (152) y siete rondas (153), se unen los términos desde A hasta cada estado final. Con orden A,B,…, los resultados sin simplificación tienen 922 caracteres (152) y 906 (153). La diferencia de tamaño muestra el efecto del orden y de la simplificación algebraica. El método es mecánico, pero genera subexpresiones repetidas; Arden permite seguir directamente ecuaciones y eliminación opera directamente sobre el grafo.

| Método | 152: pasos / longitud | 153: pasos / longitud | Facilidad de aplicación |
|---|---:|---:|---|
| Eliminación | 5 / 30 | 6 / 184 | Claro visualmente, tamaño dependiente del orden |
| `Rᵢⱼ⁽ᵏ⁾` | 6 rondas / 922 | 7 rondas / 906 | Sistemático, aunque la matriz genera más escritura |
| Arden | 6 ecuaciones / 30 | 7 ecuaciones / 207 | Natural para ecuaciones; requiere cuidar sustituciones |

En cada método, una simplificación válida usada para normalizar términos es `R + ∅ = R`, `Rλ = λR = R` y `R + R = R`. Por ejemplo, al construir `Rᵢⱼ⁽ᵏ⁾`, si no existe un arco directo (`Rᵢⱼ⁽⁰⁾ = ∅`), la actualización se reduce a la alternativa que pasa por k. Las leyes y la recurrencia se apoyan en Hopcroft et al. (2007), §§3.1–3.2; la referencia de Arden debe alinearse con el apunte del curso, que no venía adjunto.

## Evidencias y resultados de la comprobación

Los archivos originales de JFLAP, las capturas recibidas y los resultados de validación están incluidos en el repositorio.

| Ejercicio | NFA exportado desde la ER | Capturas | Equivalencia exacta |
|---|---|---:|---|
| 151 | `automatas/practica3/afd-a-er/ejercicio_151_er_nfa.jff` | 7 | Sí; 133 pares alcanzables |
| 152 | `automatas/practica3/afd-a-er/ejercicio_152_er_nfa.jff` | 6 | Sí; 31 pares alcanzables |
| 153 | `automatas/practica3/afd-a-er/ejercicio_153_er_nfa.jff` | 8 | Sí; 21 pares alcanzables |
| 154 | `automatas/practica3/afd-a-er/ejercicio_154_er_nfa.jff` | 10 | Sí; 87 pares alcanzables |
| 155 | `automatas/practica3/afd-a-er/ejercicio_155_er_nfa.jff` | 12 | Sí; 61 pares alcanzables |

Las capturas de equivalencia de JFLAP para los ejercicios 152–155 muestran **“They ARE equivalent!”**. La equivalencia de los cinco pares AFD/NFA se verificó además exactamente recorriendo el producto del AFD con los subconjuntos alcanzables del NFA: en ningún par alcanzable difiere la condición de aceptación. El resumen está en `evidencias/practica3/afd-a-er/equivalencia_exacta.csv`.

Las cinco cadenas por ejercicio se corrieron sobre el AFD original y el NFA exportado desde la expresión. Los 25 resultados coinciden; la tabla completa está en `evidencias/practica3/afd-a-er/cadenas.csv`. Para `λ` se evaluó la cadena vacía.

Las capturas de eliminación y expresión se organizaron por ejercicio en `evidencias/practica3/afd-a-er/ejercicio_151/` a `ejercicio_155/`. Los NFA exportados se conservaron junto a sus AFD de entrada en `automatas/practica3/afd-a-er/`.

## Fuentes

- Hopcroft, J. E., Motwani, R., y Ullman, J. D. (2007). *Introduction to Automata Theory, Languages, and Computation* (3.ª ed.), §§3.1–3.2. Addison-Wesley. ISBN 978-0-321-45536-9.
- Sipser, M. (2013). *Introduction to the Theory of Computation* (3.ª ed.), §1.3. Cengage Learning. ISBN 978-1-133-18779-0.

