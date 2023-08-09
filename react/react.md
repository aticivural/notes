#### What is **npx**

`npx` is a Node.js command-line tool which became available with `npm@5.2.0`. It enables `npm` to e**X**ecute command-line Node.js tools without having them to be installed globally.

#### configure proxy config

```shell
code ~/.npmrc
proxy=your-proxy-url
https-proxy=your-proxy-url
```

#### create react project

```shell
npx create-react-app <project-name> 
```

#### starts the app

```shell
npm start
```

Runs the app in the development mode.  
Open [http://localhost:3000](http://localhost:3000/) to view it in your browser.

The page will reload when you make changes.  
You may also see any lint errors in the console.

#### tests the app

```shell
 npm test
```

Launches the test runner in the interactive watch mode.  
See the section about [running tests](https://facebook.github.io/create-react-app/docs/running-tests) for more information.

#### installs the dependencies

```shell
npm install
```

npm will install all the dependencies listed in the package.json

---

### Some Useful Tips

* jsx de class yerine className kullanılıyor

* Custom components büyük harfle başlamalı

* Component lar arasında data transferi props aracılığıyla gerçekleşir

* Componentları iç içe kullanırken, **props.children** kullanılıyor, styling içinde **props.className** dıştaki component ın **className** ine set etmek gerekiyor

* state leri hook lar üzerinden update edersek componentler yeniden render edilir ve değişiklik yansır

* hooklar **use** ile başlar. **useState** vs.

* hook lar react library den gelir

* hook lar function içinde initialize edilmeli, başka yerde değil

```javascript
//sample of usage state hook
cons [title, setTitle] = useState(props.title)
```

> css için css modules kullanılabilir

### useRef

dom objelerine erişmek için kullanılıyor, html elementin **ref** property alanına set ediliyor 

### useEffect

The `useEffect` Hook allows you to perform side effects in your components.

Some examples of side effects are: fetching data, directly updating the DOM, and timers.

`useEffect` accepts two arguments. The second argument is optional.

`useEffect(<function>, <dependency>)`

### useReducer

```javascript
const [state, dispatch] = useReducer(reducer, initialArg, init);
```

`useReducer` is usually preferable to `useState` when you have complex state logic that involves multiple sub-values or when the next state depends on the previous one. `useReducer` also lets you optimize performance for components that trigger deep updates because [you can pass `dispatch` down instead of callbacks](https://reactjs.org/docs/hooks-faq.html#how-to-avoid-passing-callbacks-down).

> useReducer sonradan yine bak

### React Context

Context provides a way to pass data through the component tree without having to pass props down manually at every level.

In a typical React application, data is passed top-down (parent to child) via props, but such usage can be cumbersome for certain types of props (e.g. locale preference, UI theme) that are required by many components within an application. Context provides a way to share values like these between components without having to explicitly pass a prop through every level of the tree.

```javascript
// define the context with the initial value
const AuthContext = React.createContext({
  isLoggedIn: false
});
```

```javascript
// define the context provider which manipulates the context values
<AuthContext.Provider
      value={{
        isLoggedIn: isLoggedIn,
        onLogout: logoutHandler,
        onLogin: loginHandler,
      }}
    >
```

```javascript
// define the context hook to listen changing in the context
const ctx = useContext(AuthContext);
```

### Forward Refs

Ref forwarding is a technique for automatically passing a [ref](https://reactjs.org/docs/refs-and-the-dom.html) through a component to one of its children. This is typically not necessary for most components in the application. However, it can be useful for some kinds of components, especially in reusable component libraries. The most common scenarios are described below.

### useImperativeHandle

`useImperativeHandle` customizes the instance value that is exposed to parent components when using `ref`. As always, imperative code using refs should be avoided in most cases. `useImperativeHandle` should be used with [`forwardRef`](https://reactjs.org/docs/react-api.html#reactforwardref)
