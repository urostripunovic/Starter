# Starter

The starter mono repo of the SSR framework, no name for it yet

**Tech stack:** Hono, HTMX, Tailwind CSS, Web Components, Vite

Inspo on how to build the Fullstack SSR framework: [The BETH Stack](https://github.com/Phillip-England/bun-mpa-web-template/tree/main), [Hono's SonikJS](https://github.com/sonikjs) and [Deno land's Fresh](https://github.com/denoland/fresh)

## Rough problem statement
These are some pointers that I would need to look into and brake down even further as to better understand what I need to research, what problems I'm solving and what I would actually need to implement.

-   How am I structuring the folders? A central App folder with a client and server folder inside it? or two folders, one server and one client?
-   Routing. Do I want file based routing like [SvelteKit](https://kit.svelte.dev/docs/routing) or do I want [normal routing](https://hono.dev/api/routing#grouping)
-   How can I compile the Astro Client Directive's into a web component? Instead of doing it like [this](https://github.com/urostripunovic/hono-htmx-webcomponents/blob/main/src/index.tsx#L89-L100), maybe compile a JSX component to a web components so that <MyComponent client:load /> compiles to <client-islands src="my-component" client:load> <my-component></my-component> </client-islands>. And how can I work with VanillaJS or Vite so that this can work? Look into this, might not be possible
- What kind of database will I need to integrate? Do I have BetterSQlite3 pre-installed or do you use Cloudflare's D1 database? Either way both can be used and reference inside [Hono's bindings](https://developers.cloudflare.com/d1/examples/d1-and-hono/). See which one to use, if cloud flare is available for free then great! How would I use something like supabase or firebase for authentication on the server?
- How would I deploy this application? [Fly](https://fly.io/) seems to be an option but I would be required to learn more about docker which is nice but there is also a [vercel adapter](https://hono.dev/getting-started/vercel) with Hono.

## TODO
These the points I need to research futher.
- [ ] Research what router to use and try and implement it
- [ ] Research on how to provide Island architecture to web-component or client side scripting with out having to re-implement <client-islands src="" client:x></client-islands> again.
- [ ] Database integration + database with auth integration
- [ ] Deployment

## Folder path Ideas

File based routing:
```txt
.
├── app
│   ├── lib // reusable components
│   │   ├── Button.tsx
│   │   └── Navbar.tsx
│   ├── client.ts // client entry file
│   ├── Island.ts // Astro Client Directives
│   ├── islands
│   │   └── counter.ts // island component
│   ├── routes
│   │   ├── _404.tsx // not found page
│   │   ├── _error.tsx // error page
│   │   ├── _layout.tsx // layout template
│   │   ├── about
│   │   │   └── [name].tsx // matches `/about/:name`
│   │   └── index.tsx // matches `/`
│   └── server.ts // server entry file
├── package.json
├── public
│   └── favicon.ico
├── tsconfig.json
└── vite.config.ts
```

Normal routing:
```txt
.
├── app
│   ├── client
│   │   ├── client.ts // client entry file
│   │   ├── Island.ts // Astro Client Directives
│   │   └── islands
│   │       └── counter.ts // web components
│   └── server
│       ├── components //JSX reusable component
│       │   ├── Button.tsx
│       │   └── Navbar.tsx
│       ├── pages
│       │   ├── \_404.tsx // not found page
│       │   ├── \_error.tsx // error page
│       │   ├── \_layout.tsx // layout template
│       │   ├── about
│       │   │   └── [name].tsx // matches `/about/:name`
│       │   └── index.tsx // matches `/`
│       └── server.tsx // server entry file with endpoint for each page
├── package.json
├── public
│   └── favicon.ico
├── tsconfig.json
└── vite.config.ts
```