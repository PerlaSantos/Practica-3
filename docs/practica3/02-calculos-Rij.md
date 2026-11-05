# Cálculos 1.2: recurrencia de expresiones Rᵢⱼ⁽ᵏ⁾

Este apéndice deja explícitos los resultados de la recurrencia después de procesar los seis estados del ejercicio 152 y los siete del 153, en orden alfabético. La expresión final es la unión de las entradas desde el estado inicial A hasta cada estado final. `λ` denota la cadena vacía; `+`, unión.

## Ejercicio 152

Estados finales: D y E. Rondas: C₁,…,C₆ (se toma el orden A,B,C,D,E,F).

```text
0(λ + 00)*1 + 0(λ + 00)*1(λ + 10)*(λ + 10) + (1 + 0(λ + 00)*01 + 0(λ + 00)*1(λ + 10)*11)(λ + 11 + 10(λ + 10)*11)*(10 + 10(λ + 10)*(λ + 10)) + (0(λ + 00)*1(λ + 10)*0 + (1 + 0(λ + 00)*01 + 0(λ + 00)*1(λ + 10)*11)(λ + 11 + 10(λ + 10)*11)*(0 + 10(λ + 10)*0))(λ + (0 + 10)(λ + 10)*0 + (11 + (0 + 10)(λ + 10)*11)(λ + 11 + 10(λ + 10)*11)*(0 + 10(λ + 10)*0))*(0 + 10 + (0 + 10)(λ + 10)*(λ + 10) + (11 + (0 + 10)(λ + 10)*11)(λ + 11 + 10(λ + 10)*11)*(10 + 10(λ + 10)*(λ + 10))) + 1 + 0(λ + 00)*01 + 0(λ + 00)*1(λ + 10)*11 + (1 + 0(λ + 00)*01 + 0(λ + 00)*1(λ + 10)*11)(λ + 11 + 10(λ + 10)*11)*(λ + 11 + 10(λ + 10)*11) + (0(λ + 00)*1(λ + 10)*0 + (1 + 0(λ + 00)*01 + 0(λ + 00)*1(λ + 10)*11)(λ + 11 + 10(λ + 10)*11)*(0 + 10(λ + 10)*0))(λ + (0 + 10)(λ + 10)*0 + (11 + (0 + 10)(λ + 10)*11)(λ + 11 + 10(λ + 10)*11)*(0 + 10(λ + 10)*0))*(11 + (0 + 10)(λ + 10)*11 + (11 + (0 + 10)(λ + 10)*11)(λ + 11 + 10(λ + 10)*11)*(λ + 11 + 10(λ + 10)*11))
```

Longitud sin espacios: 922 caracteres. Esta salida no está simplificada; expresiones equivalentes más cortas se obtienen con leyes de regularidad.

## Ejercicio 153

Estados finales: D, F y G. Rondas: C₁,…,C₇ (orden A,B,C,D,E,F,G).

```text
00 + 00(λ + 0)*(λ + 0) + (10 + (01 + 00(λ + 0)*1)0)(λ + (0 + 1(λ + 0)*1)0)*(1 + 1(λ + 0)*(λ + 0)) + (11 + (01 + 00(λ + 0)*1)1 + (10 + (01 + 00(λ + 0)*1)0)(λ + (0 + 1(λ + 0)*1)0)*(0 + 1(λ + 0)*1)1)(λ + 1 + 0(λ + (0 + 1(λ + 0)*1)0)*(0 + 1(λ + 0)*1)1)*0(λ + (0 + 1(λ + 0)*1)0)*(1 + 1(λ + 0)*(λ + 0)) + 10 + (01 + 00(λ + 0)*1)0 + (10 + (01 + 00(λ + 0)*1)0)(λ + (0 + 1(λ + 0)*1)0)*(λ + (0 + 1(λ + 0)*1)0) + (11 + (01 + 00(λ + 0)*1)1 + (10 + (01 + 00(λ + 0)*1)0)(λ + (0 + 1(λ + 0)*1)0)*(0 + 1(λ + 0)*1)1)(λ + 1 + 0(λ + (0 + 1(λ + 0)*1)0)*(0 + 1(λ + 0)*1)1)*(0 + 0(λ + (0 + 1(λ + 0)*1)0)*(λ + (0 + 1(λ + 0)*1)0)) + 11 + (01 + 00(λ + 0)*1)1 + (10 + (01 + 00(λ + 0)*1)0)(λ + (0 + 1(λ + 0)*1)0)*(0 + 1(λ + 0)*1)1 + (11 + (01 + 00(λ + 0)*1)1 + (10 + (01 + 00(λ + 0)*1)0)(λ + (0 + 1(λ + 0)*1)0)*(0 + 1(λ + 0)*1)1)(λ + 1 + 0(λ + (0 + 1(λ + 0)*1)0)*(0 + 1(λ + 0)*1)1)*(λ + 1 + 0(λ + (0 + 1(λ + 0)*1)0)*(0 + 1(λ + 0)*1)1)
```

Longitud sin espacios: 906 caracteres. El tamaño mayor que las expresiones de eliminación/Arden del reporte ilustra cuánto influye incluir λ en la diagonal y expandir toda la matriz sin simplificar.
