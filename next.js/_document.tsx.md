# _document.tsx

A useful component to use in next.js that allows you to access the pre-rendered page that is sent to the client. ```<Main />``` is where your app with be injected and ```<Header />``` and ```<Footer />``` are just examples of what you can do. ```<Head />``` will be the same for every page.

```javascript
import Document, { Html, Head, Main, NextScript } from 'next/document'
import { Header } from '../components/Header'
import { Footer } from '../components/Footer'

export default class CustomDocument extends Document {
  render() {
    return <Html>
      <Head>
        <meta property="description" content="this is a site" />
      </Head>

      <body>
        <Header />
        <Main />
        <Footer />
      </body>
      <NextScript />
    </Html>
  }
}
```
