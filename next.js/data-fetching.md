# [Data Fetching with Next.js](https://nextjs.org/docs/basic-features/data-fetching)

## ```getStaticProps``` - Static Generation

Fetches data at build time and injects into re-rendered static HTML.

You should use ```getStaticProps``` if:

- The data required to render the page is available at build time ahead of a user’s request.
- The data comes from a headless CMS.
- The data can be publicly cached (not user-specific).
- The page must be pre-rendered (for SEO) and be very fast — getStaticProps generates HTML and JSON files, both of which can be cached by a CDN for performance.

```javascript
import { GetStaticProps } from 'next'

export const getStaticProps: GetStaticProps = async (context) => {

  // perform logic or data fetching here
  const res = await fetch(`https://.../data`)
  const data = await res.json()

  if (!data) {
    return {
      notFound: true,
    }
  }

  return {
    props: { // will be passed to the page component as props
      data
    }, 
  }
}
```

## SWR - Client-side fetching

The team behind Next.js has created a React hook for data fetching called [SWR](https://swr.now.sh/). We highly recommend it if you’re fetching data on the client side. It handles caching, revalidation, focus tracking, re-fetching on interval, and more. And you can use it like so:

```javascript
import useSWR from 'swr'

function Profile() {
  const { data, error } = useSWR('/api/user', fetch)

  if (error) return <div>failed to load</div>
  if (!data) return <div>loading...</div>
  return <div>hello {data.name}!</div>
}
```
