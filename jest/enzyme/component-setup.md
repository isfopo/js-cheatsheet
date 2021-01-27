# Component Setup Function

This is a helpful setup function to setup a component for a test:

```javascript
const defaultProps = { 
  // whatever props you want to be the default for your tests
}

/**
 * change the component in the function manually
 * @param {object} props - unique to this instance
 * @returns {ShallowWrapper} - a shallow instance of the given component
 */
const setup = ( props = {} ) => {
  const setupProps = { ...defaultProps, ...props }
  return shallow(<GuessedWords {...setupProps } />)
}
```
