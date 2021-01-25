# [Data Fetching with Next.js](https://nextjs.org/docs/basic-features/data-fetching)

## ```getStaticProps``` / ```getStaticPaths``` - Static Generation

Fetches data at build time and injects into re-rendered static HTML. Data can be fetched again in regular intervals using the ```revalidate``` key and the number of seconds you want it to run the function again. This function is run on the server on a node environment.

You should use ```getStaticProps``` if:

- The data required to render the page is available at build time ahead of a user’s request.
- The data comes from a headless CMS.
- The data can be publicly cached (not user-specific).
- The page must be pre-rendered (for SEO) and be very fast — getStaticProps generates HTML and JSON files, both of which can be cached by a CDN for performance.

```javascript
import { GetStaticProps } from 'next'
import { InferGetStaticPropsType } from 'next'

type Post = {
  author: string
  content: string
}

export const getStaticProps: GetStaticProps = async (context) => {

  // perform logic or data fetching here
  const res = await fetch(`https://.../data`)
  const posts: Post[] = await res.json()

  if (!posts) {
    return {
      notFound: true,
    }
  }

  return {
    props: { // will be passed to the page component as props
      posts,
      revalidate: 10 // function will run again and gather updated data every 10 seconds
    }, 
  }
}

function Blog({ posts }: InferGetStaticPropsType<typeof getStaticProps>) {
  // will resolve posts to type Post[]
}
```

If the page you want to use ```getStaticProps``` on is dynamic (meaning the filename is surrounded by square brackets, like ```[id].js```) you must also have a ```getStaticPaths``` function to tell Next whish paths to expect.

```javascript
import { GetStaticPaths } from 'next'

export const getStaticPaths: GetStaticPaths = async () => {
  return {
    paths: [
      { params: { id: '1' } },
      { params: { id: '2' } }
    ],
    fallback: ...
  }
}
```

Then Next.js will statically generate ```posts/1``` and ```posts/2``` at build time using the page component in ```pages/posts/[id].js```.

If ```fallback``` is ```false```, then any paths not returned by ```getStaticPaths``` will result in a 404 page. You can do this if you have a small number of paths to pre-render - so they are all statically generated during build time. It’s also useful when the new pages are not added often. If you add more items to the data source and need to render the new pages, you’d need to run the build again.

If ```fallback``` is ```true```, then the behavior of ```getStaticProps``` changes:

- The paths returned from ```getStaticPaths``` will be rendered to HTML at build time by ```getStaticProps```.
- The paths that have **not** been generated at build time will not result in a 404 page. Instead, Next.js will serve a “fallback” version of the page on the first request to such a path (see “Fallback pages” below for details).
- In the background, Next.js will statically generate the requested path HTML and JSON. This includes running ```getStaticProps```.
- When that’s done, the browser receives the JSON for the generated path. This will be used to automatically render the page with the required props. From the user’s perspective, the page will be swapped from the fallback page to the full page.
- At the same time, Next.js adds this path to the list of pre-rendered pages. Subsequent requests to the same path will serve the generated page, just like other pages pre-rendered at build time.

## ```getServerSideProps``` - Server-side Rendering

You should use ```getServerSideProps``` only if you need to pre-render a page whose data must be fetched at request time. Time to first byte (TTFB) will be slower than ```getStaticProps``` because the server must compute the result on every request, and the result cannot be cached by a CDN without extra configuration.

Note: You should not use ```fetch()``` to call an API route in ```getServerSideProps```. Instead, directly import the logic used inside your API route. You may need to slightly refactor your code for this approach. Fetching from an external API is fine!

```javascript
import { GetServerSideProps } from 'next'
import { InferGetServerSidePropsType } from 'next'

export const getServerSideProps: GetServerSideProps = async (context) => {
  // Fetch data from external API
  const res = await fetch(`https://.../data`)
  const data = await res.json()

  // Pass data to the page via props
  return { props: { data } }
}

function Page({ data }: InferGetServerSidePropsType<typeof getServerSideProps>) {
  // will resolve posts to type Data
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
