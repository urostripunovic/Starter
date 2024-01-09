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
These the points I need to research futher. Keep in mind this is a opinionated fullstack application. This project is divided into two parts one for creating the package with core functionality like Island Architecture, file based routing and maybe database management, and then src folder that has the general file structure of how to use the framework.

### Package
- [ ] Research on how to provide Island architecture to web-component or client side scripting with out having to re-implement `<client-islands src="my-components" client:load></client-islands>` again.
    - I read something in [Islands.js](https://islandjs.dev/en/guide/islands-arch) might need to write something with the build times or run times.
    - Implement the idea I got from [SonikJS](https://github.com/sonikjs/sonik/blob/main/src/client/client.ts). Looks a lot like my implementation of `<client-islands></client-islands>` take inspo from here and see if you can querySelectAll Web Components before they've been loaded in and then process client directives that way.
    - Think of how the naming convention could affect the imports/file reads. I want the web component defined name to match the file name. A standard could be in order like naming a file part1-part2 or Part1Part2, split them and then join them. Either way a naming standard is in order it seems. 
    - If there is no `client:` attribute it will load as client:load
    - [ ] Implement the client directive.
- [ ] Research what router to use and try and implement it.
    - These, [1](https://www.youtube.com/watch?v=fYQftxb9xTg) & [2](https://www.youtube.com/watch?v=DrP8gIpwkUg&t=965s) videos put it well into perspective, what is it that I want to solve with a file based router and how well does it play along with HTMX. The purpose of this framework is to work with HTMX and HTMX needs rest end points to shine. I could provide a controllers folder that has all the API end points but if for some reason people want CRUD operations in the same file that would pose an issue. But either way I'll provide two solutions, the controllers folder will then act as the api end point for our for both the filed based one and path based one.
    - Do I wan't to have completed file based routing? I.e no `jsxRenderer` middleware for the layout page as I would in a page based routing project? 'cos each file is supposed to be rendered as a [child inside the layout](https://github.com/urostripunovic/hono-htmx-webcomponents/blob/main/src/components.tsx#L11) using the [jsxRenderer middleware](https://github.com/urostripunovic/hono-htmx-webcomponents/blob/main/src/index.tsx#L34). My idea of implementing a file based routing system was to have a `Layout.tsx` component to which I add in the middleware and then from there the files inside the routes folder will render as children. Because my solution is not a true file based routing system but a half and half one. Compared to Hono's solution they have [reserved files](https://github.com/sonikjs/sonik/tree/main?tab=readme-ov-file#reserved-files) for errors and layouts. The idea of having a reserved files do look good but I want to see how hard it would be to implement a error, not found and nested layouts (didn't think of these ones) for myself.  
    - information links: [File-base routing #1277](https://github.com/honojs/hono/issues/1277), [Vite plug in](https://github.com/hannoeru/vite-plugin-pages), [Vike](https://vike.dev/routing)
    - [ ] Implement my idea of a file based routing solution where only the routes folder will act as url paths. I'll have a jsxRenderer layout root, a routes folder with all the files and then 
    - [ ] Use SonikJS's [implementation](https://github.com/sonikjs/sonik/tree/main?tab=readme-ov-file#server-entry-file) of file based routing
    - [ ] "Implement" page based routing
- [ ] ~~Database integration (SQLite)~~
    - How can I ensure that the db is persisted throughout the application? Do I need to create a context file that keeps track of it? Like with bindings and then call it via `c.var.db` or `c.env.db`. Hono can have [both](https://github.com/honojs/hono/blob/main/src/types.ts#L17) bindings for cloudflare and then Variables for other things which then provides a global context.
    - What kind of database would I integrate and where would I host it + how well can I secure it? This could be left to the user to create their own solutions but I could also use something like Turso, I really like SQLite. I can also provide integration of Cloudflare's own SQLite db. I need to check out how they can be implemented and set up by the user. I don't want to work with ORMs, I can maybe provide support for drizzle and prisma but I don't think so, I prefer raw SQL.
    - In terms of picking a database, this is for the user to decide. I'll provide two options for databases to use and a third one with the folder empty so that the user can pick what they want.
- [ ] How do I create a CLI or npm package so that people can create their projects.
    - Project name ofc
    - Pick filed based routing yes or no
    - Pick a db cloud flare, turso or nothing.

### Starting folder
- [ ] Type safety when working with HTML elements using htmx
    - I actually found this when checking out the [BETH stack](https://github.com/ethanniser/beth-b2b-saas/blob/main/src/types/htmx.d.ts), might add some improvements to this when adding dynamic.
    - If I want to have end points inside filed based routing I would need to account for those types as well.
- [ ] T3.gg config maybe? Easier to work with instead of having a bunch of process.ENV that aren't type safe.
- [ ] Database integration (SQLite)
    - See of the DX would with drizzle
    - [ ] Implement bare bones Turso with LibSQLite and node
    - [ ] Integrate-ish Cloudflare with node
    - [ ] No database option, leave it to the user
- [ ] Vite config for building the web components i.e production vs development
- [ ] Deployment
    - [ ] Docker with vite?
    - [ ] Cloudflare workers, I need to check this.
    - [ ] Vercel using hono adapter

### Documentation  
    - [ ] Stuff needed to understand and use the package. At a later date...

## Folder path Ideas
Display of how the file structure could look like based on what type of routing is used
*File based routing:*
```txt
.
├── app
│   ├── client.ts // client entry file
│   ├── Island.ts // Astro Client Directives web component
│   ├── islands
│   │   └── counter.ts // island component
│   ├── components // reusable components
│   │   ├── Button.tsx
│   │   └── Navbar.tsx
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
│   │   ├── Island.ts // Astro Client Directives (Subject to change)
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

### Updated file path ideas
This is dependent on how well I implement the idea for my client directives!

*Updated File based routing:*
```txt
.
├── app
│   ├── client.ts // client entry file
│   ├── islands
│   │   └── counter.ts // island component
│   ├── config // T3.gg config schema using zod
│   ├── components // reusable components
│   │   ├── Button.tsx
│   │   └── Navbar.tsx
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

*Updated Normal routing:*
```txt
.
├── app
│   ├── client.ts // client entry file
│   ├── islands // depending on how well the implementation of client directive goes 
│   │   └── counter.ts // island component
│   ├── components //Common reusable component
│   │   ├── Button.tsx
│   │   └── Navbar.tsx
│   ├── controllers // api controllers.
│   │   ├── *.ts // collected routes as a final export
│   │   └── login.tsx // login api controller
│   ├── db // database
│   ├── pages // each page would be represented as it's own route end point along with it's own HTMX operations + Web components 
│   │   ├── _404.tsx // not found page
│   │   ├── _error.tsx // error page
│   │   ├── _layout.tsx // layout template for JSXRender
│   │   ├── about.tsx // matches `/about/:name` and renders a html file that way
│   │   └── index.tsx // this would have the `/` end point for example i.e our homepage
│   └── server.tsx // server entry file with endpoint for each page
├── package.json
├── public
│   └── favicon.ico
├── tsconfig.json
└── vite.config.ts
```