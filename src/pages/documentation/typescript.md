---
title: TypeScript
date: 10/10/2024
description: Documentation, tools, pages and docs
---
# Resumen del Lenguaje de programacion Typescript

## Types en Typescript
<p>Type permite definir como será una estructura de datos o un tipo de datos.</p>

```typescript
    type HexadecimalColor = `#${string}`    
    const color2: HexadecimalColor = '#0033ff' // manera correcta
    const color = '0033ff' // manera incorrecta, y asi será mas dificil equivocarnos por que marcará error
```

## Union Types
<p>Los union types permiten definir que valores podrá tener un tipo de datos.</p>

```typescript
    type HeroPowerScale = 'local' | 'planetary' | 'galactic' | 1 | 3000 | true
    // el valor del tipo HeroPowerScale solo podrá ser una de las cadenas
    // o valores que hemos definido en el propio tipo
```

## Types y Template Union Types en Typescript
<p>Los types definen los datos y pueden ser asignados al tipo de una propiedad de un objeto, a una variable, a un argumento de una funcion, etc. El union type significa que podremos crear tipos que poseen tipos dentro para definir sus propiedades.</p>

```typescript
    type HeroId = `${string}-${string}-${string}-${string}-${string}`
    type HeroPowerScale = 'local' | 'planetary' | 'galactic'
    type Hero = {
        readonly id?: HeroId,
        name: string,
        age: number,
        isActive?: boolean,
        powerScale?: HeroPowerScale
    }
    let hero: Hero = {
        name: 'Thor',
        age: 1500
    }
    function createHero(hero:Hero): Hero{
        const {name,age} = hero
        return {id:crypto.randomUUID(), name, age, isActive:true}
    }
    const thor = createHero({name: 'Thor', age: 1500})
    thor.powerScale = 'planetary'
    console.log(thor)
```

## Intersection Types en Typescript
<p>Los intersection types permiten unir varios types en un unico type con el operador &.</p>

```typescript
    type HeroId = `${string}-${string}-${string}-${string}-${string}`
    type HeroPowerScale = 'local' | 'planetary' | 'galactic'

    type HeroBasicInfo = {
        name: string,
        age: number
    }
    type HeroProperties = {
        readonly id?: HeroId,
        isActive?: boolean,
        powerScale?: HeroPowerScale
    }
    type Hero = HeroBasicInfo & HeroProperties
    let hero: Hero = {
        name: 'Thor',
        age: 1500
    }
    function createHero(input:HeroBasicInfo): Hero{
        const {name,age} = input
        return {id:crypto.randomUUID(), name, age, isActive:true}
    }
    const thor = createHero({name: 'Thor', age: 1500})
    thor.powerScale = 'planetary'
    console.log(thor)
```

## Type Indexing
<p>El type indexing permite acceder a una propiedad de un objeto y ser usado en cualquier punto del codigo sin tener que acceder al dato completo, solo a la parte deseada.</p>

```typescript
    type HeroProperties = {
        isActive: boolean,
        address: {
            planet: string,
            city:string
        },
        powerScale: number
    }
    const addressHero: HeroProperties['address'] = {
        planet: 'Earth',
        city: 'Madrid'
    }
    const myHero:HeroProperties = {
        isActive: true,
        address: addressHero,
        powerScale: 1000
    }
    console.log(addressHero)
    console.log(myHero)
```

## Arrays, definiciones posibles
<p>Existen varias maneras de definir un array, con el metodo push introducimos un dato en el array.</p>

```typescript
    const languages = [] // manera rapida pero erronea, ya que no tiene tipo y se infiere como never, el push dará error
    const languages: string[] = [] // esta es una manera correcta de declarar y tipar un array de strings
    const languages: Array<string> = [] // esta tambien es correcta pero menos clara
    const languages: string[] | number[] = [] // error comun al tipar un array de valores multiples
    const languages: (string | number)[] = [] //manera correcta de tipar un array de multiples tipos de datos
    const heros: Hero[] = [] // array de un type de datos
    const matrix: string[][] = [] // definicion de matriz de datos de tipo string
    languages.push ('Javascript')
    languages.push(2) // dará error si hemos tipado como strings, pero no dará error en un tipado multiple
```

## Resumen de types y arrays y TUPLAS
<p>Ejemplo de un tablero de juego del tres en raya que condensa todo lo dicho anteriormente. Y añadimos las tuplas, que son arrays con limites fijados de longitud.</p>

```typescript
    type CellValue = 'X' | 'O' | '' // definimos los posibles valores
    type GameBoard = [
        [CellValue, CellValue, CellValue],
        [CellValue, CellValue, CellValue],
        [CellValue, CellValue, CellValue]
    ] // esto es una tupla
    const gameBoard:GameBoard = [
        ['X','','X'],
        ['O','X',''],
        ['X','O','']
    ]
    console.log(gameBoard)
```

## Enums (no existen en javascript)
<p>Pongo primero el ejemplo de javascript y luego la replica y mejora de typescript.</p>

```javascript
    const ERROR_TYPES = {
        NOT_FOUND: 'notFound',
        UNAUTHORIZED: 'unauthorized',
        FORBIDDEN: 'forbidden'
    }
    function mostrarMensaje (tipoDeError){ // aqui tipoDeError es una string
        if(tipoDeError === ERROR_TYPES.NOT_FOUND){
            console.log('No se encuentra el recurso')
        }else if(tipoDeError === ERROR_TYPES.UNAUTHORIZED){
            console.log('No tienes permisos para entrar')
        }else if(tipoDeError === ERROR_TYPES.FORBIDDEN){
            console.log('No puedes entrar aqui')
        }
    }
```

<p>En typescript cada elemento del enum tomará un valor por defecto empezando por 0 y aumentando, 1, 2, etc. Podemos poner nosotros los valores a mano si igualamos cada una de las opciones a lo que deseemos, pe: NOT_FOUND = 'a' o lo que queramos</p>

```typescript
    enum ERROR_TYPES {
        NOT_FOUND,
        UNAUTHORIZED,
        FORBIDDEN
    }
    function mostrarMensaje (tipoDeError: ERROR_TYPES){ // aqui tipoDeError es el valor que coincide con enum, 0, 1, 2 etc.
        if(tipoDeError === ERROR_TYPES.NOT_FOUND){
            console.log('No se encuentra el recurso')
        }else if(tipoDeError === ERROR_TYPES.UNAUTHORIZED){
            console.log('No tienes permisos para entrar')
        }else if(tipoDeError === ERROR_TYPES.FORBIDDEN){
            console.log('No puedes entrar aqui')
        }
    }
```

## Fetch de datos en Typescript
<p>Manera de ejecutar un fetch de una API y manejar los datos de la respuesta.</p>

```typescript
    export{} // con esta linea convertimos el archivo en un module
    // si no queremos añadir esto tendremos que cambiar la extension
    // del archivo a .mts

    const API_URL = "https://api.github.com/search/repositories?q=javascript"

    const response = await fetch(API_URL)

    if(!response.ok){
        throw new Error('Request failed.')
    }

    const data = await response.json()

    const repos = data.items.map( repo => {
        console.log(repo)
    })
```

## Operadores ternarios
<p>En funcion de una condicion inicial realizaran oparaciones dependiendo de si se cumple o no. Puede hacerse tanto para el caso de que se cumpla hacer algo, como para el caso de que no se cumpla hacer otra cosa, pero no hace falta que lleve todas las opciones. Ejemplos:</p>
<p>Ejemplo 1: construccion basica del operador</p>

```typescript
    condición ? expr1 : expr2
```
<p>Ejemplo 2: operador binario</p>

```typescript
    variable ? revision="se cumple"
```
<p>Ejemplo 3: operador ternario</p>

```typescript    
    variable ? revision="se cumple" : revision="no se cumple"
```