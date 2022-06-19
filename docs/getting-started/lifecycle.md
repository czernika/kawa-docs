# App lifecycle

Kawa Framework is based on WordPress. We will not cover, how WordPress handles request, but we will cover, how Kawa Framework works as WordPress theme

First of all, Kawa uses Bedrock folder structure - server request is forwarded into `web` directory and meets `index.php` file. Next Kawa includes `configuration/application.php` file with WordPress core files, setting up WordPress environment constants, defines database connection, default theme and booting it. As you may understand, we're setting Kawa theme as default theme

> Every next topic will cover `web/app/themes/kawa` directory and every file we mention will be located within. When we're saying `functions.php` file, what we're actually mean is `web/app/themes/kawa/functions.php` file

!> Every WordPress theme (except for block themes) requires files named `index.php` and `style.css`. Kawa Framework no exception in this rule - keep it in mind

## Booting Application

All starts within `functions.php`. It is pretty empty file and requires only some PHP file from `bootstrap` directory. This is where our journey begins

First of all, we will build App Dependency Injection Container, using [PHP DI-6](https://php-di.org) package. Next we will instantiate Application object, add some crucial dependencies such as Request and Kernel instances, define template engine which will be used for our application and boot it

?> Few extra words about Kernel class - it is located within `src/Http` directory and contains a lot of app features - it will define app routes, middleware, theme service providers and much more. This is your application core

On a booting app handles two main things - first of all it will load ap bootstrappers including Service Providers. Every Service Provider class consist of two methods - you may use any of them, both or even none - but they will fire in appropriate order. First every Service Provider will handle `register()` method. It is a good place to register some app container bindings, which may be used within your application

> You may ask - what is the difference between registering bindings within `bootstrap/app.php` file and any Service Provider? There is none but order. Registering container within appropriate Service Provider is more SOLID-way of defining things than registering them directly

Next Service Provider will fire `boot()` method. Every passed value into service container will be available under `$app` property of Provider class - you may use it for booting some external libraries or theme hooks 

When app is booted, it hooks into `kawa/response` action within `index.php` file

!> Note - Kawa Framework does not ruins WordPress template hierarchy. So if you will create `single.php` file, its content will be rendered on every post page. We decided to keep it functionality as this approach will not break any template plugins like WooCommerce and so on. It may be still good to have default template hierarchy as theme fallback if something went terribly wrong

## Handling request

`kawa/response` hook accepts global request object with information about server, queried parameters and so on. It is being handled by Kernel class. What it is actually doing, is collect every route from any file within `routes` directory, defines current request method and looking for any matching route to its condition object among every route. If it finds satisfied route, it will call route handler callback. If it not - then 404 response will be returned

> Route may has any condition such as callable tag condition, typical uri routing based on regex or any specific one you define. First route which matches provided condition will be set as satisfied  

When handler defined, Kernel will call it to get page response. But first it will go through every middleware for this route, included grouped and controllers one. If some of them "failed", middleware object will correct response object and return a new one. If there are more than one failed - the first failed response will be returned

And finally Kernel object send headers and drawing the result of handler callback. You may now enjoy your "Hello World!" page

If you're not going to use Kawa Framework (wanna use it like regular WordPress theme), you will need to remove `kawa/response` hook from `index.php` file and clean up `functions.php` or just comment app booting within `bootstrap/app.php` - that's minimum. Of course you should remove unused composer dependencies in this case but still Kawa Framework will not be booted without calling `boot()` method.

!> It still makes sense as Kawa Framework is not tested with every possible plugin - remember your application still will use Bedrock folder structure, where `HOME_URL` and `SITE_URL` are differs. Kawa Framework strongly relies on Bedrock structure and will **NOT** be tested with default WordPress structure
