# 🧭 Guía rápida — Métodos modernos de arrays

> Tu referencia mientras hacés los ejercicios. Volvé acá cuando dudes.
> Con el tiempo no la vas a necesitar más. Al principio, mirala todas las veces que haga falta — no es trampa, es estudiar.

---

## 🎯 La gran idea

Antes había **una sola forma** de procesar arrays: el `for` clásico.

```ts
const numeros = [1, 2, 3, 4];
const dobles = [];
for (let i = 0; i < numeros.length; i++) {
  dobles.push(numeros[i] * 2);
}
```

Hoy tenés métodos que **describen tu intención** en vez de detallar el "cómo":

```ts
const dobles = numeros.map(n => n * 2);
```

El segundo dice **qué querés**. El primero dice **cómo iterar**. La diferencia es enorme.

Las tres operaciones nucleares cubren el 90% de lo que vas a hacer con datos:

| Quiero... | Uso |
|-----------|-----|
| Transformar cada elemento en otra cosa | `map` |
| Quedarme solo con algunos elementos | `filter` |
| Combinar todo en un único valor | `reduce` |

---

## 1. `map` — transformar

**Modelo mental**: tenés una lista de N elementos. Querés una lista de N elementos (mismo tamaño) donde cada uno fue transformado.

```ts
const numeros = [1, 2, 3];
const dobles = numeros.map(n => n * 2);
// [2, 4, 6]
```

**Estructura**: `array.map(elemento => nuevoValor)`

**Casos típicos**:

```ts
// Calcular con impuesto
const conIva = productos.map(p => ({ ...p, precioFinal: p.precio * 1.21 }));

// Extraer una propiedad
const nombres = usuarios.map(u => u.nombre);
```

**Regla**: el array resultante **siempre tiene el mismo length que el original**. Si necesitás devolver más o menos elementos, `map` no es la herramienta.

---

## 2. `filter` — seleccionar

**Modelo mental**: tenés N elementos. Querés quedarte solo con los que cumplen una condición. Resultado tiene N o menos.

```ts
const numeros = [1, 2, 3, 4, 5];
const pares = numeros.filter(n => n % 2 === 0);
// [2, 4]
```

**Estructura**: `array.filter(elemento => condicion)`

La función debe devolver `true` (se queda) o `false` (se va).

**Casos típicos**:

```ts
const mayores = usuarios.filter(u => u.edad >= 18);

// Filtros compuestos
const disponiblesYBaratos = productos.filter(p => p.stock > 0 && p.precio < 100);
```

**Regla**: la función debe devolver un booleano. Si devolvés un número o un string, JavaScript intenta convertirlo a booleano (1 = true, 0 = false, "" = false) y eso es fuente de bugs raros. Sé explícito con la condición.

---

## 3. `reduce` — combinar

**Modelo mental**: tenés N elementos. Querés un único valor final que los resume. Ese valor puede ser un número, un string, un objeto, otro array — lo que sea.

```ts
const numeros = [1, 2, 3, 4];
const suma = numeros.reduce((acumulador, actual) => acumulador + actual, 0);
// 10
```

**Estructura**: `array.reduce((acumulador, actual) => nuevoAcumulador, valorInicial)`

Esto es lo que más cuesta al principio. Desglosémoslo:

- **acumulador**: el valor que se va construyendo. Empieza siendo el "valor inicial".
- **actual**: el elemento del array que estamos viendo en este paso.
- **valorInicial**: con qué empezar (`0` para sumas, `[]` para construir arrays, `{}` para construir objetos).

Imaginate que `reduce` recorre el array uno por uno, y en cada paso decís "dame mi acumulador actualizado con este elemento".

**Ejemplo paso a paso** — sumar `[1, 2, 3]` empezando con `0`:

| Paso | acumulador | actual | resultado |
|------|-----------|--------|-----------|
| 1 | 0 (inicial) | 1 | 0 + 1 = 1 |
| 2 | 1 | 2 | 1 + 2 = 3 |
| 3 | 3 | 3 | 3 + 3 = 6 |

Final: **6**.

**Casos típicos**:

```ts
// Contar ocurrencias
const conteo = letras.reduce((acc, letra) => {
  acc[letra] = (acc[letra] || 0) + 1;
  return acc;
}, {});

// Encontrar el máximo
const max = numeros.reduce((mayor, n) => n > mayor ? n : mayor, -Infinity);
```

**Regla**: el acumulador es lo que **devolvés**. Si no retornás explícitamente, `reduce` te va a fallar en el siguiente paso porque va a recibir `undefined`. Siempre `return`.

---

## 🔍 Métodos complementarios

### `find` — encontrar el primero que cumple

Como `filter` pero devuelve **un solo elemento**: el primero que matchea. Si nada matchea, devuelve `undefined`.

```ts
const ana = usuarios.find(u => u.nombre === "Ana");
```

Cuándo usarlo: cuando sabés que querés un solo resultado.

### `some` — ¿hay alguno que cumple?

Devuelve `true` o `false`. ¿Existe al menos un elemento que cumple la condición?

```ts
const hayAdmin = usuarios.some(u => u.rol === "admin");
```

### `every` — ¿todos cumplen?

Devuelve `true` solo si **todos** cumplen.

```ts
const todosMayores = usuarios.every(u => u.edad >= 18);
```

### `sort` — ordenar (con cuidado)

```ts
// Números: necesitás función comparadora
[3, 1, 2].sort((a, b) => a - b); // ascendente: [1, 2, 3]
[3, 1, 2].sort((a, b) => b - a); // descendente: [3, 2, 1]

// Por propiedad
usuarios.sort((a, b) => a.edad - b.edad);
```

**⚠️ Trampa**: `sort` **modifica el array original**. Si querés inmutabilidad, hacé una copia primero: `[...arr].sort(...)`.

---

## 🔗 Combinando métodos (chaining)

La verdadera potencia viene cuando los encadenás. Se lee de arriba hacia abajo como una línea de montaje:

```ts
// Total a pagar de productos en stock con 10% de descuento
const total = productos
  .filter(p => p.stock > 0)                  // 1. solo los disponibles
  .map(p => p.precio * 0.9)                  // 2. aplicá descuento
  .reduce((acc, precio) => acc + precio, 0); // 3. sumá todo
```

Cada paso devuelve un nuevo array que alimenta al siguiente. Esto es **pensamiento funcional puro**.

---

## 🌳 Árbol de decisión

¿Qué necesitás hacer?

- **¿Transformar cada uno?** → `map`
- **¿Quedarte con algunos?** → `filter`
- **¿Llegar a un solo valor (suma, máximo, conteo, agrupación)?** → `reduce`
- **¿Encontrar uno específico?** → `find`
- **¿Saber si existe alguno?** → `some`
- **¿Verificar que todos cumplen?** → `every`
- **¿Ordenar?** → `sort` (recordá la mutación)
- **¿Solo ejecutar algo por cada uno sin transformar?** → `forEach`

---

## ❌ Errores comunes (todos los cometemos)

### 1. Olvidar el `return` cuando usás llaves

```ts
// MAL - no devuelve nada
numeros.map(n => { n * 2 });
// [undefined, undefined, undefined]

// BIEN - return explícito
numeros.map(n => { return n * 2 });

// MEJOR - sin llaves, return implícito
numeros.map(n => n * 2);
```

### 2. Olvidar el valor inicial en `reduce`

```ts
// PELIGROSO - sin valor inicial
[].reduce((acc, n) => acc + n); // Error: Reduce of empty array with no initial value

// SEGURO - con valor inicial
[].reduce((acc, n) => acc + n, 0); // 0
```

### 3. Confundir `filter` con `find`

```ts
// filter devuelve un array, aunque haya un solo resultado
const resultado = usuarios.filter(u => u.id === 5);
// [{ id: 5, ... }]  ← un array de un elemento

// find devuelve el objeto directamente
const usuario = usuarios.find(u => u.id === 5);
// { id: 5, ... }  ← el objeto
```

### 4. Usar `map` cuando no necesitás el resultado

Si solo querés ejecutar algo por cada elemento (loggear, mandar emails), usá `forEach`. `map` crea un array nuevo que después tirás.

```ts
// MAL
usuarios.map(u => console.log(u.nombre));

// BIEN
usuarios.forEach(u => console.log(u.nombre));
```

### 5. `=` vs `===`

```ts
// MAL - asignación, siempre da verdadero
usuarios.filter(u => u.edad = 18);

// BIEN - comparación estricta
usuarios.filter(u => u.edad === 18);
```

---

## 📋 Cheat sheet final

```ts
arr.map(x => transformar(x))            // mismo length, valores transformados
arr.filter(x => condicion(x))           // length <= original, mismos valores
arr.reduce((acc, x) => combinar, init)  // un solo valor de salida
arr.find(x => condicion(x))             // primer matching o undefined
arr.some(x => condicion(x))             // true si al menos uno cumple
arr.every(x => condicion(x))            // true si todos cumplen
arr.forEach(x => efecto(x))             // ejecutar algo, no devuelve nada útil
[...arr].sort((a, b) => a - b)          // ordenar (copia, no muta)
```

---

## 💡 El insight más importante

Estos métodos no son **trucos de sintaxis**. Son la introducción al **pensamiento funcional**, que es la base de:

- React (los componentes son funciones puras que transforman datos en UI)
- SQL (selecciona, filtra, agrupa — es lo mismo)
- Pipelines de datos
- Programación reactiva

Cuando entiendas que casi todo procesamiento de datos en cualquier lenguaje moderno es alguna combinación de **transformar / filtrar / agregar**, vas a empezar a reconocer el mismo patrón en todas partes. Ese momento es un cambio de nivel.

---

_Ahora a los ejercicios. Recordá: AI como tutora, no como escritora. Lo que escribís, lo escribís vos._
