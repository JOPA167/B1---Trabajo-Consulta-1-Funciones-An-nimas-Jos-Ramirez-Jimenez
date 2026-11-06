# B1 – Trabajo de Consulta 1: Funciones Anónimas

**Autor:** José Pablo Ramírez Jiménez

---

## Parte 1 – Explorar: la programación funcional en el mundo real

### WhatsApp y Erlang (programación funcional)

El problema de WhatsApp era atender a cientos de millones de usuarios conectados al mismo tiempo, con un equipo muy pequeño y sin que el servicio se cayera. Con el modelo tradicional de hilos, cada conexión consume mucha memoria y las partes del programa comparten datos, lo que obliga a usar candados y produce errores difíciles de rastrear.

Lo resolvieron usando Erlang, un lenguaje de programación funcional creado para telecomunicaciones y diseñado para manejar millones de conexiones simultáneas con mucha fiabilidad. El toque funcional está en cómo se programa: los datos son **inmutables**, así que ningún proceso puede modificar los datos de otro. En lugar de ciclos con variables que cambian, se usa **recursión**: el proceso se vuelve a llamar con su nuevo estado. Los procesos no comparten memoria y solo se comunican enviándose mensajes, y las funciones se pueden pasar como valores.

### Netflix y la programación reactiva

El problema de Netflix era que la app necesitaba datos de muchos servicios a la vez, y pedirlos uno por uno era lento, mientras que hacerlo en paralelo con hilos y callbacks traía errores de sincronización y código enredado. En 2012 su arquitectura tenía dificultades para adaptarse a la cantidad de usuarios, y decidieron rediseñarla para reducir las llamadas REST.

La solución fue adoptar la programación funcional reactiva con RxJava, donde los datos se tratan como flujos sobre los que se encadenan operaciones, y cada operador recibe una **función anónima (lambda)** que indica qué hacer con cada dato. Según uno de sus ingenieros, este enfoque permitió aprovechar la concurrencia del servidor sin los problemas típicos de seguridad entre hilos y de sincronización.

```scala
// Ejemplo ilustrativo (no es código real de Netflix)
usuarioService.obtenerPerfil(id)
  .flatMap(perfil => recomendaciones.para(perfil))   // pide recomendaciones
  .filter(r => r.estaDisponible)                     // descarta lo no disponible
  .map(r => r.titulo)                                // se queda con el título
  .subscribe(titulo => mostrar(titulo))              // qué hacer al llegar cada uno
```

El ejemplo es descriptivo: indica qué función cumple cada línea. La primera obtiene el perfil del usuario y, a partir de él, pide recomendaciones; luego se filtra el contenido disponible, se extrae el título y, por último, `subscribe` define qué hacer cuando llega cada resultado (mostrarlo). Así los pasos se encadenan sin hilos ni callbacks anidados, lo que permite aprovechar la concurrencia sin código enredado.

---

## Parte 2 – Entender: ¿qué es una función anónima?

Una función anónima es una función sin nombre y su peculiaridad radica en que se escribe en el mismo lugar donde se la va a usar y se ejecuta allí mismo. Esto permite ejecutarla dentro de otra función. También pueden recibir el nombre de funciones lambda.

### ¿En qué se diferencia de una función nombrada? Escribe la misma lógica de ambas formas

Una función nombrada debe ser declarada previamente, para luego hacer el respectivo llamado donde la necesitemos; si no respetamos ese orden, rompemos la lógica del programa. En el caso de las funciones anónimas, describimos su funcionamiento en el lugar donde las necesitamos y en el mismo lugar serán ejecutadas. El factor que se considera a la hora de usarlas es la complejidad de la función o la lógica del programa que estamos codificando.

```scala
// Función nombrada
def cuadrado(x: Int): Int = x * x

println(List(1, 2, 3, 4).map(cuadrado))       // List(1, 4, 9, 16)

// Función anónima (misma lógica)
println(List(1, 2, 3, 4).map(x => x * x))     // List(1, 4, 9, 16)
```

### ¿Qué significa que las funciones sean ciudadanos de primera clase? ¿Por qué eso permite pasar una lambda a `map` o `filter`?

Cuando usamos el término de ciudadano de primera clase estamos haciendo referencia a que las funciones se manejan de igual manera que otro valor (número, texto): podemos guardarlas en variables, pasarlas como argumentos, retornarlas desde una función o guardarlas en estructuras de datos.

Por eso, cuando trabajamos con `map` o `filter`, que son funciones de orden superior, podemos pasarles funciones: las funciones de orden superior reciben como argumento otra función.

### ¿Qué es un closure?

Un closure es una función que conserva acceso a las variables del ámbito donde fue definida, incluso después de que ese ámbito haya terminado de ejecutarse.

```scala
def crearMultiplicador(n: Int): Int => Int =
  x => x * n                    // n viene de afuera de la lambda

val doble = crearMultiplicador(2)
println(doble(5))               // 10
```

---

## Parte 3 – Aplicar: ¿en qué problemas sirven las funciones anónimas?

### 3.1 Catálogo de tipos de problemas (4/7)

#### Datos de partida (notas sobre 10)

Se considera aprobado con 7 o más. Todos los ejemplos usan estas mismas notas.

**Scala:** [scala/scala.md](scala/scala.md#datos-de-partida)

**Java:** [java/java.md](java/java.md#datos-de-partida)

**Python:** [python/python.md](python/python.md#datos-de-partida)

---

#### 1. Selección: ¿qué elementos cumplen una condición? (`filter`)

**Situación real:** la secretaría académica necesita obtener las notas reprobadas (menores a 7) para identificar a los estudiantes que deben rendir el examen supletorio.

**Scala:** [scala/scala.md](scala/scala.md#filter)

**Java (Streams):** [java/java.md](java/java.md#filter)

**Python:** [python/python.md](python/python.md#filter)

La función anónima responde "sí o no" para cada nota, y `filter` se queda solo con las que dicen "sí". La lista original no se modifica: se obtiene una nueva.

---

#### 2. Transformación: ¿cómo convierto cada elemento en otro valor? (`map`)

**Situación real:** el sistema de la universidad guarda las notas sobre 10, pero el reporte para el ministerio las pide como porcentaje sobre 100.

**Scala:** [scala/scala.md](scala/scala.md#map)

**Java (Streams):** [java/java.md](java/java.md#map)

**Python:** [python/python.md](python/python.md#map)

`map` devuelve una lista con el mismo número de elementos, cada uno transformado. La lista original de notas no cambia.

---

#### 3. Validación: ¿existe alguno que cumpla? ¿todos cumplen? (`exists`, `forall`)

**Situación real:** un docente quiere saber si **algún** estudiante obtuvo la nota máxima (10), para proponerlo a un reconocimiento, y si **todos** aprobaron el curso, para decidir si hace falta una clase de refuerzo.

**Scala:** [exists](scala/scala.md#exists) y [forall](scala/scala.md#forall)

**Java (Streams):** [anyMatch](java/java.md#anymatch) y [allMatch](java/java.md#allmatch) (`anyMatch` equivale a `exists`, `allMatch` a `forall`)

**Python:** [any](python/python.md#any) y [all](python/python.md#all) (`any` equivale a `exists`, `all` a `forall`)

Estas operaciones devuelven un valor booleano, no una lista. Además se detienen apenas conocen la respuesta: `exists` para en la primera nota que cumple y `forall` en la primera que no cumple.

---

#### 4. Conteo: ¿cuántos elementos cumplen una condición? (`count`)

**Situación real:** la universidad quiere saber cuántos estudiantes aprobaron para calcular el porcentaje de aprobación del curso.

**Scala:** [scala/scala.md](scala/scala.md#count)

**Java (Streams):** [java/java.md](java/java.md#count)

**Python:** [python/python.md](python/python.md#count-len--filter)

En Scala, `count` recibe directamente la condición. En Java y Python se combina `filter` con el conteo, porque `count()` de Streams y `len()` no reciben condición.

---

## Parte 4 – Operaciones adicionales de Scala

### 4.1 Elige tus operaciones

**Lista de partida (notas sobre 10):** [operaciones/operaciones.md](operaciones/operaciones.md#datos-de-partida)

#### 1. `sortBy`

**1. Qué hace:** ordena la lista de menor a mayor según un criterio que se le indica. Se le dice por qué valor ordenar y Scala compara esos valores.

**2. Lambda:** recibe **un elemento** y devuelve **la clave de ordenamiento** (un `Double`, un `String`, etc.).

**3. Ejemplo:** [operaciones/operaciones.md](operaciones/operaciones.md#sortby)

El signo menos invierte el orden, porque ordena de menor a mayor la nota negativa.

**4. Problema que resuelve:** armar el ranking de notas del curso. Es un **tipo nuevo** de problema (ordenamiento): no filtra, no transforma, no valida ni cuenta, solo reordena.

---

#### 2. `sortWith`

**1. Qué hace:** también ordena la lista, pero quien programa define la regla de comparación completa entre dos elementos.

**2. Lambda:** recibe **dos elementos** `(a, b)` y devuelve **`Boolean`**: `true` si `a` debe ir antes que `b`.

**3. Ejemplo:** [operaciones/operaciones.md](operaciones/operaciones.md#sortwith)

**4. Problema que resuelve:** ordenar de mayor a menor las notas para publicar el cuadro de honor. Es el mismo **tipo nuevo** (ordenamiento). Es más flexible que `sortBy` cuando la regla de orden es más compleja, por ejemplo, desempatar por varios criterios.

---

#### 3. `indexWhere`

**1. Qué hace:** busca el **primer** elemento que cumple una condición y devuelve su **posición** (empezando en 0). Si ninguno cumple, devuelve `-1`.

**2. Lambda:** recibe **un elemento** y devuelve **`Boolean`**.

**3. Ejemplo:** [operaciones/operaciones.md](operaciones/operaciones.md#indexwhere)

**4. Problema que resuelve:** ubicar al primer estudiante reprobado de la lista para revisar su caso; con esa posición se puede obtener su nombre de otra lista. Se parece a la **selección** (`filter`), pero devuelve solo la posición del primero y se detiene al encontrarlo. También se parece a la **validación** (`exists`), con la diferencia de que `exists` solo responde sí o no, e `indexWhere` dice *dónde* está.

---

#### 4. `grouped`

**1. Qué hace:** divide la lista en bloques **separados** de un tamaño dado. El último bloque puede quedar más corto.

**2. Lambda:** **no recibe ninguna**; solo corta la lista en bloques. Se encadena con una operación que sí la reciba, como `map`, cuya lambda recibe **un bloque** y devuelve **un valor** para ese bloque. Devuelve un `Iterator`, por eso se agrega `.toList` para ver el resultado.

**3. Ejemplo:** [operaciones/operaciones.md](operaciones/operaciones.md#grouped)

**4. Problema que resuelve:** calcular el promedio de las notas por grupos de evaluaciones. Combinado con `map`, se parece a la **transformación**; lo nuevo es el **recorrido por bloques**, que no existe en los tipos de problema básicos (selección, transformación, validación y conteo).

---

### Pregunta de varios pasos

**¿Si el reporte del ministerio pide las notas como porcentaje y ordenadas de mayor a menor, en qué posición del ranking aparece el primer estudiante que queda por debajo del 80 %?**

**Solución:** [operaciones/operaciones.md](operaciones/operaciones.md#combinación)

**Pasos:**

1. `sortBy`: ordena las notas de mayor a menor.
2. `map`: convierte cada nota a porcentaje.
3. `indexWhere`: devuelve la posición de la primera que está bajo 80 % (se cuenta desde 0).

**Conclusión:** el primer estudiante bajo el 80 % está en la posición 3, es decir, en el cuarto lugar del ranking.

---

## Parte 5 – Comparar y concluir

### 5.1 Scala, Java y Python frente a frente

| Criterio | Scala | Java | Python |
|---|---|---|---|
| **Sintaxis de la lambda** | `n => n < 7` (también `_ < 7`) | `n -> n < 7` | `lambda n: n < 7` |
| **Cómo se declara el tipo** | El tipo se infiere: `val notas = List(8.5, 9.0)`. Se puede indicar como `List[Double]`, y en la lambda es opcional: `(n: Double) => n < 7` | Se declara de forma explícita en la variable: `List<Double> notas`. En la lambda se infiere del contexto | No se declara (tipado dinámico): `notas = [8.5, 9.0]`. Los tipos son opcionales como anotaciones |
| **Operaciones con listas disponibles** | Son métodos de la propia lista: `filter`, `map`, `exists`, `forall`, `count`, `sortBy`, `sortWith`, `indexWhere`, `grouped` | Se usan sobre un `Stream`: `filter`, `map`, `anyMatch`, `allMatch`, `count`, `sorted`. Hay que llamar a `.stream()` al inicio y a `.toList()` al final. No tiene `indexWhere` ni `grouped` directos | Son funciones externas: `filter`, `map`, `any`, `all`, `sorted`. Devuelven iteradores, por lo que se envuelven en `list(...)`. No tiene `count` con condición, `indexWhere` ni `grouped` directos |
| **Restricción del lenguaje** | Ninguna importante para las lambdas: pueden tener varias líneas usando llaves `{ }` | Las variables externas que usa la lambda deben ser `final` o efectivamente `final` (no se pueden reasignar) | La `lambda` solo admite **una expresión**: no puede tener varias instrucciones ni docstring |
| **Legibilidad (tu opinión)** | Es la más corta y fluida: las operaciones se encadenan directamente sobre la lista | Es la más extensa: necesita `.stream()` y `.toList()` | Las lambdas son fáciles de leer en casos simples, pero anidar `list(filter(...))` o `list(map(...))` complica la lectura |

### Reflexión

**¿En qué lenguaje fue más natural resolver los problemas?**

El lenguaje que se me hizo más natural fue **Scala**, ya que al usar las funciones lambda se me facilita la lectura gracias a su signo `=>`. Además, si el problema necesita varias funciones, se van encadenando con un punto, y muchas de estas funciones tienen procesos que evitan pasos extras para seguir trabajando en la misma línea de código.

**¿En qué tipo de problema encontraste más diferencias entre lenguajes y por qué?**

Donde encontré más diferencias fue en las funciones `foreach` y `forall`, especialmente con Java, ya que tiene equivalentes no tan intuitivos y el uso de pasos intermedios lo hace más largo. Esto se puede ver en el ejemplo de [Validación (Parte 3.1, problema 3)](#3-validación-existe-alguno-que-cumpla-todos-cumplen-exists-forall). También encontré que las equivalencias son diferentes entre Scala y Python.
