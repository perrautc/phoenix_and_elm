# PhoenixAndElm

To start your Phoenix app:

  * Install dependencies with `mix deps.get`
  * Create and migrate your database with `mix ecto.create && mix ecto.migrate`
  * Install Node.js dependencies with `npm install`
  * Start Phoenix endpoint with `mix phoenix.server`

Now you can visit [`localhost:4000`](http://localhost:4000) from your browser.

Ready to run in production? Please [check our deployment guides](http://www.phoenixframework.org/docs/deployment).

## Commits and Branches

  * 01-hello-world - Hello world, have the elm build system working

### 01-Hello-World
  * Create a new phoenix application
  ```
    mix phoenix.new phoenix_and_elm
  ```
  * Create Contact model using phoenix.gen.model
  ```
  mix phoenix.gen.model Contact contacts first_name last_name gender:integer birth_date:date location phone_number email headline:text picture
  ```
  * Create the database and migrate
  ```
  mix ecto.create
  mix ecto.migrate
  ```
  * Install Elm
  ```
  npm install elm elm-brunch --save
  ```
  * Configure Brunch to watch and compile elm on change
  ```
  // brunch-config.js
  // plugins: {
    //...
    elmBrunch: {
        // Set to the elm file(s) containing your "main" function
        // `elm make` handles all elm dependencies (required)
        // relative to `elmFolder (optional)`
        mainModules: ["web/elm/Main.elm"],
 
        // Defaults to 'js/' folder in paths.public (optional)
        outputFolder: 'web/static/js',
 
        // If specified, all mainModules will be compiled to a single file (optional and merged with outputFolder)
        outputFile: 'main.js',
 
        // optional: add some parameters that are passed to elm-make
        makeParameters: ['--warn']
      }
    }
  ```
  * Create folder and setup initial Elm packages

  ```
  $ mkdir web/elm && cd web/elm && elm-package install elm-lang/html -y
  $ touch Main.elm
  --- web/elm/Main.elm

  module Main exposing(..)

  import Html exposing(Html, text)

  main : Html a
  main = 
    text "Hello, World!"
  ```
  * Update main app.js to import generated javascript by Elm and render result.
  ```
  // web/static/js/app.js

  import Elm from './main';

  const elmDiv = document.querySelector('#elm_target');

  if (elmDiv) {
    Elm.Main.embed(elmDiv);
  }
  ```
  * Update phoenix template to contain elmDiv target
  ```
<!-- web/templates/page/index.html.eex -->

<div id="elm_target"></div>
  ```
  * start phoenix serve
  ```
  iex -S mix phoenix.server
  ```