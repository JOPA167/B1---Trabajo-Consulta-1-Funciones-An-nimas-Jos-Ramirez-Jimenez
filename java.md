# Parte 3 - Soluciones en Java

## Datos de partida

```java
List<Double> notas = List.of(8.5, 9.0, 6.0, 7.5, 10.0);
```

## filter

```java
List<Double> reprobadas = notas.stream()
        .filter(n -> n < 7)
        .toList();
```

## map

```java
List<Double> porcentajes = notas.stream()
        .map(n -> n * 10)
        .toList();
```

## anyMatch

```java
boolean hayExcelencia = notas.stream().anyMatch(n -> n == 10);
```

## allMatch

```java
boolean todosAprobaron = notas.stream().allMatch(n -> n >= 7);
```

## count

```java
long aprobados = notas.stream()
        .filter(n -> n >= 7)
        .count();
```

## Captura de ejecución

![Ejecución en Java](../img/java.png)
