# Installing jest and enzyme

Use this npm command:

```shell
npm install --save-dev enzyme jest-enzyme @wojtekmaj/enzyme-adapter-react-17
```

An in ```setupTests.js```:

```javascript
import '@testing-library/jest-dom';
import Enzyme from 'enzyme';
import Adapter from '@wojtekmaj/enzyme-adapter-react-17';

Enzyme.configure({ adapter: new Adapter() });
```

Note: this adaptor is **not** the official adapter for react 17 and may not be fully supported.
