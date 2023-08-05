## React
**Create react app without create-react-app command**
```html
<script src="https://unpkg.com/react@16/umd/react.development.js"></script>
<script src="https://unpkg.com/react-dom@16/umd/react-dom.development.js"></script>
<script src="https://unpkg.com/babel-standalone@6.26.0/babel.js"></script>
```
**step - 2:**
```js
function App() {
	return <h1>hello world</h1>;
}

ReactDOM.render(<App />, document.getElementById("root"));
```
