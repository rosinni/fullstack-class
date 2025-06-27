# Guía para Principiantes: Uso de React Router


Cuando construyes una aplicación en React, todo vive dentro de una sola página. Aunque visualmente parece que hay diferentes pantallas (como "Inicio", "Acerca de", "Contacto"), React no recarga la página completa cuando el usuario navega. En su lugar, muestra u oculta componentes según la dirección (URL) en la que estés.

React Router es una herramienta que permite cambiar de "pantalla" dentro de una aplicación React, dependiendo de la dirección (por ejemplo, `/` o `/demo`), sin recargar la página.


## ¿Cómo funciona React Router?

React Router actúa como un sistema de navegación que dice:

- "Si el usuario está en la dirección `/`, muéstrale el componente Home".
- "Si el usuario está en `/demo`, muéstrale el componente Demo".

### Forma clásica (más sencilla)

```jsx
import { BrowserRouter, Routes, Route } from "react-router-dom";
import Home from "./pages/Home";
import Demo from "./pages/Demo";

<BrowserRouter>
  <Routes>
    <Route path="/" element={<Home />} />
    <Route path="/demo" element={<Demo />} />
  </Routes>
</BrowserRouter>
```

### Forma moderna (más organizada)

```jsx
import {
  createBrowserRouter,
  createRoutesFromElements,
  Route,
} from "react-router-dom";
import { Layout } from "./pages/Layout";
import { Home } from "./pages/Home";
import { Demo } from "./pages/Demo";

export const router = createBrowserRouter(
  createRoutesFromElements(
    <Route path="/" element={<Layout />} errorElement={<h1>Not found!</h1>}>
      <Route path="/" element={<Home />} />
      <Route path="/demo" element={<Demo />} />
    </Route>
  )
);
```

En el punto de entrada (`main.jsx`):

```jsx
import { RouterProvider } from "react-router-dom";
import { router } from "./routes";

<RouterProvider router={router} />
```

---

## ¿Qué es el componente `Layout`?

El componente `Layout` funciona como una plantilla base que se repite en todas las páginas. Por ejemplo, si tu aplicación tiene un menú (navbar) arriba y un pie de página (footer) abajo, puedes colocarlos dentro del componente `Layout`.

Dentro de ese `Layout` hay un elemento especial llamado `<Outlet />`. Es un marcador de posición donde se va a renderizar el contenido de la página actual.

### Ejemplo de Layout.jsx

```jsx
import React from "react";
import { Outlet } from "react-router-dom";
import Navbar from "../components/Navbar";
import Footer from "../components/Footer";

export const Layout = () => {
  return (
    <>
      <Navbar />
      <div className="container mt-4">
        <Outlet />
      </div>
      <Footer />
    </>
  );
};
```



## ¿Qué hace `<Outlet />`?

`<Outlet />` le indica a React Router dónde colocar el componente correspondiente a la ruta actual.

- Si estás en la ruta `/`, se mostrará `<Home />`.
- Si estás en la ruta `/demo`, se mostrará `<Demo />`.



## Estructura visual de la aplicación

```
<Layout />
├── <Navbar />
├── <Outlet />  ← Aquí va el contenido de la ruta actual
└── <Footer />
```



React Router permite mostrar diferentes páginas en una aplicación React sin recargar el navegador. El componente `Layout` ayuda a mantener una estructura común entre todas las páginas, y `<Outlet />` es el lugar donde se insertan las vistas específicas según la ruta.

Este enfoque mejora la organización del proyecto y permite que tu aplicación escale de forma más limpia y profesional.
