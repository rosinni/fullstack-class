# ¿Por qué pasar de `fetch().then()` a `async/await` con `try...catch`?

Cuando empiezas a trabajar con llamadas a APIs en JavaScript, `fetch()` es una herramienta muy común. Al principio lo usas con promesas y `.then()`, pero tarde o temprano te encuentras con `async/await` y te preguntas: 

> ¿Por qué cambiar la forma en que ya funciona? Aquí te explico el porqué.

## El enfoque básico: `fetch().then()`

Cuando aprendes a usar `fetch()` por primera vez, probablemente lo haces así:

```javascript
function obtenerUsuario() {
    fetch('https://api.ejemplo.com/user/42')
    .then(response => response.json())
    .then(data => {
        console.log('Datos:', data);
    })
    .catch(error => {
        console.error('Error al obtener los datos:', error);
    });
}
```
Es válido y funciona bien, pero:

- Cuando hay muchas operaciones encadenadas (`then().then().then()`), el código se vuelve más difícil de leer.

- Si mezclas varias promesas o lógica compleja, puede aparecer el llamado **"callback hell"**.

### ¿Qué cambia con async/await?

Es la misma idea, pero usando `async/await`, que permite escribir el código como si fuera **síncrono**, y con manejo de errores más limpio mediante `try...catch`:

```javascript
async function obtenerUsuario() {
  try {
    const response = await fetch('https://api.ejemplo.com/user/42');
    const data = await response.json();
    console.log('Datos:', data);
  } catch (error) {
    console.error('Error al obtener los datos:', error);
  }
}
```

### ¿Por qué es mejor en muchos casos?

| Ventaja                   | Explicación                                                                 |
|---------------------------|------------------------------------------------------------------------------|
| ✅ Más legible             | Parece código secuencial: `const res = await fetch(...)`                    |
| ✅ Mejor manejo de errores | El bloque `try...catch` atrapa errores de red o parsing sin `.catch()`      |
| ✅ Fácil de depurar        | Puedes usar `console.log` o `debugger` línea por línea                       |
| ✅ Ideal para lógica compleja | Si haces múltiples llamadas `fetch`, es más limpio y ordenado          |


#### ¿Entonces se debe evitar `.then()`?

No. `.then()` sigue siendo **válido y útil**, sobre todo para scripts simples o cuando no puedes usar `async/await`.  Pero a medida que el código crece, **`async/await` con `try...catch` se vuelve la forma recomendada**.


## ¿Por qué usar async/await mejora tu código?

Imagina que estás cocinando una cena y necesitas seguir pasos como: hervir agua, cocinar pasta, servir el plato. Si lo haces con `.then()`, sería algo así como dejar notas pegadas por toda la cocina:

```javascript
hervirAgua().then(() => {
  cocinarPasta().then(() => {
    servirPlato().then(() => {
      console.log("Cena lista");
    });
  });
});
````

😩 Resultado: Es difícil de leer y mantener.

Ahora imagina que puedes escribir tu receta paso a paso, de forma clara:

```javascript

async function prepararCena() {
  try {
    await hervirAgua();
    await cocinarPasta();
    await servirPlato();
    console.log("Cena lista");
  } catch (error) {
    console.error("Algo salió mal:", error);
  }
}
````

😌 Resultado: Parece una lista clara de instrucciones.


Usar `fetch()` con `.then()` funciona bien para tareas simples. Pero a medida que tu código crece, `async/await` con `try...catch` te da mayor control, claridad y facilidad de mantenimiento.

La forma en que escribes el código importa tanto como lo que hace el código. Piensa en `async/await` como una forma de hablarle al navegador en frases más naturales y ordenadas.
