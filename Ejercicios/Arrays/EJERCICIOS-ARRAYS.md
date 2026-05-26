# 🔁 Ejercicios — Métodos modernos de arrays

> 20 ejercicios graduados de menor a mayor dificultad.
> Hacelos en orden. No saltees, aunque algunos parezcan fáciles.
> El objetivo no es resolverlos: es entender **cuándo** usar cada método sin pensarlo.

## Reglas mientras los hago

1. **Sin AI escribiendo código.** Podés pedir explicaciones, pero el código lo escribís vos.
2. Cada ejercicio en su propio archivo: `01-doblar.ts`, `02-mayusculas.ts`, etc.
3. Después de resolverlo, escribí 1 comentario al final: `// Aprendí: ...`
4. Si un ejercicio te lleva más de 30 minutos, parate, pedí explicación del concepto, volvé.

## Setup rápido

```bash
mkdir ejercicios-arrays && cd ejercicios-arrays
npm init -y
npm install -D typescript ts-node @types/node
npx tsc --init
```

Después corrés cada uno con `npx ts-node 01-doblar.ts`.

---

## 🟢 Nivel básico — `map`

### 1. Doblar
Dado `[1, 2, 3, 4, 5]`, devolvé `[2, 4, 6, 8, 10]`.

### 2. Mayúsculas
Dado `["ana", "juan", "pedro"]`, devolvé `["ANA", "JUAN", "PEDRO"]`.

### 3. Extraer propiedad
Dado:
```ts
const usuarios = [
  { nombre: "Ana", edad: 25 },
  { nombre: "Juan", edad: 30 },
  { nombre: "Pedro", edad: 22 }
];
```
Devolvé un array solo con los nombres: `["Ana", "Juan", "Pedro"]`.

### 4. Calcular con IVA
Dado:
```ts
const productos = [
  { nombre: "pan", precio: 100 },
  { nombre: "leche", precio: 200 }
];
```
Devolvé el mismo array pero con un campo nuevo `precioConIva` (IVA 21%).

---

## 🟢 Nivel básico — `filter`

### 5. Pares
Dado `[1, 2, 3, 4, 5, 6, 7, 8]`, devolvé solo los pares.

### 6. Palabras largas
Dado `["hola", "programación", "ts", "javascript", "ok"]`, devolvé solo las de más de 5 letras.

### 7. Mayores de edad
Dado el array de `usuarios` del ejercicio 3, devolvé solo los mayores de 24 años.

### 8. Filtro compuesto
Dado:
```ts
const productos = [
  { nombre: "pan", precio: 50, stock: 10 },
  { nombre: "leche", precio: 200, stock: 0 },
  { nombre: "queso", precio: 80, stock: 5 }
];
```
Devolvé los productos con stock > 0 y precio < 100.

---

## 🟡 Nivel intermedio — `reduce`

### 9. Sumar
Dado `[10, 20, 30, 40]`, devolvé la suma total.

### 10. Máximo
Dado `[3, 7, 2, 9, 1, 5]`, devolvé el número más grande **sin usar `Math.max`**.

### 11. Contar ocurrencias
Dado `["a", "b", "a", "c", "b", "a"]`, devolvé `{ a: 3, b: 2, c: 1 }`.

### 12. Agrupar por propiedad
Dado:
```ts
const personas = [
  { nombre: "Ana", pais: "Argentina" },
  { nombre: "Juan", pais: "Chile" },
  { nombre: "Pedro", pais: "Argentina" },
  { nombre: "María", pais: "Chile" }
];
```
Devolvé `{ Argentina: [...], Chile: [...] }`.

---

## 🟡 Nivel intermedio — combinaciones

### 13. Filtrar y mapear
Dado los `productos` del ejercicio 8, devolvé solo los nombres de los productos con stock > 0.

### 14. Total del carrito
Dado:
```ts
const carrito = [
  { producto: "libro", precio: 500, cantidad: 2 },
  { producto: "pluma", precio: 100, cantidad: 5 },
  { producto: "cuaderno", precio: 300, cantidad: 1 }
];
```
Devolvé el total a pagar.

### 15. Encontrar primero
Dado los `usuarios` del ejercicio 3, devolvé el primer usuario mayor de 25 años. (Pista: hay un método específico para esto.)

### 16. Verificar
Dado los `usuarios` del ejercicio 3, contestá dos preguntas:
- ¿Hay algún usuario menor de 23?
- ¿Son todos mayores de 18?

(Pista: dos métodos distintos.)

---

## 🔴 Nivel real — problemas que vas a ver en la vida

### 17. Top hashtags
Dado un array de tweets:
```ts
const tweets = [
  "Aprendiendo #typescript y #react",
  "Hoy resolví un bug en #typescript",
  "#programacion es lo mejor",
  "Empezando con #react"
];
```
Devolvé un objeto con la frecuencia de cada hashtag, ordenado de más a menos usado.

### 18. Reporte de gastos por categoría
Dado:
```ts
const transacciones = [
  { fecha: "2025-11-01", categoria: "comida", monto: 1500 },
  { fecha: "2025-11-02", categoria: "transporte", monto: 800 },
  { fecha: "2025-11-03", categoria: "comida", monto: 2000 },
  { fecha: "2025-11-04", categoria: "ocio", monto: 3000 },
  { fecha: "2025-11-05", categoria: "transporte", monto: 1200 }
];
```
Devolvé un objeto con el total gastado por categoría.

### 19. Ranking de ventas
Dado un array de ventas con `vendedor` y `monto`, devolvé un ranking de vendedores ordenado por total vendido (de mayor a menor).

```ts
const ventas = [
  { vendedor: "Ana", monto: 1000 },
  { vendedor: "Juan", monto: 500 },
  { vendedor: "Ana", monto: 1500 },
  { vendedor: "Pedro", monto: 800 },
  { vendedor: "Juan", monto: 2000 }
];
```

Resultado esperado: `[{ vendedor: "Juan", total: 2500 }, { vendedor: "Ana", total: 2500 }, { vendedor: "Pedro", total: 800 }]`.

---

## 🏆 Reto final

### 20. Reimplementar `map` desde cero
Escribí tu propia función `miMap` que reciba un array y una función, y devuelva un array nuevo con la función aplicada a cada elemento. **Sin usar `.map()` ni `for...of`.** Solo `for` clásico o `while`.

```ts
function miMap<T, U>(array: T[], fn: (item: T) => U): U[] {
  // tu código acá
}

// debería funcionar igual que .map():
console.log(miMap([1, 2, 3], n => n * 2)); // [2, 4, 6]
```

**Por qué este reto importa**: si entendés cómo está hecho `map` por dentro, no es magia. Y "no es magia" es la diferencia entre un programador y un copista.

---

## ✅ Cuando termines los 20

Pasos:

1. Commit + push a tu repo `ejercicios-arrays`.
2. En tu bitácora del día, anotá:
   - ¿Cuál te costó más?
   - ¿Cuál te pareció más útil para problemas reales?
   - ¿Podés explicar la diferencia entre `map`, `filter` y `reduce` en una oración cada uno, sin mirar?
3. Pedí review: "Acá está mi solución del ejercicio X. ¿Qué cambiaría un senior?"
4. Recién entonces, avanzá al próximo tema.

---

_Cuando los tengas todos hechos, pedíme la siguiente lista (objetos avanzados + recursividad)._
