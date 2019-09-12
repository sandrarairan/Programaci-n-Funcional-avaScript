# Programaci-n-Funcional-javaScript
Programación Funcional en JavaScript por Bedu- Curso PLatzi

## Funciones Algebraicas y Funciones de JavaScript
Por lo general, utilizamos las funciones de JavaScript para realizar algunas operaciones o una secuencia de procedimientos. También podemos convertir una función algebraica a una función pura de JavaScript: definimos una función F que recibe el parámetro X y devuelve el valor de X*2.

F(x) = 2 * x
Por supuesto, la manera de utilizar este tipo de funciones es asignando el valor de X:

F(2) = 2 * 2
F(2) = 4
Estas funciones siempre van a devolver el mismo resultado, es decir, si entregamos el parámetro 2, siempre vamos a recibir un 4, si entregamos el parámetro 3, siempre vamos a recibir 6 y así sucesivamente.

Las funciones en JavaScript funcionan de la misma manera:

const double = (x) => x*2
double(2) // 4
En la siguiente clase vamos a ver otros ejemplos de funciones puras en JavaScript.Funciones puras
Las funciones puras siempre devuelven el mismo resultado cuando reciben los mismos parámetros. En cambio, otras funciones que dependen de factores externos (como el tiempo o una petición HTTP) no siempre pueden devolver el mismo resultado aunque reciban los mismos parámetros, incluso, pueden no necesitar recibir parámetros para ejecutarse correctamente.

Ejemplos de funciones puras:

const double = x => x*2
double(2) // siempre es 4
double(3) // siempre es 6

const isGreaterThan = (value, comparison) => value > comparison
isGreaterThan(5, 6) // siempre devuelve false
isGreaterThan(8, 6) // siempre devuelve true
Ejemplos de funciones que NO son puras:

const time = () => new Data().toLocalTimeString()
time() // siempre devuelve un resultado diferente


## Funciones puras
Las funciones puras siempre devuelven el mismo resultado cuando reciben los mismos parámetros. En cambio, otras funciones que dependen de factores externos (como el tiempo o una petición HTTP) no siempre pueden devolver el mismo resultado aunque reciban los mismos parámetros, incluso, pueden no necesitar recibir parámetros para ejecutarse correctamente.

Ejemplos de funciones puras:

const double = x => x*2
double(2) // siempre es 4
double(3) // siempre es 6

const isGreaterThan = (value, comparison) => value > comparison
isGreaterThan(5, 6) // siempre devuelve false
isGreaterThan(8, 6) // siempre devuelve true
Ejemplos de funciones que NO son puras:

const time = () => new Data().toLocalTimeString()
time() // siempre devuelve un resultado diferente

## Objetos y Tipos de Memoria en JavaScript
Un objeto es una referencia a un espacio en memoria, cada vez que creamos un objeto, este se guarda en la memoria (no sabemos exactamente dónde) y podemos acceder a su valor gracias a las coordenadas.

Existen dos tipos de memoria: Stack y Heap.
La memoria Stack es mucho más rápida y nos permite almacenar los datos de forma ““ordenada”” y en JavaScript la utilizamos para guardar datos primitivos, como booleanos, números o strings. En cambio, los objetos utilizan la memoria Heap, una memoria que nos permite guardar grandes cantidades de información pero con un poco menos de velocidad.

Estos dos conceptos nos van a ayudar mucho a la hora de copiar objetos cuando utilizamos la programación funcional.

## Copiar y modificar objetos en JavaScript
En JavaScript tenemos diferentes formas de copiar y modificar elementos o variables, normalmente, basta con asignar dos variables e indicar que la segunda es igual a la primera:

let a = 1
let b = a

console.log(a, b) // 1, 1
De esta forma podemos copiar el valor de otra variable y realizar modificaciones más adelante:

let a = 1
let b = a
b += 1

console.log(a, b) // 1, 2
Sin embargo, todo esto cambia cuando trabajamos con objetos. Así como aprendimos en la clase anterior, los objetos se comportan distinto al resto de datos primitivos dentro de JavaScript.

Cuando asignamos el valor de una variable de tipo objeto a otras variables, en realidad, estamos copiando la referencia al objeto inicial. Esto quiere decir que, a pesar de que modifiquemos la copia de nuestras variables de tipo objeto, en realidad, estamos modificando el objeto original y, por lo tanto, todas las variables con la referencia a este objeto que acabamos de modificar:

let car = {
        color: 'red',
        year: 2019,
        km: 0,
}

let car2 = car
car2.color = 'blue'

console.log(car, car2) // ambos objetos tienen color azul, no solo `car2`
En vez de copiar los valores de nuestros objetos, cuando utilizamos el = lo que copiamos es la referencia al objeto con sus respectivos valores. Esto lo podemos solucionar utilizando la función Object.assign:

let car = {
        color: 'red',
        year: 2019,
        km: 0,
}

let car2 = Object.assign({} , car)
car2.color = 'blue'

console.log(car, car2) // `car` es de color rojo y `car2` de color azul
Sin embargo, este método no es suficiente para copiar y modificar objetos con subobjetos por el mismo problema de las referencias. La mejor manera copiar los valores de nuestros objetos en vez de sus referencias es utilizando las funciones JSON.parse y JSON.stringify:

let car = {
        color: 'red',
        year: 2019,
        km: 0,
        owner: {
                name: 'David',
                age: 25
        }
}

let car2 = JSON.parse(JSON.stringify(car))
car2.owner.age += 1

console.log(car, car2) // el dueño de `car2` es un año mayor al dueño de `car`

## Utilizando inmutabilidad en nuestras funciones
Otra característica de las funciones puras es la inmutabilidad. Si necesitamos modificar el valor de los parámetros que reciben nuestras funciones, debemos copiar el valor de los argumentos y modificar estas nuevas variables, así evitamos modificar innecesariamente variables con las que nuestras funciones puras no tienen nada que ver.

Ejemplo:

// Con mutaciones
const addToList = (list, item, quantity) => {
	list.push({ // modificamos el argumento `list`
		item,
		quantity
	})
	return list
}

//  Sin mutaciones (inmutabilidad)
const addToList = (list, item, quantity) => {
	const newList = JSON.parse(JSON.stringify(list))
	newList.push({ // modificamos la copia del argumento
		item,
		quantity
	})

	return newList
}

# Estado compartido o shared state
Shared State significa que diferentes métodos trabajan a partir de una misma variable. y, así como aprendimos en clases anteriores, cuando modificamos variables con el mismo objeto de referencia podemos encontrarnos con algunos problemas y obtener resultados inesperados a pesar de ejecutar el mismo código y recibir los mismos parámetros:

// Intento #1
const a = {
        value: 2
}

const addOne = () => a.value += 1
const timesTwo = () => a.value *= 2

addOne()
timesTwo()

console.log(a.value) // 6

// Sin embargo, si ejecutamos las mismas funciones en orden invertido
// obtenemos resultados diferentes

timesTwo()
addOne()

console.log(a.value) // 5 !??
Para resolver este tipo de problemas debemos utilizar la programación funcional, en vez de modificar la variable original, nuestras funciones deben copiar y modificar sus argumentos:

const b = {
        value: 2
}

const addOne = x => Object.assign({}, x, { value: x.value + 1 })
const timesTwo = x => Object.assign({}, x, { value: x.value * 2 })

addOne(b)
timesTwo(b)

// El resultado siempre es el mismo a pesar de
// ejecutar las funciones en orden diferente

timesTwo(b)
addOne(b)

console.log(b.value)
## Composición de funciones, Closures y Currying
# Funciones compuestas o Function Composition
Conocemos como Function Composition a las funciones que obtenemos como resultado de combinar otras dos o más funciones, el resultado de cada función es el argumento de la siguiente y así sucesivamente.

# Closures en programación funcional
Los Closures son funciones que retornan otras funciones y recuerdan el scope en el que fueron creadas, es decir, son funciones que utilizan principios de la programación funcional, no modifica el valor de variables u objetos externos, más bien, utilizan sus propias variables independientes (a partir de los parámetros que reciban estas funciones) para dar resultados correctos.

Closures: Es una función que retorna otra función
Ejemplo:

functionbuildSum(a){
	returnfunction(b){
		return a+b
	}
}
Y con arrow functions:

const buildSum = a => b => a+b

# Currying
Gracias a los closures es posible implementar el Currying, descomponer funciones complejas en otras funciones más pequeñas donde cada función recibe un solo argumento. A continuación un ejemplo:

// Sin Currying
function sumThreeNumbers(a, b, c) {
        return a + b + c
}

console.log(sumThreeNumbers(1, 2, 3)) // 6

function sumThreeNumbers(a) {
        return function(b) {
                return function(c) {
                        return a + b + c
                }
        }
}

console.log(sumThreeNumbers(1)(2)(3)) // 6

## Higher Order Functions
# Introducción a las Higher Order Functions
Por ahora, todas las funciones que hemos construido se pueden definir como First Class Functions, sin embargo, existen otro tipo de funciones que conocemos como Higher Order Functions o funciones de alto orden y podemos distinguirlas porque reciben otra función como argumento.

Un buen ejemplo de funciones de alto orden es la función .map de JavaScript:

// Ciclo for (sin HOF)
const array = [1, 2, 3]
const array2 = []

for (let i = 0; let i < array.length; i++) {
        array2.push(array[i] * 2)
}

// Utilizando la función .map (HOF)
const array = [1, 2, 3]
const array2 = array.map(item => item * 2)

// Ambas formas devuelven el mismo resultado,
// sin embargo, utilizando HOFs podemos escribir
// código mucho más legible y fácil de entender
console.log(array2) // [2, 4, 6]

# Programación Declarativa
La programación imperativa consiste en explicar paso a paso cómo conseguir un resultado, en cambio, la programación declarativa se centra en qué hay que hacer.

Podemos utilizar programación declarativa trabajando con funciones especiales de JavaScript. Por ejemplo, en vez de utilizar un ciclo for, podemos utilizar la función .map para ejecutar alguna función en cada elemento de una array, el resultado es el mismo pero, cuando utilizamos métodos declarativos es mucho más fácil de leer y entender nuestro código a primera vista.


