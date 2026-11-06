# Parte 3 - Soluciones en Python

## Datos de partida

```python
notas = [8.5, 9.0, 6.0, 7.5, 10.0]
```

## filter

```python
reprobadas = list(filter(lambda n: n < 7, notas))
```

## map

```python
porcentajes = list(map(lambda n: n * 10, notas))
```

## any

```python
hay_excelencia = any(map(lambda n: n == 10, notas))
```

## all

```python
todos_aprobaron = all(map(lambda n: n >= 7, notas))
```

## count (len + filter)

```python
aprobados = len(list(filter(lambda n: n >= 7, notas)))
```

## Captura de ejecución

![Ejecución en Python](../img/python.png)
