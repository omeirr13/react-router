# react-router
in order to use react router, we have to install it
- react-router-dom(dom version of react router)
- react-router-native(react native version)

## Steps to use react router:
- go to index.js and wrap entire application inside Router of our choice, and in our case since we are in the browser we will use the BrowserRouter
```
import { BrowserRouter } from "react-router-dom"
```
- BrowserRouter is a react context.
- We will wrap our entire application with this component, and this component will provide a bunch of information about routing to all other components in application, which will make everything work seamlessly.

## How to define routes?
```
import { Routes, Route } from "react-router-dom";
```
- so we have Routes component, which wrap all individual Route component, which each define a path to a different page in our website
- each of route need to have a path, and an element

```
function App(){
  return (
    <Routes>
      <Route path="/" element={<Home />} />
      <Route path="/books" element={<BookList />} />
    </Routes>
  )
}
```

## Link Components
- Link component is technically an anchor tag, but it will be used in React to automatically swap things in and out our application without refreshing our application.
```
<nav>
  <ul>
    <li><Link to="/">Home</Link></li>
    <li><Link to="/books">Books</Link></li>
  </ul>
</nav>
```

- replace property
```
<Link to="/" replace>Home</Link>
```
- it will replace/overwrite in the history, not add a new page, when we go back it will go back two pages, as it replaced the previous one, could be used when we we login to another page, when going back we dont want to go to previous page.

- reloadDocument
```
<Link to="/" reloadDocument>Home</Link>
```
- normally react just changes content inside the route, but with that its going to reload the entire page


# NavLink
- works just like normal link, but allows us to do specific things with active state of a link
- it has className, style, and children properties
- these properties allow you to actually take in a function, and this function has isActive

```
<NavLink style={({ isActive }) => {
  return isActive ? { color: "red } : {}
}}
```

- if we dont want to do this, by default it will apply a class of active
```
.active {
  color: red;
}
```


## Different types of routers:
### HashRouter
- very similar to BrowserRouter, but instead of storing url in normal url, it stores inside the hashed portion of our url
- now instead of ```localhost:3000/books``` it will be ```localhost:3000/#/books```
- now all of this after # is not part of url, we storing stuff related to url in it
- This can be really useful, if we are working on a shared server, where we cant change url of website, in those scenarios then this is useful.

### HistoryRouter
- it is under the name unstable_HistoryRouter
- it allows us to take control of history element, in react router
- so history elemetn just determines how your entire browser renders out history of all pages ive been on.

### MemoryRouter
- it actually stores everything related to where we've been in memory, and doesnt rely on url bar of browser
- when we click on pages, url never changes, because its stored in memory of our webpage.
- really good when we are running tests that dont connect to browser, so cant use BrowserRouter

### StaticRouter
- specifically for working on the server, designed for SSR(server side rendering)
- When rendering React applications on the server, you need a router that works without relying on browser APIs
```import { StaticRouter } from "react-router-dom/server"```
- StaticRouter doenst allow you to change pages, it specifies a single url, and we do that by specifiying a location

### NativeRouter
```import { NativeRouter } from "react-router-native"```
- only used in react native

# Dynamic Routes
```<Rout path="/books/:id element={<Book /> } />```

## useParams
```const { id } = useParams();``` 
- we can access parameters

## Catch all route
```Route path="*" element={<NotFound /> } />```

# Nested Routes
- makes it easy to group multiple routes together.
```
<Route path="/books">
  <Route path=":id" element={<Book />} />
</Route>
```

- but what will we show in our original "/books" element
- to do that we dont define a path instead we say its our index route, which is a boolean that we set to true
```
<Route path="/books">
  <Route index element={ <BookList /> } />
  <Route path=":id" element={<Book />} />
</Route>
```

### We can also specify layouts using this
```
<Route path="/books" element={<BookLayout /> } />
  <Route index element={ <BookList /> } />
  <Route path=":id" element={<Book />} />
</Route>
```

- But our content wont be rendered inside the layout, unless we define a component called Outlet
- Outlet just renders whatever the current route is.

```
import { Link, Outlet } from "react-router-dom";

export const function BookLayout() {
  return (
  <>
    <Link to="/books/1>Book1</Link>
    <Link to="/books/2>Book2</Link>
    <Link to="/books/3>Book3</Link>
    <Outlet />
  </>
  );
}
```

### What if we dont want a route, but just the layout
- we can wrap a bunch of components that arent necessarily related by url inside same layout
```
<Route element={<BookLayout /> } />
  <Route index element={ <BookList /> } />
  <Route path=":id" element={<Book />} />
</Route>
```


### Context
- inside Outlet we can pass a variable called context
- esentially we can define some values, that we pass down to any single component that is inside this layout route
```<Outlet context={{ hello: "world" }} /> ```

- we can come in the pages, that are rendered inside the layout
```const obj = useOutletContext()```


### useRoutes
- we dont have to set routes up using jsx like this, instead we can use a custom hook and set up everything using javascript.

```
let element = useRoutes([
  {
    path:"/",
    element: <NavLayout />,
    children: [
      {
        index: true,
        element: <Home />
      },
      {
        index: true,
        element: <About />
      },
    ]
  }
])

return (
  {element}
)
```

# Navigate component
```
<Navigate to="/home" />
```

- will automatically navigate, someone whenever we reach certain page, what happens if we dont want to navigate based on a component, oftem times we navigate based on form submissions

# useNavigate hook
```
import navigate from "react-router-dom";
const navigate = useNavigate();
- Link, Navigate, useNavigate hook all take same thing..
- to property, replace attribute, and custom state

useEffect(()=>{
  setTimeout(()=>{
    navigate("/");
  }, 1000);
}, []);
navigate("/");
```

- navigate(-num) will simulate hitting the backbutton num times.


# Search Parameters
## useSearchParams hook
- search params/query params
- it will get the query params in the url
- localhost:3000/filter?age=23&citylahore
```
  const [searchParams,setSearchParams] = useSearchParams();
  console.log(searchParams.get('age');
```
- setSearchParams will allow us to change the query params, and it will change live, will rerender each time i change it.

# Navigation state
- if in Link, NavLink, useNavigate, or Navigate Component we set the state, we can get that state using useLocation hook
- useLocation gives us location object

```
const location = useLocation();
```
- pathname: everything coming after localhost:3000
- search: anything in our search query
- key: a unique value we can use anytime we need to cache things, related to a location, its just a unique id
- hash: in url part that comes after the hash symbol, often called the fragment identifier, commonly used by browsers to navigate to specific places in a page, it does not effect routing
- state: this is where we access state, its not stored anywhere except here


# Private routes
```

import {  Navigate } from "react-router-dom";
import { useAuth } from "../contexts/AuthContext";

export default function PrivateRoute({ children }) {
  const { currentUser } = useAuth();

  return currentUser ? children : <Navigate to="/login" />;
}

Then this for the dashboard element in your app.js: 
<Route path="/"
  element={
    <PrivateRoute>
      <Dashboard />
    </PrivateRoute>
  }
></Route>
```

