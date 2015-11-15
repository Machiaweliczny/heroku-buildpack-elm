# Heroku buildpack for Phoenix/Elm apps

Simply `cd web/elm`
And: `echo "elm-make Something.elm SomethingElse.elm --output ../../priv/static/elm.js" > elm_buildpack_make"`

Enjoy

First you have to add this buildpack like from heroku project root directory
`heroku buildpacks:add --index 2 https://github.com/Machiaweliczny/heroku-buildpack-elm/`
First you should add elixir buildpack - `heroku buildpacks:set https://github.com/HashNuke/heroku-buildpack-elixir`

Also you need to provide Procfile like this:
`echo "web: mix phoenix.server" > Procfile"` in root phoenix project directory.

Also proper `config/prod.exs` shoudl look kinda like it.
```
use Mix.Config

config :familiada, Familiada.Endpoint,
  http: [port: {:system, "PORT"}],
  url: [scheme: "https", host: System.get_env("HEROKU_URL"), port: 443],
  force_ssl: [rewrite_on: [:x_forwarded_proto]],
  secret_key_base: System.get_env("SECRET_KEY_BASE")

config :logger, level: :info

config :familiada, Familiada.Repo,
  adapter: Ecto.Adapters.Postgres,
  username: System.get_env("DATABASE_USERNAME"),
  password: System.get_env("DATABASE_PASSWORD"),
  url: System.get_env("DATABASE_URL"),
  pool_size: 20

```

So you also have to set "SECRET_KEY_BASE" (Rest of environment variables is provided by Heroku) with this cmd:
```
heroku config:set SECRET_KEY_BASE=<key_of_64_chars>
```

DONE

