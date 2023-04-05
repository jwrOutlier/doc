# 路由

## react-router@5

* 基本使用

```jsx
import { BrowserRouter, Switch, Route, Link } from 'react-router-dom';

function Home() {
  return <h1>Home</h1>;
}

function About() {
  return <h1>About</h1>;
}

function NoFound() {
  return <h1>NoFound</h1>;
}

export default function App() {
  return (
    <BrowserRouter>
      <ul>
        <li>
          <Link to="/">Home</Link>
        </li>
        <li>
          <Link to="/about">About</Link>
        </li>
        <li>
          <Link to="/xxxx">NotFound</Link>
        </li>
      </ul>
      <hr />
      <Switch>
        <Route exact path="/">
          <Home />
        </Route>
        <Route path="/about">
          <About />
        </Route>
        <Route path="*">
          <NoFound />
        </Route>
      </Switch>
    </BrowserRouter>
  );
}
```

> exact 精准匹配  如果不加的话 `/` 只会匹配到从上往下的第一个
> 将 `path="*"` 则会匹配没有的路由

* 动态路由匹配

```jsx
import {
  BrowserRouter,
  Switch,
  Route,
  Link,
  useParams,
} from 'react-router-dom';

export default function App() {
  return (
    <BrowserRouter>
      <div>
        <h2>Accounts</h2>
        <ul>
          <li>
            <Link to="/netflix">Netflix</Link>
          </li>
          <li>
            <Link to="/zillow-group">Zillow Group</Link>
          </li>
          <li>
            <Link to="/yahoo">Yahoo</Link>
          </li>
          <li>
            <Link to="/modus-create">Modus Create</Link>
          </li>
        </ul>
        <Switch>
          <Route path="/:id" children={<Child />} />
        </Switch>
      </div>
    </BrowserRouter>
  );
}


function Child() {
  let { id } = useParams();

  return (
    <div>
      <h3>ID: {id}</h3>
    </div>
  );
}
```

> `useParams()` 用来获取url上面的参数

* 嵌套路由

