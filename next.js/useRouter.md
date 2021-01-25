# [useRouter](https://nextjs.org/docs/api-reference/next/router#userouter)

If you want to access the ```router``` object inside any function component in your app, you can use the ```useRouter``` hook.

```javascript
import { useRouter } from 'next/router'

export default function Component() {
  const router = useRouter()

  // ...
}
```

Uses of the ```router``` object:

- Allows access to the query string (```router.query```)
- Used inside ```useEffect`` to tell if router fields are updated client side and ready to use (```router.isReady```)
- Handle client-side transitions (```router.push(url)```)
- Pre-fetch pages for faster client-side transitions (```router.prefetch```) - the ```Link``` component does the same thing
- Listen to events happening inside the router (```router.events```)
