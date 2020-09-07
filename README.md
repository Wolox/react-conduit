# QA Conduit

# Configurations

## Applications
- Download and install [Rbenv](https://github.com/rbenv/rbenv#basic-github-checkout).
- Download and install [Ruby-Build](https://github.com/rbenv/ruby-build#installing-as-an-rbenv-plugin-recommended).
- Install the appropriate Ruby version by running `rbenv install [version]` where `version` is the one located in [.ruby-version](.ruby-version)


## Database Setup

Run in terminal:

```bash
  sudo -u postgres psql
  CREATE ROLE "development-qa-conduit" LOGIN CREATEDB PASSWORD 'qa-conduit';
```

Log out from postgres.


# Getting started

To get the Rails server running locally:

- Clone this repo
- `bundle install` to install all required dependencies
- `rails db:drop db:create db:migrate` to make all database migrations
- `rails s` to start the local server

Your server is ready to run. You can do this by executing `rails server` and going to [http://localhost:3000](http://localhost:3000). Happy coding!

# QA Conduit - Code Overview

<details><summary>Show QA Conduit code overview</summary>
<p>

> Example Rails codebase that adheres to the [RealWorld](https://github.com/gothinkster/realworld-example-apps) API spec.

This repo is functionality complete -- PRs and issues welcome!

Check out the [rails-5.1 branch](https://github.com/gothinkster/rails-realworld-example-app/tree/rails-5.1) to see the updated code for Rails 5.1

## Dependencies

- [acts_as_follower](https://github.com/tcocca/acts_as_follower) - For implementing followers/following
- [acts_as_taggable](https://github.com/mbleigh/acts-as-taggable-on) - For implementing tagging functionality
- [Devise](https://github.com/plataformatec/devise) - For implementing authentication
- [Jbuilder](https://github.com/rails/jbuilder) - Default JSON rendering gem that ships with Rails, used for making reusable templates for JSON output.
- [JWT](https://github.com/jwt/ruby-jwt) - For generating and validating JWTs for authentication

## Folders

- `app/models` - Contains the database models for the application where we can define methods, validations, queries, and relations to other models.
- `app/views` - Contains templates for generating the JSON output for the API
- `app/controllers` - Contains the controllers where requests are routed to their actions, where we find and manipulate our models and return them for the views to render.
- `config` - Contains configuration files for our Rails application and for our database, along with an `initializers` folder for scripts that get run on boot.
- `db` - Contains the migrations needed to create our database schema.

## Configuration

### camelCase Payloads

- [`config/initializers/jbuilder.rb`](https://github.com/gothinkster/rails-realworld-example-app/blob/master/config/initializers/jbuilder.rb) - Jbuilder configuration for camelCase output
- [`app/controllers/application_controller.rb#underscore_params!`](https://github.com/gothinkster/rails-realworld-example-app/blob/master/app/controllers/application_controller.rb#L44) - Convert camelCase params into snake_case params

### null_session

By default Ruby on Rails will throw an exception when a request doesn't contain a valid CSRF token. Since we're using JWT's to authenticate users instead of sessions, we can tell Rails to use an empty session instead of throwing an exception for requests by specifying `:null_session` in [app/controllers/application_controller.rb](https://github.com/gothinkster/rails-realworld-example-app/blob/master/app/controllers/application_controller.rb#L4).

### Authentication

Requests are authenticated using the `Authorization` header with a valid JWT. The [application_controller.rb#authenticate_user!](https://github.com/gothinkster/rails-realworld-example-app/blob/master/app/controllers/application_controller.rb#L32) filter is used like the one provided by Devise, it will respond with a 401 status code if the request requires authentication that hasn't been provided. The [application_controller.rb#authenticate_user](https://github.com/gothinkster/rails-realworld-example-app/blob/master/app/controllers/application_controller.rb#L18) filter is called on every request to try and authenticate the `Authorization` header. It will only interrupt the request if a JWT is present and invalid. The user's id is then parsed from the JWT and stored in an instance variable called [`@current_user_id`](https://github.com/gothinkster/rails-realworld-example-app/blob/master/app/controllers/application_controller.rb#L24). `@current_user_id` can be used in any controller when we only need the user's id to save a trip to the database. Otherwise, we can call [`current_user`](https://github.com/gothinkster/rails-realworld-example-app/blob/master/app/controllers/application_controller.rb#L36) to fetch the authenticated user from the database.

Devise only requires an email and password upon registration. To allow additional parameters on sign up, we use [application_controller#configure_permitted_parameters](https://github.com/gothinkster/rails-realworld-example-app/blob/master/app/controllers/application_controller.rb#L14) to allow additional parameters.

</p>
</details>


# Documentation

You can find more documentation in the [docs](docs) folder. The documentation available is:

- [Run locally with Docker](docs/docker.md)
- [Deploy with Elastic Beanstalk](docs/deploy.rb)
- [Locales structure](docs/locales.rb)
- [Seeds](docs/seeds.rb)

## Contributing

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Run rspec tests (`bundle exec rspec spec -fd`)
5. Run scss lint (`bundle exec scss-lint app/assets/stylesheets/`)
6. Run rubocop lint (`bundle exec rubocop app spec -R`)
7. Push your branch (`git push origin my-new-feature`)
8. Create a new Pull Request

## About

This project is written by [Wolox](http://www.wolox.com.ar).

[![Wolox](https://raw.githubusercontent.com/Wolox/press-kit/master/logos/logo_banner.png)](http://www.wolox.com.ar)
