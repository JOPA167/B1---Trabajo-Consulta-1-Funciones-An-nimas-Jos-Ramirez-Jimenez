# Parte 4 - Operaciones adicionales de Scala

## Datos de partida

```scala
val notas = List(8.5, 9.0, 6.0, 7.5, 10.0)
```

## sortBy

```scala
notas.sortBy(n => n)
notas.sortBy(n => -n)
```

## sortWith

```scala
notas.sortWith((a, b) => a > b)
```

## indexWhere

```scala
notas.indexWhere(n => n < 7)
notas.indexWhere(n => n > 10)
```

## grouped

```scala
notas.grouped(2).toList
```

## Combinación

**¿Si el reporte del ministerio pide las notas como porcentaje y ordenadas de mayor a menor, en qué posición del ranking aparece el primer estudiante que queda por debajo del 80 %?**

```scala
notas
  .sortBy(n => -n)
  .map(n => n * 10)
  .indexWhere(p => p < 80)
```

## Captura de ejecución

![Ejecución de operaciones en Scala](../img/operaciones.png)
