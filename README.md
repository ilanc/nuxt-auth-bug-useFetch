# bug

## steps to reproduce

```
yarn
yarn dev --open
```

- http://localhost:4001/example
  - will redirect to default NEXTAUTH login screen
  - username: jsmith
  - password: hunter2

## broken

the page is broken on page reload - i.e. when the server-side code handles the `useFetch()`

- `<pre>` = `null`
- see server logs:
  ```log
  [nuxt] [request error] [unhandled] [500] fetch failed (http://localhost/api/auth/session?callbackUrl=http://localhost/example.json)
  ```
- i.e. it is ignoring the `PORT`,`AUTH_ORIGIN`,`NEXTAUTH_URL` env settings and defaulting to an origin of `http://localhost`

## expected

it works as expected if you switch routes client-side and then switch back to the `/example` page:

- http://localhost:4001/blog
- F5
- then click `example` in topnav
- now it works:
- `<pre>` = `{ ... }` i.e. data from [example.json](./public/example.json)

## cause

- [examples.vue](./pages/examples.vue)
  - loads using `useFetch` => [example.json](./public/example.json)
  - `useFetch` server-side is not taking `PORT`,`AUTH_ORIGIN` into account
