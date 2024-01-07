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
- How can I add type safety when working with this framework? type safety with html elements using htmx for example, might not be needed
- What do I do about caching? Is this some middleware I'll be creating, Hono only has one for Deno and Cloudflare

## TODO
These the points I need to research futher.
- [ ] Research what router to use and try and implement it.
    - These, [1](https://www.youtube.com/watch?v=fYQftxb9xTg) & [2](https://www.youtube.com/watch?v=DrP8gIpwkUg&t=965s) videos put it well into perspective, what is it that I want to solve with a file based router and how well does it play along with HTMX. The purpose of this framework is to work with HTMX and HTMX needs rest end points to shine. So if each file represents a route I would then most likely have a equivalent file with all of the CRUD-operation (a controller) end points with the same route name. Having a file based routing system might lead to a redundant DX so a pseudo filed based routing, like creating maps and files and then naming the routes after them? I'm mimicking file based routing but in actuality creating a page based router.
    - **Conclusion:** Page based routing with file based named routing, controllers for handling api end points
- [ ] Database integration (SQLite)
    - How can I ensure that the db is persisted throughout the application? Do I need to create a context file that keeps track of it? Like with bindings and then call it via `c.var.db` or `c.env.db`
- [ ] Type safety when working with HTML elements using htmx
    - I actually found this when checking out the [BETH stack](https://github.com/ethanniser/beth-b2b-saas/blob/main/src/types/htmx.d.ts), might add some improvements to this when adding dynamic
- [ ] Research on how to provide Island architecture to web-component or client side scripting with out having to re-implement <client-islands src="my-components" client:load></client-islands> again.
    - I read something in [Islands.js](https://islandjs.dev/en/guide/islands-arch) might need to write something with the build times or run times.
- [ ] Deployment
    - Vite
    
## Folder path Ideas
Display of how the file structure could look like based on what type of routing is used
*File based routing:*
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
│   ├── controllers // api controllers.
│   │   └── login.tsx // login api controller
│   └── server.ts // server entry file
├── package.json
├── public
│   └── favicon.ico
├── tsconfig.json
└── vite.config.ts
```

*Normal routing:*
```txt
.
├── app
│   ├── client //All client side scripting files
│   │   ├── client.ts // client entry file
│   │   ├── Island.ts // Astro Client Directives
│   │   └── islands
│   │       └── counter.ts // web components
│   └── server
│       ├── components //Common reusable component
│       │   ├── Button.tsx
│       │   └── Navbar.tsx
│       ├── controllers // api controllers.
│       │   └── login.tsx // login api controller
│       ├── pages // each page would be represented as it's own route end point along with it's own HTMX operations + Web components 
│       │   ├── _404.tsx // not found page
│       │   ├── _error.tsx // error page
│       │   ├── _layout.tsx // layout template for JSXRender
│       │   ├── about.tsx // matches `/about/:name` and renders a html file that way
│       │   └── index.tsx // this would have the `/` end point for example i.e our homepage
│       └── server.tsx // server entry file with endpoint for each page
├── package.json
├── public
│   └── favicon.ico
├── tsconfig.json
└── vite.config.ts
```