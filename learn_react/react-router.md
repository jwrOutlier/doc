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
>
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

> `useParams()` 用来获取动态路由的路径参数

* 嵌套路由

```jsx
import {
  BrowserRouter,
  Switch,
  Route,
  Link,
  useParams,
  useRouteMatch,
} from 'react-router-dom';

export default function App() {
  return (
    <BrowserRouter>
      <ul>
        <li>
          <Link to="/">home</Link>
        </li>
        <li>
          <Link to="/topics">Topics</Link>
        </li>
      </ul>

      <hr />

      <Switch>
        <Route exact path="/">
          <Home></Home>
        </Route>
        <Route path="/topics">
          <Topics />
        </Route>
      </Switch>
    </BrowserRouter>
  );
}

function Home() {
  return <p>我是Home组件</p>;
}

function Topics() {
  let { path,url} = useRouteMatch()
  return (
    <div className="topics">
      <h2>topics</h2>
      <ul>
        <li>
          <Link to={url + '/Rendering'}>Rendering with React</Link>
        </li>
        <li>
          <Link to={url + '/Components'}>Components</Link>
        </li>
      </ul>

      <Switch>
        <Route path={path + '/:topicId'}>
          <Topic></Topic>
        </Route>
      </Switch>
    </div>
  );
}


function Topic() {
  let { topicId } = useParams();
  return (
    <div>
      <h3>{topicId}</h3>
    </div>
  );
}
```

> `useRouteMatch()`  用于在函数式组件中获取当前 URL 的匹配信息

* 编程式导航

通过useHistory()来实现

```jsx
import React from 'react';
import { Link, useHistory } from 'react-router-dom';

export default function NavMenu() {
  const router = useHistory();
  const handleClick = () => {
    router.push('/menu3');
  };
  return (
    <div>
      <ul>
        <li>
          <Link to="/menu1">menu1</Link>
        </li>
        <li>
          <Link to="/menu2">menu2</Link>
        </li>
        <li>
          <span onClick={handleClick}>menu3</span>
        </li>
      </ul>
    </div>
  );
}


import { Switch,Route } from 'react-router-dom';
import NavMenu from './components/NavMenu';
export default function App() {
  return (
    <div className="App">
      <NavMenu></NavMenu>
      {/* 显示区域 */}
      <div className="container">
        <Switch>
          <Route path="/menu1" component={Menu1}></Route>
          <Route path="/menu2" component={Menu2}></Route>
          <Route path="/menu3" component={Menu3}></Route>
        </Switch>
      </div>
    </div>
  );
}


function Menu1() {
  return <p>Menu1</p>;
}
function Menu2() {
  return <p>Menu2</p>;
}
function Menu3() {
  return <p>Menu3</p>;
}
```

* 重定向

```jsx
import { Switch, Route, Redirect } from 'react-router-dom';
import NavMenu from './components/NavMenu';
export default function App() {
  return (
    <div className="App">
      <NavMenu></NavMenu>
      {/* 显示区域 */}
      <div className="container">
        <Switch>
          <Route path="/menu1" component={Menu1}></Route>
          <Route path="/menu2" component={Menu2}></Route>
          <Route path="/menu3" component={Menu3}></Route>
          <Redirect from="/" to="/menu1"></Redirect>
        </Switch>
      </div>
    </div>
  );
}

function Menu1() {
  return <p>Menu1</p>;
}
function Menu2() {
  return <p>Menu2</p>;
}
function Menu3() {
  return <p>Menu3</p>;
}
```

* 路由传参方式

1.使用params 传参 详见 动态路由匹配

2.search 参数传参

```jsx
import {
  Switch,
  Route,
  Redirect,
  useHistory,
  useLocation,
} from 'react-router-dom';
import qs from 'query-string';

export default function App() {
  const router = useHistory();
  const handleClick = () => {
    router.push({
      pathname: '/showInfo',
      search: "name=张三"
    });
  };
  return (
    <>
      <button onClick={handleClick}>点击跳转</button>
      <Switch>
        <Route path="/showInfo" component={ShowInfo}></Route>
      </Switch>
    </>
  );
}

function ShowInfo() {
  let location = useLocation()
  let search = qs.parse(location.search);
  return (
    <div>
      <p>{search.name}</p>
    </div>
  );
}
```

> 使用search传参时，在匹配的组件中使用要useLocation()来获取search，并且使用query-string库转换一下

3.state 传参

```jsx
import {
  Switch,
  Route,
  useHistory,
  useLocation,
} from 'react-router-dom';
export default function App() {
  const router = useHistory();
  const handleClick = () => {
    router.push({
      pathname: '/showInfo',
      state: {
        name:'张三'
      }
    });
  };
  return (
    <>
      <button onClick={handleClick}>点击跳转</button>
      <Switch>
        <Route path="/showInfo" component={ShowInfo}></Route>
      </Switch>
    </>
  );
}

function ShowInfo() {
  let { state} = useLocation()
  return (
    <div>
      <p>{state.name}</p>
    </div>
  );
}
```

> state传参时有一个问题：当页面刷新时 state 会丢失数据，解决方法：可以存储在本地

* 路由配置
