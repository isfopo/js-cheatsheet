# Warning for Mocks in Jest

Destructuring React hooks on import will prevent Jest from being able to mock them.

Mocking ```useContext``` will work:

```javascript
import React from "react";

const language = React.useContext(LanguageContext)
```

Mocking ```useContext``` will not work:

```javascript
import { useContext } from "react";

const language = useContext(LanguageContext)
```
