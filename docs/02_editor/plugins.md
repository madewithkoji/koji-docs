# Plugins

## What are plugins?

Plugins encapsulate generic functionality and expose it to Koji frontends. They 
provide an easy way to inject common functionality, like an analytics script, 
metadata tags, ad units, or attribution banners.

## What do plugins do?

Plugins are snippets of code injected into your app at request time.

Plugins *do not* modify the content or functionality of an app. Rather, they 
operate at a layer "above" the app content. Some plugins that are currently 
available include plugins for Google Analytics, a "password" wall that requires 
a user enter a secret code before proceeding to the app, and a "Subscribe to 
me on Youtube" attribution overlay.

## What does a plugin look like?

A plugin consists of one or more injection instructions. An injection 
instruction contains a few pieces of data:
- One or more paths to match against (like `index.html` or `*.html` or `*.css`)
- An injection site (a token to look for in the source code as it is being streamed to the browser, like `</head>`)
- An injection position, specifying whether the plugin code should be injected before or after the injection site
- The code snippet to inject
- A configuration template for variables that the user can configure when enabling the plugin (Plugin configuration templates follow the same specification as VCCs, and variables are accessed as ES-style templates `${variableKey}`)

When a user loads your app, KojiCDN streams the requested files to the user's 
browser. If the app has plugins enabled, the CDN checks to see whether the file 
being requested matches any of the injections' paths. If a match is found, the 
CDN looks for the injection site and injects the plugin's code as the file is 
being sent to the user.

## Search Engine Optimization (SEO)

This has implications for search engines and social crawlers, which often do 
not render Javascript when indexing pages. For example, when you share a link 
on Facebook, it looks for Open Graph `<meta>` tags in order to generate a share 
card. In a typical client-side rendered Single Page App, using something like 
[`react-helmet`](https://github.com/nfl/react-helmet), these tags are not 
available in the DOM until the page's Javascript has loaded, and you need to 
use a paid service like [Prerender.io](https://prerender.io) to serve a shadow 
copy of the rendered page to crawlers.

Because Koji Plugins are computed server-
side, they are available to all crawlers and bots without any additional 
configuration or 3rd-party services. By enabling the Open Graph plugin, for 
example, apps are instantly ready to share on Facebook and Twitter.

## Limitations

At this time, Koji plugins are not designed to modify app functionality or to 
inject code that interfaces with an app's native code. This is an intentional 
decision to avoid fragmentation and spaghettification. Meaningful changes to 
functionality should simply be done within the code of the app, and repeated 
core behaviors should leveage JS packages and the larger Javascript community.

Plugins are only injected into static services. If your app contains multiple 
static services, plugins will be injected into all of them. At this time, there 
is no way to enable or disable plugins on a per-service basis.

## Developing plugins

At this time, plugin development is not open to the community. This is coming 
soon! For now, if you have an idea for a plugin, please share it on Discord and 
we'll see what we can do!
