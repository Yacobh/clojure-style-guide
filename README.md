# Guía de estilo para Clojure

> Los modelos a seguir son importantes. <br/> 
> -- Oficial Alex J. Murphy / RoboCop

Esta guía de estilo para Clojure recomienda mejores practicas con el
fin de que programadores de Clojure puedan desarrollar código que
pueda ser mantenido por otros programadores del lenguaje. Una guía de
estilo que refleja correctamente usos reales del lenguaje será usada.
Mientras que una guía que trate de imponer un estilo rechazado por
aquellos individuos a quién la guía trata de ayudar, está destinada a
que nunca se use sin importar que tan buena sea.

Esta guía está separada en varias secciones de reglas relacionadas. He
intentado proveer razones fundamentales tras las reglas aquí
expuestas. Si es el caso de que la razón es omitida, es porque la he
considerado evidente.

Cabe decir que estas reglas no salen de la nada. Las reglas son, en su
gran mayoría basadas en mi extensiva carrera como ingeniero de
software profesional, comentarios y sugerencias de miembros de la
comunidad de Clojure, y varios prestigiosos recursos de programación
de Clojure tales como ["Clojure Programming"](http://www.clojurebook.com/) 
y ["The Joy of Clojure"](http://joyofclojure.com/).

La guía es todavía un trabajo en progreso ya que algunas secciones aún
faltan y otras están incompletas. Existen también unas reglas que se
beneficiarían de mejores ejemplos. A su debido tiempo, estos y otros
problemas serán solucionados mas por ahora, vale la pena tenerlos en
mente.

Ten en cuenta que la comunidad de desarrollo de Clojure también
mantiene una lista de [estándares de código para librerías](http://dev.clojure.org/display/community/Library+Coding+Standards).

Tú tienes la opción de generar un archivo PDF o una copia HTML de esta
guía usando [Pandoc](http://pandoc.org/).

Traducciones de esta guía están disponibles en los siguientes idiomas:

* [Chino](https://github.com/geekerzp/clojure-style-guide/blob/master/README-zhCN.md)
* [Coreano](https://github.com/kwakbab/clojure-style-guide/blob/master/README-koKO.md)
* [Inglés](https://github.com/bbatsov/clojure-style-guide/blob/master/README.md)
* [Japonés](https://github.com/totakke/clojure-style-guide/blob/ja/README.md)
* [Portugués](https://github.com/theSkilled/clojure-style-guide/blob/pt-BR/README.md) (trabajo en progreso)

## Tabla de Contenido

* [Diseño y organización de código
  fuente](#diseño-y-organización-de-código-fuente)
* [Sintaxis](#sintaxis)
* [Nombramiento](#nombramiento)
* [Colecciones](#colecciones)
* [Mutación](#mutación)
* [Strings o cuerdas](#strings-o-cuerdas)
* [Excepciones](#excepciones)
* [Macros](#macros)
* [Comentarios](#comentarios)
    * [Anotaciones de comentarios](#anotaciones-de-comentarios)
* [Existencial](#existencial)
* [Herramientas](#herramientas)
* [Pruebas](#pruebas)
* [Organización de librerías](#organización-de-librerías)

## Diseño y organización de código fuente

> Casi todos están convencido que todos los estilos, salvo el suyo, son
> feos e ilegibles. Elimina "salvo el suyo" y probablemente estén en lo
> correcto... <br/> -- Jerry Coffin (acerca de indentación/sangría)

* <a name="spaces"></a> Para indentación/sangría usa **espacios** y no
  tabuladores. <sup>[[link](#spaces)]</sup>

* <a name="body-indentation"></a> Usa 2 espacios de sangría en los
cuerpos de formas con argumentos. Esto incluye todos las formas `def`,
formas especiales y macros que incluyen enlaces locales como `loop`,
`let`, `when`, `cond`, `as->`, `cond->`, `case`, `with-*`, etc.
<sup>[[link](#body-indentation)]</sup>

    ```Clojure
    ;; bueno
    (when algo
      (algo-mas))

    (with-out-str
      (println "Hola ")
      (println "mundo!"))

    ;; malo - cuatro espacios
    (when algo
        (algo-mas))

    ;; malo - un solo espacio
    (with-out-str
     (println "Hola")
     (println "mundo!"))
    ```

* <a name="vertically-align-fn-args"></a> Alinea verticalmente
 argumentos de funciones o macros que abarcan múltiples líneas.
 <sup>[[link](#vertically-align-fn-args)]</sup>

    ```Clojure
    ;; bueno
    (filter even?
            (range 1 10))

    ;; malo
    (filter even?
      (range 1 10))
    ```

* <a name="one-space-indent"></a> Usa un solo espacio de sangría para
  argumentos de funciones o macros cuando no aparezcan en la misma
  línea que el nombre de la función.
  <sup>[[link](#one-space-indent)]</sup>

    ```Clojure
    ;; bueno
    (filter
     even?
     (range 1 10))

    (or
     uno
     dos
     tres)

    ;; malo - sangría de dos espacios
    (filter
      even?
      (range 1 10))

    (or
      uno
      dos
      tres)
    ```

* <a name="vertically-align-let-and-map"></a> Alinea verticalmente
  enlaces `let` y palabras clave "keywords", en mapas.
  <sup>[[link](#vertically-align-let-and-map)]</sup>

    ```Clojure
    ;; bueno
    (let [cosa1 "algo"
          cosa2 "algo más"]
      {:cosa1 cosa1
       :cosa2 cosa2})

    ;; malo
    (let [cosa1 "algo"
      cosa2 "algo más"]
      {:cosa1 cosa1
      :cosa2 cosa2})
    ```

* <a name="optional-new-line-after-fn-name"></a> La linea entre el
  nombre de una función y su vector de argumentos es opcional y puede
  ser excluida cuando no conlleve documentación.
  <sup>[[link](#optional-new-line-after-fn-name)]</sup>

    ```Clojure
    ;; bueno
    (defn foo
      [x]
      (bar x))

    ;; bueno
    (defn foo [x]
      (bar x))

    ;; malo
    (defn foo
      [x] (bar x))
    ```

* <a name="multimethod-dispatch-val-placement"></a> Incluye el
  `dispatch-val` o valor de envío de un multimétodo en la misma linea
  que el nombre de la función.
  <sup>[[link](#multimethod-dispatch-val-placement)]</sup>

    ```Clojure
    ;; bueno
    (defmethod foo :bar [x] (baz x))

    (defmethod foo :bar
      [x]
      (baz x))

    ;; malo
    (defmethod foo
      :bar
      [x]
      (baz x))

    (defmethod foo
      :bar [x]
      (baz x))
    ```

* <a name="docstring-after-fn-name"></a> Al agregar documentación, en
  especial a una función con la forma del ejemplo anterior, asegúrate
  que la documentación aparezca inmediatamente después del vector de
  argumentos. De otra forma, éste no será parte de la
  documentación.
  <sup>[[link](#docstring-after-fn-name)]</sup>

    ```Clojure
    ;; bueno
    (defn foo
      "cadena de documentación"
      [x]
      (bar x))

    ;; malo
    (defn foo [x]
      "cadena de documentación"
      (bar x))
    ```

 * <a name="oneline-short-fn"></a> La línea entre el vector de argumento
  de una función y su cuerpo es opcional si el cuerpo es pequeño.
  <sup>[[link](#oneline-short-fn)]</sup>

    ```Clojure
    ;; bueno
    (defn foo [x]
      (bar x))

    ;; bueno para una función pequeña
    (defn foo [x] (bar x))

    ;; bueno para funciones de varias aridades
    (defn foo
      ([x] (bar x))
      ([x y]
       (if (predicate? x)
         (bar x)
         (baz x))))

    ;; malo
    (defn foo
      [x] (if (predicate? x)
            (bar x)
            (baz x)))
    ```

* <a name="multiple-arity-indentation"></a> Agrega sangría para cada
  aridad de una función. De igual forma, alinea verticalmente sus argumentos.
  <sup>[[link](#multiple-arity-indentation)]</sup>

  
    ```Clojure
    ;; bueno
    (defn foo
      "Yo tengo dos aridades."
      ([x]
       (foo x 1))
      ([x y]
       (+ x y)))

    ;; malo - sangría innecesaria
    (defn foo
      "I have two arities."
      ([x]
        (foo x 1))
      ([x y]
        (+ x y)))
    ```

* <a name="multiple-arity-order"></a> Ordena las aridades de una
  función de mínimo a máximo número de argumentos. Lo que ocurre
  frecuentemente en funciones con distintas aridades es que existe un
  número K de argumentos los cuales completamente especifican el
  comportamiento de la función. Para el caso de aridades N < K, la
  función es parcialmente aplicada y para aridades N > K, la función
  provee un fold sobre la versión con K aridades sobre varargs, o
  argumentos variables. <sup>[[link](#multiple-arity-order)]</sup>
  
    ```Clojure
    ;; bueno - fácilmente encontramos la aridad que necesitamos
    (defn foo
      "Yo tengo dos aridades."
      ([x]
       (foo x 1))
      ([x y]
       (+ x y)))

    ;; regular - todas las aridades están basadas en la función con aridad 2
    (defn foo
      "Yo tengo dos aridades."
      ([x y]
       (+ x y))
      ([x]
       (foo x 1))
      ([x y z & mas]
       (reduce foo (foo x (foo y z)) mas)))

    ;; malo - en desorden
    (defn foo
      ([x] 1)
      ([x y z] (foo x (foo y z)))
      ([x y] (+ x y))
      ([w x y z & mas] (reduce foo (foo w (foo x (foo y z))) mas)))
    ```
  
* <a name="align-docstring-lines"></a> Agrega sangría en cada línea
  de documentación si ésta abarca más de una sola línea
  <sup>[[link](#align-docstring-lines)]</sup>

    ```Clojure
    ;; bueno
    (defn foo
      "Hola. Esto es una documentación
      que abarca varias líneas."
      []
      (bar))

    ;; malo
    (defn foo
      "Hola. Esto es una documentación
    que abarca varias líneas."
      []
      (bar))
    ```

* <a name="crlf"></a> Usa las terminaciones de línea de estilo Unix.
  (Usuarios de BDS, Solaris, Gnu Linux, y OSX ya las usan por defecto.
  Usuarios de Windows tendrán que ser más cuidadosos.)
  <sup>[[link](#crlf)]</sup>

    * Si estas usando Git, es posible que quieras agregar el siguiente
      cambio de configuración para ayudar a prevenir que terminaciones
      de línea de estilo Windows se introduzcan al proyecto:
      
    ```
    bash$ git config --global core.autocrlf true
    ```

* <a name="bracket-spacing"></a>

  Si existe texto inmediatamente antes o después de una sección entre
  paréntesis `(...)`, corchetes `[...]` o llaves `{...}`, agrega un
  espacio antes de la apertura y después de la clausura del mismo.
  <sup>[[link](#bracket-spacing)]</sup>

    ```Clojure
    ;; bueno
    (foo (bar baz) quux)

    ;; malo
    (foo(bar baz)quux)
    (foo ( bar baz ) quux)
    ```

* <a name="no-commas-for-seq-literals"></a> No agregues comas `,` entre
  elementos de una secuencia literal.
  <sup>[[link](#no-commas-for-seq-literals)]</sup>

    ```Clojure
    ;; bueno
    [1 2 3]
    (1 2 3)

    ;; malo
    [1, 2, 3]
    (1, 2, 3)
    ```

* <a name="opt-commas-in-map-literals"></a> Considera mejorar la
  legibilidad de mapas literales a través del uso prudente de comas y
  líneas. <sup>[[link](#opt-commas-in-map-literals)]</sup>

    ```Clojure
    ;; bueno
    {:nombre "Bruce Wayne" :identidad-secreta "Batman"}

    ;; bueno y discutiblemente más legible
    {:nombre "Bruce Wayne"
     :identidad-secreta "Batman"}

    ;; bueno y discutiblemente más compacto
    {:nombre "Bruce Wayne", :identidad-secreta "Batman"}
    ```

* <a name="gather-trailing-parens"></a> Agrega todos los paréntesis
  que cierran una expresión en una sola línea.
  <sup>[[link](#gather-trailing-parens)]</sup>

    ```Clojure
    ;; bueno
    (when algo
      (algo-mas))

    ;; malo. Los paréntesis están en distintas líneas
    (when algo
      (algo-mas)
    )
    ```

* <a name="empty-lines-between-top-level-forms"></a> Usa líneas vacías
  entre formas de nivel superior.
  <sup>[[link](#empty-lines-between-top-level-forms)]</sup>

    ```Clojure
    ;; bueno
    (def x ...)

    (defn foo ...)

    ;; malo
    (def x ...)
    (defn foo ...)
    ```

    Una excepción de esta regla es durante el agrupamiento de
    definiciones relacionadas tipo `def`.

    ```Clojure
    ;; bueno
    (def min-filas 10)
    (def max-filas 20)
    (def min-columnas 15)
    (def max-columnas 30)
    ```

* <a name="no-blank-lines-within-def-forms"></a> No agregues líneas
  vacías en medio de la definición de una función o un macro. Una
  excepción a esta regla es cuando se desea indicar grupos de
  construcciones en pares como aquellas encontradas en `let` y `cond`.
  <sup>[[link](#no-blank-lines-within-def-forms)]</sup>

* <a name="80-character-limits"></a> Donde sea factible, evita tener
  líneas con más de 80 caracteres.
  <sup>[[link](#80-character-limits)]</sup>

* <a name="no-trailing-whitespace"></a> Evita tener espacio en blanco al
  final. <sup>[[link](#no-trailing-whitespace)]</sup>

* <a name="one-file-per-namespace"></a> Usa un archivo por cada espacio
  de nombre o namespace. <sup>[[link](#one-file-per-namespace)]</sup>

* <a name="comprehensive-ns-declaration"></a> Comienza cada namespace
  con una forma comprensiva de tipo `ns`. Si es necesario, ésta debe
  estar compuesta de líneas para `refer`, `require`, e `import`, en ese
  orden. <sup>[[link](#comprehensive-ns-declaration)]</sup>

    ```Clojure
    (ns ejemplos.ns
      (:refer-clojure :exclude [next replace remove])
      (:require [clojure.string :as s :refer [blank?]]
                [clojure.set :as set]
                [clojure.java.shell :as sh])
      (:import java.util.Date
               java.text.SimpleDateFormat
               [java.util.concurrent Executors
                                     LinkedBlockingQueue]))
    ```

* <a name="prefer-require-over-use"></a> En la forma `ns`, debes
  preferir el uso de `:require :as` en lugar de `:require :refer` en
  lugar de `:require :refer :all`. También debes preferir `:require`
  sobre `:use`, ya que la forma `:use` es considerada obsoleta.
  <sup>[[link](#prefer-require-over-use)]</sup>

    ```Clojure
    ;; bueno
    (ns ejemplos.ns
      (:require [clojure.zip :as zip]))

    ;; bueno
    (ns ejemplos.ns
      (:require [clojure.zip :refer [lefts rights]]))

    ;; aceptable según lo garantizado
    (ns ejemplos.ns
      (:require [clojure.zip :refer :all]))

    ;; malo
    (ns ejemplos.ns
      (:use clojure.zip))
    ```

* <a name="no-single-segment-namespaces"></a> Evita el uso de namespaces
  de un solo segmento.
  <sup>[[link](#no-single-segment-namespaces)]</sup>

    ```Clojure
    ;; bueno
    (ns ejemplos.ns)

    ;; malo
    (ns ejemplos)
    ```

* <a name="namespaces-with-5-segments-max"></a> Evita el uso de
  namespaces demasiado largos, es decir, con más de 5 segmentos.
  <sup>[[link](#namespaces-with-5-segments-max)]</sup>

* <a name="10-loc-per-fn-limit"></a> Evita funciones con más de 10
  líneas de código. Idealmente, la mayoría de tus funciones deben de
  tener menos de 5 líneas de código.
  <sup>[[link](#10-loc-per-fn-limit)]</sup>

* <a name="4-positional-fn-params-limit"></a> Evita tener listas de
  argumentos que requieren más de 3 o 4 argumentos en orden.
  <sup>[[link](#4-positional-fn-params-limit)]</sup>

* <a name="forward-references"></a> Evita el uso de referencias directas
  o adelantadas ya que son muy raramente necesarias.
  <sup>[[link](#forward-references)]</sup>

## Sintaxis

* <a name="ns-fns-only-in-repl"></a> Evita el uso de funciones como
  `require` y `refer` que alteran tu namespace. Estas funciones son
  completamente innecesarias fuera de un ambiente REPL.
  <sup>[[link](#ns-fns-only-in-repl)]</sup>

* <a name="declare"></a> Usa la forma `declare` para referencias
  directas o adelantas cuando éstas sean necesarias.
  <sup>[[link](#declare)]</sup>

* <a name="higher-order-fns"></a> Debes preferir funciones de orden
  superior como `map` en lugar de `loop/recur`.
  <sup>[[link](#higher-order-fns)]</sup>

* <a name="pre-post-conditions"></a> Debes preferir pre y post
  condiciones sobre pruebas o tests dentro del cuerpo de una función.
  <sup>[[link](#pre-post-conditions)]</sup>

    ```Clojure
    ;; bueno
    (defn foo [x]
      {:pre [(pos? x)]}
      (bar x))

    ;; malo
    (defn foo [x]
      (if (pos? x)
        (bar x)
        (throw (IllegalArgumentException. "x must be a positive number!")))
    ```

* <a name="dont-def-vars-inside-fns"></a> No definas vars dentro de tus
  funciones. <sup>[[link](#dont-def-vars-inside-fns)]</sup>

    ```Clojure
    ;; muy malo
    (defn foo []
      (def x 5)
      ...)
    ```

* <a name="dont-shadow-clojure-core"></a> No ocultes los nombres de
  `clojure.core` con nuevas definiciones o enlaces locales.
  <sup>[[link](#dont-shadow-clojure-core)]</sup>

    ```Clojure
    ;; malo - necesitamos clojure.core/map para referirnos a la funcion
    (defn foo [map]
      ...)
    ```

* <a name="alter-var"></a> Usa `alter-var-root` en lugar de `def` para
  cambiar el valor de una var. <sup>[[link]](#alter-var)</sup>

    ```Clojure
    ;; bueno
    (def cosa 1) ; valor de cosa es ahora 1
    ; algunos cambios a cosa
    (alter-var-root #'cosa (constantly nil)) ; valor de cosa es ahora nil

    ;; malo
    (def cosa 1)
    ; algunos cambios a cosa
    (def cosa nil)
    ; valor de cosa es ahora nil
    ```

* <a name="nil-punning"></a> Usa la forma `seq` como condición de
  terminación para probar que una secuencia está vacía.
  <sup>[[link](#nil-punning)]</sup>

    ```Clojure
    ;; bueno
    (defn imprimir-seq [s]
      (when (seq s)
        (prn (first s))
        (recur (rest s))))

    ;; malo
    (defn imprimir-seq [s]
      (when-not (empty? s)
        (prn (first s))
        (recur (rest s))))
    ```

* <a name="to-vector"></a> Usa `vec` en lugar de `into` cuando se
  necesite convertir una secuencia a un vector.
  <sup>[[link](#to-vector)]</sup>

    ```Clojure
    ;; bueno
    (vec alguna-seq)

    ;; malo
    (into [] alguna-seq)
    ```

* <a name="when-instead-of-single-branch-if"></a> Usa la forma `when` en
  lugar de `(if ... (do ...)`.
  <sup>[[link](#when-instead-of-single-branch-if)]</sup>

    ```Clojure
    ;; bueno
    (when predicado
      (foo)
      (bar))

    ;; malo
    (if predicado
      (do
        (foo)
        (bar)))
    ```

* <a name="if-let"></a> Usa `if-let` en lugar de `let` + `if`.
  <sup>[[link](#if-let)]</sup>

    ```Clojure
    ;; bueno
    (if-let [resultado (foo x)]
      (algo-con resultado)
      (algo-mas))

    ;; malo
    (let [resultado (foo x)]
      (if resultado
        (algo-con resultado)
        (algo-mas)))
    ```

* <a name="when-let"></a> Usa `when-let` en lugar de `let` + `when`.
  <sup>[[link](#when-let)]</sup>

    ```Clojure
    ;; bueno
    (when-let [resulatado (foo x)]
      (haz-algo-con resulatado)
      (haz-algo-mas-con resulatado))

    ;; malo
    (let [resulatado (foo x)]
      (when resulatado
        (haz-algo-con resulatado)
        (haz-algo-mas-con resulatado)))
    ```

* <a name="if-not"></a> Usa `if-not` en lugar de `(if (not ...) ...)`.
  <sup>[[link](#if-not)]</sup>

    ```Clojure
    ;; bueno
    (if-not predicado
      (foo))

    ;; malo
    (if (not predicado)
      (foo))
    ```

* <a name="when-not"></a> Usa `when-not` en lugar de `(when (not ...)
  ...)`. <sup>[[link](#when-not)]</sup>

    ```Clojure
    ;; bueno
    (when-not pred
      (foo)
      (bar))

    ;; malo
    (when (not pred)
      (foo)
      (bar))
    ```

* <a name="when-not-instead-of-single-branch-if-not"></a> Usa `when-not`
  en lugar de `(if-not ... (do ...)`.
  <sup>[[link](#when-not-instead-of-single-branch-if-not)]</sup>

    ```Clojure
    ;; bueno
    (when-not predicado
      (foo)
      (bar))

    ;; malo
    (if-not predicado
      (do
        (foo)
        (bar)))
    ```

* <a name="not-equal"></a> Usa `not=` en lugar de `(not (= ...))`.
  <sup>[[link](#not-equal)]</sup>

    ```Clojure
    ;; bueno
    (not= foo bar)

    ;; malo
    (not (= foo bar))
    ```

* <a name="printf"></a> Usa `printf` en lugar de `(print (format ...))`.
  <sup>[[link](#printf)]</sup>

    ```Clojure
    ;; bueno
    (printf "Hola, %s!\n" nombre)

    ;; ok
    (println (format "Hola, %s!" nombre))
    ```

* <a name="multiple-arity-of-gt-and-ls-fns"></a> Al hacer comparaciones,
  recuerda que la funciones en Clojure como `<`, `>`, etc. aceptan un
  numero variable de argumentos.
  <sup>[[link](#multiple-arity-of-gt-and-ls-fns)]</sup>

    ```Clojure
    ;; bueno
    (< 5 x 10)

    ;; malo
    (and (> x 5) (< x 10))
    ```

* <a name="single-param-fn-literal"></a> Debes preferir `%` en lugar de
  `%1` en funciones literales con solo un argumento.
  <sup>[[link](#single-param-fn-literal)]</sup>

    ```Clojure
    ;; bueno
    #(Math/round %)

    ;; malo
    #(Math/round %1)
    ```

* <a name="multiple-params-fn-literal"></a> Debes preferir `%1` en lugar
  de `%` en funciones literales con más de un argumento.
  <sup>[[link](#multiple-params-fn-literal)]</sup>

    ```Clojure
    ;; bueno
    #(Math/pow %1 %2)

    ;; malo
    #(Math/pow % %2)
    ```

* <a name="no-useless-anonymous-fns"></a> No envuelvas funciones en
  otras funciones anónimas cuando no sea necesario.
  <sup>[[link](#no-useless-anonymous-fns)]</sup>

    ```Clojure
    ;; bueno
    (filter even? (range 1 10))

    ;; malo
    (filter #(even? %) (range 1 10))
    ```

* <a name="no-multiple-forms-fn-literals"></a> No uses funciones
  literales si el cuerpo de la función consiste en más de una forma.
  <sup>[[link](#no-multiple-forms-fn-literals)]</sup>

    ```Clojure
    ;; bueno
    (fn [x]
      (println x)
      (* x 2))

    ;; malo ya que necesitas usa la forma `do`
    #(do (println %)
         (* % 2))
    ```

* <a name="complement"></a> Debes preferir el uso de `complement` en
  lugar de una función anónima. <sup>[[link](#complement)]</sup>

    ```Clojure
    ;; bueno
    (filter (complement predicado?) coll)

    ;; malo
    (filter #(not (predicado? %)) coll)
    ```

    Esta regla debe de ser ignorada si el complemento de un predicado
    existe como otra función como es el caso con `even?` y `odd?`
    
* <a name="comp"></a> Aplica el uso de `comp` cuando su uso produzca
  código más simple <sup>[[link](#comp)]</sup>

    ```Clojure
    ;; Asumiendo `(:require [clojure.string :as str])`...

    ;; bueno
    (map #(str/capitalize (str/trim %)) ["tope " " prueba "])

    ;; mejor
    (map (comp str/capitalize str/trim) ["tope " " prueba "])
    ```

* <a name="partial"></a> Aplica el uso de `partial` cuando su uso
  produzca código más simple <sup>[[link](#partial)]</sup>

    ```Clojure
    ;; bueno
    (map #(+ 5 %) (range 1 10))

    ;; discutiblemente mejor
    (map (partial + 5) (range 1 10))
    ```
    
* <a name="threading-macros"></a> Debes preferir el uso de threading
  macros como `->` o `->>` en vez de tener muchas expresiones una dentro
  de la otra. <sup>[[link](#threading-macros)]</sup>
  
    ```Clojure
    ;; bueno
    (-> [1 2 3]
        reverse
        (conj 4)
        prn)

    ;; no tan bueno
    (prn (conj (reverse [1 2 3])
               4))

    ;; bueno
    (->> (range 1 10)
         (filter even?)
         (map (partial * 2)))

    ;; no tan bueno
    (map (partial * 2)
         (filter even? (range 1 10)))
    ```
  
* <a name="else-keyword-in-cond"></a> Usa `:else` como la última
  expresión de prueba en `cond`.
  <sup>[[link](#else-keyword-in-cond)]</sup>

    ```Clojure
    ;; bueno
    (cond
      (neg? n) "negativo"
      (pos? n) "positivo"
      :else "cero")

    ;; malo
    (cond
      (neg? n) "negativo"
      (pos? n) "positivo"
      true "cero")
    ```

* <a name="condp"></a> Debes preferir el uso de `condp` en lugar de
  `cond` cuando el predicado y la expresión no cambian.
  <sup>[[link](#condp)]</sup>

    ```Clojure
    ;; bueno
    (cond
      (= x 10) :diez
      (= x 20) :veinte
      (= x 30) :treinta
      :else :desconocido)

    ;; much better
    (condp = x
      10 :diez
      20 :veinte
      30 :treinta
      :desconocido)
    ```

* <a name="case"></a> Debes preferir `case` en lugar de `cond` o `condp`
  cuando las expresiones de prueba son contantes durante el tiempo de
  compilación. <sup>[[link](#case)]</sup>

    ```Clojure
    ;; bueno
    (cond
      (= x 10) :diez
      (= x 20) :veinte
      (= x 30) :treinta
      :else :dunno)

    ;; mejor
    (condp = x
      10 :diez
      20 :veinte
      30 :treinta
      :dunno)

    ;; lo mejor
    (case x
      10 :diez
      20 :veinte
      30 :treinta
      :dunno)
    ```

* <a name="shor-forms-in-cond"></a> Usa formas cortas en `cond` y formas
  relacionadas. Cuando esto no sea posible, trata de proveer pistas
  visuales indicando los grupos que son pares a través de comentarios o
  líneas vacías. <sup>[[link](#shor-forms-in-cond)]</sup>

    ```Clojure
    ;; bueno
    (cond
      (prueba1) (accion1)
      (prueba2) (accion2)
      :else   (accion-por-defecto))

    ;; más o menos
    (cond
      ;; prueba caso 1
      (prueba1)
      (funcion-larga-que-require-una-nueva-linea
        (subforma-complicada
          (-> 'que-ocupa multiples-lineas)))

      ;; prueba caso 2
      (prueba2)
      (otra-funcion-larga
        (con-otra-subforma
          (-> 'que-ocupa multiples-lineas)))

      :else
      (el-caso-de-accion-por-defecto
        (que-tambien-abarca 'multiples
                          lineas)))
    ```

* <a name="set-as-predicate"></a> Usa un conjunto o `set` como predicado
  cuando sea apropiado <sup>[[link](#set-as-predicate)]</sup>

    ```Clojure
    ;; bueno
    (remove #{1} [0 1 2 3 4 5])

    ;; malo
    (remove #(= % 1) [0 1 2 3 4 5])

    ;; bueno
    (count (filter #{\a \e \i \o \u} "un elefante se balanceaba"))

    ;; malo
    (count (filter #(or (= % \a)
                        (= % \e)
                        (= % \i)
                        (= % \o)
                        (= % \u))
                   "un elefante se balanceaba"))
    ```

* <a name="inc-and-dec"></a> Usa `(inc x)` y `(dec x)` en lugar de `(+ x
  1)` y `(- x 1)`. <sup>[[link](#inc-and-dec)]</sup>

* <a name="pos-and-neg"></a> Usa `(pos? x)`, `(neg? x)` y `(zero? x)` en
  lugar de `(> x 0)`, `(< x 0)` y `(= x 0)`.
  <sup>[[link](#pos-and-neg)]</sup>

* <a name="list-star-instead-of-nested-cons"></a> Usa `list*` en lugar
  de una lista en línea de invocaciones de `cons`.
  <sup>[[link](#list-star-instead-of-nested-cons)]</sup>

    ```Clojure
    ;; bueno
    (list* 1 2 3 [4 5])

    ;; malo
    (cons 1 (cons 2 (cons 3 [4 5])))
    ```

* <a name="sugared-java-interop"></a> Usa formas de interoperación con
  Java que han sido endulzadas sintácticamente
  <sup>[[link](#sugared-java-interop)]</sup>

    ```Clojure
    ;;; Creación de objetos
    ;; bueno
    (java.util.ArrayList. 100)

    ;; malo
    (new java.util.ArrayList 100)

    ;;; Invocación de método estático
    ;; bueno
    (Math/pow 2 10)

    ;; malo
    (. Math pow 2 10)

    ;;; invocación de método de instancia
    ;; bueno
    (.substring "hola" 1 3)

    ;; malo
    (. "hola" substring 1 3)

    ;;; acceso a campo o valor estático
    ;; bueno
    Integer/MAX_VALUE

    ;; malo
    (. Integer MAX_VALUE)

    ;;; acceso de campo o valor de instancia
    ;; bueno
    (.algunCampo algun-objeto)

    ;; malo
    (. algun-objeto algunCampo)
    ```

* <a name="compact-metadata-notation-for-true-flags"></a> Usa una
  anotación compacta para metadatos para quienes sus valores han de ser
  booleanos.
  <sup>[[link](#compact-metadata-notation-for-true-flags)]</sup>

    ```Clojure
    ;; bueno
    (def ^:private a 5)

    ;; malo
    (def ^{:private true} a 5)
    ```

* <a name="private"></a> Denota secciones privadas de tu código como
  tal. <sup>[[link](#private)]</sup>

    ```Clojure
    ;; bueno
    (defn- funcion-privada [] ...)

    (def ^:private var-privada ...)

    ;; malo
    (defn funcion-privada [] ...) ; no es privada en lo absoluto

    (defn ^:private funcion-privada [] ...) ; demasiado verboso

    (def var-privada ...) ; no es privada en lo absoluto
    ```
    
* <a name="access-private-var"></a> Para acceder una var privada, por
  ejemplo, durante tus pruebas, usa la forma `@# algun.ns/var`
  <sup>[[link](#access-private-var)]</sup>

* <a name="attach-metadata-carefully"></a> Ten cuidado con respecto a
  exactamente que se le adjuntan metadatos.
  <sup>[[link](#attach-metadata-carefully)]</sup>

    ```Clojure
    ;; se le adjuntan metadatos a la var referenciada por `a`
    (def ^:private a {})
    (meta a) ;=> nil
    (meta #'a) ;=> {:private true}

    ;; se le adjuntan metadatos al valor vacío del hash-map
    (def a ^:private {})
    (meta a) ;=> {:private true}
    (meta #'a) ;=> nil
    ```

## Nombramiento
> Las únicas dificultades reales en la programación son la invalidación
> de caché y el nombrar las cosas. <br/> -- Phil Karlton

* <a name="ns-naming-schemas"></a> Al nombrar tus namespaces, usa las
  siguientes dos esquemas: <sup>[[link](#ns-naming-schemas)]</sup>
  organizacion

    * `proyecto.modulo`
    * `organizacion.proyecto.modulo`

* <a name="lisp-case-ns"></a> Usa el nombramiento estilo `lisp` con
  segmentos compuestos de namespaces como por ejemplo
  `diego.project-euler`) <sup>[[link](#lisp-case-ns)]</sup>

* <a name="lisp-case"></a> Usa el nombramiento estilo `lisp` para
  nombres de funciones y variables. <sup>[[link](#lisp-case)]</sup>

    ```Clojure
    ;; bueno
    (def alguna-var ...)
    (defn alguna-funcion ...)

    ;; malo
    (def algunaVar ...)
    (defn algunafuncion ...)
    (def alguna_funcion ...)
    ```

* <a name="CamelCase-for-protocols-records-structs-and-types"></a> Usa
  el nombramiento estilo `CamelCase` para protocolos, records, y tipos y
  asegúrate de mantener en mayúscula siglas como HTTP, RFC, XML.
  <sup>[[link](#CamelCase-for-protocols-records-structs-and-types)]</sup>
  
  
* <a name="pred-with-question-mark"></a> Al nombrar métodos que
  funcionan como predicados, es decir, aquellos que retornan un valor
  booleano de true o false, deben de terminar con un signo de
  interrogación. <sup>[[link](#pred-with-question-mark)]</sup>

    ```Clojure
    ;; bueno
    (defn palindromo? ...)

    ;; malo
    (defn palindromo-p ...) ; Estilo de Common Lisp
    (defn es-palindromo ...) ; Estilo de Java
    ```
  
* <a name="changing-state-fns-with-exclamation-mark"></a> Los nombres de
  funciones o macros que no son seguras bajo transacciones STM deben de
  terminar con un signo de admiración.
  <sup>[[link](#changing-state-fns-with-exclamation-mark)]</sup>

* <a name="arrow-instead-of-to"></a> Usa `->` al nombrar funciones de
  conversion. <sup>[[link](#arrow-instead-of-to)]</sup>

    ```Clojure
    ;; bueno
    (defn f->c ...)

    ;; no tan bueno
    (defn f-a-c ...)
    ```

* <a name="earmuffs-for-dynamic-vars"></a>

  Agrégale *asteriscos* al principio y al final de los nombres de
  variables dinámicas. <sup>[[link](#earmuffs-for-dynamic-vars)]</sup>

    ```Clojure
    ;; bueno
    (def ^:dynamic *a* 10)

    ;; malo
    (def ^:dynamic a 10)
    ```

* <a name="dont-flag-constants"></a> Recuerda que no es necesario usar
  ninguna anotación especial para denotar constantes. Esto es debido a
  que se asume que todo es constante a menos que se especifique lo
  contrario. <sup>[[link](#dont-flag-constants)]</sup>

* <a name="underscore-for-unused-bindings"></a> Usa un guión bajo`_`
  para denotar argumentos o variables de deconstrucción cuyo valor no es
  usado por el resto del cuerpo.
  <sup>[[link](#underscore-for-unused-bindings)]</sup>

    ```Clojure
    ;; bueno
    (let [[a b _ c] [1 2 3 4]]
      (println a b c))

    (dotimes [_ 3]
      (println "Hello!"))

    ;; malo
    (let [[a b c d] [1 2 3 4]]
      (println a b d))

    (dotimes [i 3]
      (println "Hola!"))
    ```

* <a name="idiomatic-names"></a> Sigue el ejemplo del namespace
  `clojure.core` usando nombres como `pred` y `coll`.
    * en funciones, usa
        * `f`, `g`, `h` - para representar argumentos
        * `n` - para representar una cantidad
        * `index`, `i` - para representar un índice numeral
        * `x`, `y` - para números
        * `xs` - para secuencias
        * `m` - para mapas
        * `s` - para strings
        * `re` - para expresiones regulares o regex
        * `coll` - para colecciones
        * `pred` - para predicados
        * `& more` - para representar argumentos variados
        * `xf` - para una xforma o un transducer
    * en macros, usa
        * `expr` - para expresiones
        * `body` - para el cuerpo del macro
        * `binding` - para el vector de enlace en el macro

## Colecciones
> Es mejor tener 100 funciones que operan una sola estructura de datos
> que tener 10 funciones que operan en 10 estructuras de datos <br/> --
> Alan J. Perlis

* <a name="avoid-lists"></a> A menos de que su estructura sea necesaria,
  evita usar listas para uso genérico. <sup>[[link](#avoid-lists)]</sup>

* <a name="keywords-for-hash-keys"></a> Debes preferir el uso de
  keywords como claves o keys.
  <sup>[[link](#keywords-for-hash-keys)]</sup>

    ```Clojure
    ;; bueno
    {:nombre "Daniel" :edad 30}

    ;; malo
    {"nombre" "Daniel" "edad" 30}
    ```

* <a name="literal-col-syntax"></a> Debes preferir el uso del sintaxis
  literal para colecciones en vez de sus respectivos constructores. Sin
  embargo, al definir conjuntos o sets, solo usa el sintaxis literal si
  los valores son constantes al momento de compilación.
  <sup>[[link](#literal-col-syntax)]</sup>

    ```Clojure
    ;; bueno
    [1 2 3]
    #{1 2 3}
    (hash-set (func1) (func2)) ; Valores determinados en compilación

    ;; malo
    (vector 1 2 3)
    (hash-set 1 2 3)
    #{(func1) (func2)} ; Lanzará una excepción si (func1) = (func2)
    ```

* <a name="avoid-index-based-coll-access"></a>
  
  En lo posible, evita tener que acceder miembros de una colección
  usando su índice. <sup>[[link](#avoid-index-based-coll-access)]</sup>

* <a name="keywords-as-fn-to-get-map-values"></a> Debes preferir el uso
  de keywords como funciones al acceder valores de mapas.
  <sup>[[link](#keywords-as-fn-to-get-map-values)]</sup>

    ```Clojure
    (def m {:nombre "Daniel" :edad 30})

    ;; bueno
    (:nombre m)

    ;; más verboso de lo necesario
    (get m :nombre)

    ;; malo - suceptible a un NullPointerException
    (m :nombre)
    ```

* <a name="colls-as-fns"></a> Utiliza el hecho de que la mayoría de
  colecciones actúan como funciones de sus miembros.
  <sup>[[link](#colls-as-fns)]</sup >
   
    ```Clojure
    ;; bueno
    (filter #{\a \e \o \i \u} "esto es una prueba")

    ;; malo - demasiado feo para ser compartido
    ```

* <a name="keywords-as-fns"></a> Utiliza el hecho de que keywords pueden
  actuar como funciones de una colección
  <sup>[[link](#keywords-as-fns)]</sup>

    ```Clojure
    ((juxt :a :b) {:a "ala" :b "bala"})
    ```

* <a name="avoid-transient-colls"></a> Evita hacer tus colecciones
  "transient", a menos que se trate de pociones de tu código que necesitan
  muy alto rendimiento. <sup>[[link](#avoid-transient-colls)]</sup>

* <a name="avoid-java-colls"></a> Evita el uso de colecciones de Java.
  <sup>[[link](#avoid-java-colls)]</sup>

* <a name="avoid-java-arrays"></a> Evita el uso de arrays de Java,
  excepto en escenarios de interoperabilidad o código de alto
  rendimiento que trata con tipos de Java primitivos.
  <sup>[[link](#avoid-java-arrays)]</sup>

## Mutación

### Refs

* <a name="refs-io-macro"></a>

 Considera envolver todas tus usos de input/output, o I/O con el macro
 `io!` para evitar malas sorpresas si accidentalmente llamas ese tipo de
 código en una transacción. Esto es debido a que las transacciones
 pueden ser llamadas varias veces y, por ende, el código que contienen
 debe ser puro (sin efectos externos).
 <sup>[[link](#refs-io-macro)]</sup>

* <a name="refs-avoid-ref-set"></a> Evita el uso de `ref-set` en lo
  posible. <sup>[[link](#refs-avoid-ref-set)]</sup>

    ```Clojure
    (def r (ref 0))

    ;; bueno
    (dosync (alter r + 5))

    ;; malo
    (dosync (ref-set r 5))
    ```

* <a name="refs-small-transactions"></a>
  
  Intenta de mantener al mínimo el tamaño de tus transacciones, es
  decir, la cantidad de trabajo encapsulada en ellas.
  <sup>[[link](#refs-small-transactions)]</sup>

* <a name="refs-avoid-short-long-transactions-with-same-ref"></a> Evita
  tener transacciones largas y cortas interactuando con un mismo Ref.
  <sup>[[link](#refs-avoid-short-long-transactions-with-same-ref)]</sup>

### Agents

* <a name="agents-send"></a> Usa `send` solamente para acciones
  limitadas por la CPU y que no bloquean hilos o threads, en especial
  el de I/O. <sup>[[link](#agents-send)]</sup>

* <a name="agents-send-off"></a> Usa `send'off` para acciones que
  pueden, de alguna forma, bloquear un thread.
  <sup>[[link](#agents-send-off)]</sup>

### Átomos

* <a name="atoms-no-update-within-transactions"></a> Evita actualizar
  átomos dentro de transacciones STM.
  <sup>[[link](#atoms-no-update-within-transactions)]</sup>

* <a name="atoms-prefer-swap-over-reset"></a> En lo posible, debes
  preferir el uso de `swap!` en lugar de `reset!`.
  <sup>[[link](#atoms-prefer-swap-over-reset)]</sup>

    ```Clojure
    (def a (atom 0))

    ;; bueno
    (swap! a + 5)

    ;; no tan bueno
    (reset! a 5)
    ```

## Strings o cuerdas

* <a name="prefer-clojure-string-over-interop"></a>
  
  Debes preferir el uso de funciones de manipulaciones provenientes de
  `clojure.string` en lugar de interoperar con Java o escribir tus
  propias versiones.
  <sup>[[link](#prefer-clojure-string-over-interop)]</sup>

    ```Clojure
    ;; bueno
    (clojure.string/upper-case "daniel")

    ;; malo
    (.toUpperCase "daniel")
    ```

## Excepciones

* <a name="reuse-existing-exception-types"></a> Si es necesario lanzar
  una excepción, asegúrate de usar tipos de excepciones ya existentes.
  Por ejemplo, `java.lang.IllegalArgumentException`,
  `java.lang.UnsupportedOperationException`,
  `java.lang.IllegalStateException`, `java.io.IOException`
  <sup>[[link](#reuse-existing-exception-types)]</sup>

* <a name="prefer-with-open-over-finally"></a> Debes preferir el uso
  de `with-open` en lugar de `finally`.
  <sup>[[link](#prefer-with-open-over-finally)]</sup>

## Macros

* <a name="dont-write-macro-if-fn-will-do"></a>
  No escribas un macro si una función es suficiente.
<sup>[[link](#dont-write-macro-if-fn-will-do)]</sup>

* <a name="write-macro-usage-before-writing-the-macro"></a> Construye
  un ejemplo de uso para un macro antes de escribir el macro en sí.
  <sup>[[link](#write-macro-usage-before-writing-the-macro)]</sup>

* <a name="break-complicated-macros"></a> En lo posible, divide macros
  complicados en funciones pequeñas.
  <sup>[[link](#break-complicated-macros)]</sup>

* <a name="macros-as-syntactic-sugar"></a> Un macro debe proveer
  `azúcar sintáctico`, es decir, debe simplificar el formación
  sintáctica de la expresión. El núcleo del macro deben de ser
  funciones simples ya que esto incrementa su capacidad de compilación.
  <sup>[[link](#macros-as-syntactic-sugar)]</sup>

* <a name="syntax-quoted-forms"></a>
  Debes preferir formas con sintaxis citado en lugar de la
  construcción manual de listas. 
<sup>[[link](#syntax-quoted-forms)]</sup>

## Comentarios

> Buen código es su mejor propia documentación. Al prepararte para
> agregar un comentario, debes preguntarte, "¿Cómo puedo mejorar mi
> código de tal forma que este comentario no sea necesario?" Mejora tu
> código primero y después documéntalo para hacerlo aun más claro. <br/>
> -- Steve McConnell

* <a name="self-documenting-code"></a> Haz que tu código sea tan
  auto-documentado como sea posible.
  <sup>[[link](#self-documenting-code)]</sup>

* <a name="four-semicolons-for-heading-comments"></a> Escribe
  comentarios de encabezadura con al menos 4 puntos y comas.
  <sup>[[link](#four-semicolons-for-heading-comments)]</sup>

* <a name="three-semicolons-for-top-level-comments"></a>
  Escribe comentarios principales con 3 puntos y comas.
<sup>[[link](#three-semicolons-for-top-level-comments)]</sup>

* <a name="two-semicolons-for-code-fragment"></a>
  Escribe comentarios acerca de un fragmento de código con dos puntos y
  comas. Además, asegúrate de escribir el comentario antes del fragmento
  que al que hace referencia y alinéalo a él.
<sup>[[link](#two-semicolons-for-code-fragment)]</sup>

* <a name="one-semicolon-for-margin-comments"></a>
  Escribe comentarios marginales con un punto y coma.
<sup>[[link](#one-semicolon-for-margin-comments)]</sup>

* <a name="semicolon-space"></a>
  Asegúrate de siempre tener al menos un espacio entre un punto y coma y
  el texto que lo prosigue.
<sup>[[link](#semicolon-space)]</sup>

    ```Clojure
    ;;;; Título

    ;;; Esta sección de código tiene estas importantes implicaciones:
    ;;;   1. Foo.
    ;;;   2. Bar.
    ;;;   3. Baz.

    (defn funcion [argumento]
      ;; Si zob, entonces veeblefitz.
      (quux zot
            mumble             ; Zibblefrotz.
            frotz))
    ```

* <a name="english-syntax"></a>
  Comentarios compuestos de más de una palabra deben empezar con
  mayúscula y usar puntuación. También separa oraciones con un
  [un espacio](http://es.wikipedia.org/wiki/Espacio_entre_oraciones).
<sup>[[link](#english-syntax)]</sup>

* <a name="no-superfluous-comments"></a>
  Evita el uso de comentarios innecesarios.
<sup>[[link](#no-superfluous-comments)]</sup>

    ```Clojure
    ;; malo
    (inc contador) ; Incrementa el valor del contador por uno
    ```

* <a name="comment-upkeep"></a> Mantén vigentes tus comentarios. Un
  comentario desactualizado es peor que no tener un comentario. 
<sup>[[link](#comment-upkeep)]</sup>

* <a name="dash-underscore-reader-macro"></a> Debes preferir el uso de
  macro lector `#_` en lugar de un comentario regular al comentar una
  forma en su totalidad.
  <sup>[[link](#dash-underscore-reader-macro)]</sup>

    ```Clojure
    ;; bueno
    (+ foo #_(bar x) delta)

    ;; malo
    (+ foo
       ;; (bar x)
       delta)
    ```

> Buen código es como un buen chiste... no necesita explicación. <br/>
> -- Russ Olsen

* <a name="refactor-dont-comment"></a> Evita escribir comentarios para
  documentar código malo. Mejora tu código con el fin de hacer que se
  auto documente. ("Hacer, o no hacer. No hay intentar." --Yoda)
  <sup>[[link](#refactor-dont-comment)]</sup>

### Anotaciones de comentarios

* <a name="annotate-above"></a> Anotaciones debe de ser escritas en la
  línea inmediatamente antes del código al que referencia.
  <sup>[[link](#annotate-above)]</sup>

* <a name="annotate-keywords"></a> La anotación debe estar compuesta
  por el keyword de anotacion seguido por dos puntos y un espacio,
  después por una nota describiendo el problema.
  <sup>[[link](#annotate-keywords)]</sup>

* <a name="indent-annotations"></a> Si la descripción de un problema
  requiere el uso de múltiples líneas, todas las líneas deben seguir
  la misma indentación o sangría de la primera línea.
  <sup>[[link](#indent-annotations)]</sup>

* <a name="sing-and-date-annotations"></a> Marca tu anotación con
  fecha e iniciales para que su relevancia sea fácilmente verificada.
  <sup>[[link](#sing-and-date-annotations)]</sup>

    ```Clojure
    (defn alguna-funcion
      []
      ;; FIXME: Esto ha causado problemas ocasionalmente en v1.2.3. 
      ;;        Es posible que sea relacionado con la actualizacion
      ;;        de BarUtilidad (xz 13-1-31)
      (baz))
    ```

* <a name="rare-eol-annotations"></a> En casos donde el problema es
  tan evidente que cualquier tipo de documentación es redundante,
  anotaciones sin notas pueden ser hechas después del código al que se
  hace referencia. Recuerda que este uso debe ser la excepción y no la
  regla. <sup>[[link](#rare-eol-annotations)]</sup>

    ```Clojure
    (defn bar
      []
      (sleep 100)) ; OPTIMIZE
    ```

* <a name="todo"></a> Usa `TODO` para marcar funcionalidad que debe
  ser agregada después. <sup>[[link](#todo)]</sup>

* <a name="fixme"></a> Usa `FIXME` para marcar código fallido que debe
  ser arreglado. <sup>[[link](#fixme)]</sup>

* <a name="optimize"></a> Usa `OPTIMIZE` para marcar código lento o
  ineficiente que puede causar problemas de rendimiento
  <sup>[[link](#optimize)]</sup>

* <a name="hack"></a> Usa `HACK` para anotar usos cuestionables de las
  practicas usadas que deben ser corregidas.
  <sup>[[link](#hack)]</sup>

* <a name="review"></a> Usa `REVIEW` para marcar código que debe ser
  revisado para confirmar que esta trabajando correctamente. Por
  ejemplo: `REVIEW: ¿Estamos segurlos que el cliente untiliza X en
  este momento?` <sup>[[link](#review)]</sup>

* <a name="document-annotations"></a> Usa otros keywords de anotacion
  cuando sea apropriado y asegúrate de documentarlos en el `README` o
  archivo equivalente de tu proyecto.
  <sup>[[link](#document-annotations)]</sup>

## Existencial

* <a name="be-functional"></a> Escribe código de forma funcional
  usando mutación solo cuando tenga sentido.
  <sup>[[link](#be-functional)]</sup>

* <a name="be-consistent"></a> Sé consistente con el uso de esta guía.
  <sup>[[link](#be-consistent)]</sup>

* <a name="common-sense"></a> Usa el sentido común.
  <sup>[[link](#common-sense)]</sup>

## Herramientas

Aquí presentamos algunas herramientas creadas por la comunidad de
Clojure que pueden ayudarte en tu camino para escribir Clojure idiomático.

* [Slamhound](https://github.com/technomancy/slamhound) es una
  herramienta que automáticamente genera declaraciones de `ns` para tu
  código existente.
  
* [kibit](https://github.com/jonase/kibit) es un analizador estático de
  código para Clojure que usa
  [core.logic](https://github.com/clojure/core.logic para encontrar
  patrones de código para los cuales pueden existir formas, funciones, o
  macros más idiomáticos.

## Pruebas

 * <a name="test-directory-structure"></a> Guarda tus pruebas en un
   directorio separado. Normalmente, bajo `test/tuproyecto/` en lugar
   de `src/tuproyecto/`. Tu herramienta de construcción es
   responsable de hacer que estos archivos estén disponibles donde sean
   necesarios. La mayoría de modelos hacen esto automáticamente.
   <sup>[[link](#test-directory-structure)]</sup>
   
 * <a name="test-ns-naming"></a>
   Nombre tu ns `tuproyecto.algo-test`, un archivo que usualmente es nombrado
   `test/tuproyecto/algo_test.clj` (o `.cljc`, `cljs`).
   <sup>[[link](#test-ns-naming)]</sup>

 * <a name="test-naming"></a> Al usar `clojure.test, define tus prubeas
 con `deftest` y nombrales `algo-test`. Por ejemplo

   ```clojure
   ;; bueno
   (deftest algo-test ...)

   ;; malo
   (deftest algo-tests ...)
   (deftest test-algo ...)
   (deftest algo ...)
   ```
   <sup>[[link](#test-naming)]</sup>

## Organización de librerías 

 * <a name="lib-coordinates"></a> Si estas publicando librerías con el
   fin de que sean usadas por otros, asegúrate de seguir la [Central
   Repository
   guidelines](http://central.sonatype.org/pages/choosing-your-coordinates.html)
   para escoger tu `groupId` and `artifactId`. Esto ayuda prevenir
   conflictos de nombre y facilita cualquier tipo de uso. Un buen
   ejemplo es el de
   [Component](https://github.com/stuartsierra/component).
   <sup>[[link](#lib-coordinates)]</sup>

 * <a name="lib-min-dependencies"></a>
   Evita incluir dependencias innecesarias. Por ejemplo, es preferible
   copiarle una función de 3 líneas a tu proyecto en lugar de una
   dependencia que conlleva cientos de vars que no piensas usar.
   <sup>[[link](#lib-min-dependencies)]</sup>

 * <a name="lib-core-separate-from-tools"></a> Separa la funcionalidad
   central de tu librería y sus puntos de integración en distintos
   artefactos. De esta forma, usuarios pueden consumir tu librería sin
   tener que estar atados a preferencias de herramientas externas. Por
   ejemplo, [Component](https://github.com/stuartsierra/component)
   provee functionalidad central, y
   [reloaded](https://github.com/stuartsierra/reloaded) provee
   integración con Leiningen.
   <sup>[[link](#lib-core-separate-from-tools)]</sup>
   
# Contribuciones

Nada escrito en esta guía está escrito en piedra. Mi deseo es trabajar
junto con todos aquellos interesados en el estilo de código Clojure
con el fin de poder construir un recurso que sea beneficioso para la
comunidad entera de Clojure.

No tengas miedo de abrir tickets o hacer pull requests con mejorías.
De antemano, muchas gracias por tu ayuda!

Es posible contribuir a la guía de estilo financialmente a través de 
[gittip](https://www.gittip.com/bbatsov).

[![Contribuye a través de Gittip](https://rawgithub.com/twolfson/gittip-badge/0.2.0/dist/gittip.png)](https://www.gittip.com/bbatsov)

# Licencia

![Creative Commons License](http://i.creativecommons.org/l/by/3.0/88x31.png)
Este trabajo está licenciado bajo la licencia
[Creative Commons Attribution 3.0 Unported License](http://creativecommons.org/licenses/by/3.0/deed.en_US)

# Difunde la palabra

Una guía de estilo escrita por la comunidad es de poco uso si la misma
comunidad no sabe de su existencia. Por ende, comparte la guía con tus
amigos y colegas. Cada comentario, sugerencia u opinión hace que esta
guía sea solo un tanto mejor. Y queremos tener la mejor guía posible, cierto?

Salud,
[Bozhidar](https://twitter.com/bbatsov)
