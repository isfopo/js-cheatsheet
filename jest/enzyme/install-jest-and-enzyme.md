# Installing jest and enzyme

Use this npm command:

```shell
npm install --save-dev enzyme jest-enzyme @wojtekmaj/enzyme-adapter-react-17
```

With the imports of:

```javascript
import Enzyme from 'enzyme';
import Adapter from '@wojtekmaj/enzyme-adapter-react-17';
```

Note: this adaptor is **not** the official adaptor for react 17 and may not be fully supported.
