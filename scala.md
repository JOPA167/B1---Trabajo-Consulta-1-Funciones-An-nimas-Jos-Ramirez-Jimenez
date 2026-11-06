# Parte 3 - Soluciones en Scala

## Datos de partida

```scala
val notas = List(8.5, 9.0, 6.0, 7.5, 10.0)
```

## filter

```scala
val reprobadas = notas.filter(n => n < 7)
```

## map

```scala
val porcentajes = notas.map(n => n * 10)
```

## exists

```scala
val hayExcelencia = notas.exists(n => n == 10)
```

## forall

```scala
val todosAprobaron = notas.forall(n => n >= 7)
```

## count

```scala
val aprobados = notas.count(n => n >= 7)
```

## Captura de ejecución

![Ejecución en Scala](../img/scala.png)
