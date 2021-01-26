# Example data-test

This is an example of an enzyme test that allows us to make sure that the correct is rendering without any kind of error.

In App.test.js:

```javascript
test('renders without error', () => {
  const wrapper = shallow(<App />);
  const appComponent = wrapper.find("[data-test='component-app']")
  expect(appComponent.length).toBe(1);
});
```

In App.js:

```javascript
function App() {
  return (
    <div className="App" data-test="component-app">

    </div>
  );
}
```

Uses the naming convention ```component-<name-of-component>```
