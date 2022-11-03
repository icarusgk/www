---
layout: docs
title: "Vercel with Rails - Integrations Quickstart"
---

{% include helpers/reading_time.html %}

{% include icons/vercel.html width="50" color="#430098" %}
{% include icons/rails.html width="50" color="#CC0000" %}


# __Vercel with Rails__
##### `Integrations`
Learn how to make Vercel, Rails, and Dotenv Vault work together in a simple web app. This tutorial assumes you are already familiar with `.env` files and know [how to sync them](/docs/tutorials/sync).

## Package installation
First, create a Gemfile for your Rails application, if you haven't done so already, by initializing it with `bundle init`. Declare the `gem 'dotenv-vault-rails'` in the Gemfile.
##### Rails
```Ruby
// Add gem reference to Gemfile
gem 'dotenv-vault-rails'
```
##### CLI
```Ruby
// Install the gem
bundle install
```
#### Example
```Ruby
<h1>Welcome <%= ENV["HELLO"] %></h1>
```

## Application references
Create a `config` folder and an `application.rb` file inside it. Reference the Vault package as early as possible in the file and apply the `/load` parameter at the back.

#### Rails
```Ruby
// config/application.rb
require 'dotenv-vault/load'
```
<details>

  <summary>Source </summary>

```Ruby
require_relative "boot"

require "rails/all"
require 'dotenv-vault/load'

# Require the gems listed in Gemfile, including any gems
# you've limited to :test, :development, or :production.
Bundler.require(*Rails.groups)

module IntegrationExampleHerokuRails
  class Application < Rails::Application
    # Initialize configuration defaults for originally generated Rails version.
    config.load_defaults 6.1

    # Configuration for the application, engines, and railties goes here.
    #
    # These settings can be overridden in specific environments using the files
    # in config/environments, which are processed later.
    #
    # config.time_zone = "Central Time (US & Canada)"
    # config.eager_load_paths << Rails.root.join("extras")
  end
end
```
</details>

<!-- [example](https://github.com/dotenv-org/integration-example-vercel-rails/blob/374e9a3e5e5f6ffe2f4a83f08bf2c0222871ed40/config/application.rb#L4) -->

## Build the Vault
Make sure you are [logged in and in sync](/docs/tutorials/sync) with your Vault first then run `npx dotenv-vault` from CLI in your project root. This will build an encrypted .env.vault file that serves as a unique identifier for your project in Dotenv. Inside it you will find the public keys for every environment you have setup and must be committed to source.
##### CLI
```Java
npx dotenv-vault build
```

## Fetch the keys
With the Vault successfully built, you now can fetch the `.env.vault` decryption keys for each environment in the Vault project. Running `npx dotenv-vault keys production`, for example, will return the `production` key and so will `development` and `ci` respectively.

##### CLI
```Java
$ npx dotenv-vault keys production
remote:   Listing .env.vault decryption keys... done

dotenv://:key_1234@dotenv.org/vault/.env.vault?environment=production
```

## Set deployment
Now that you have access to the keys for every environment, you will have to reference them as environment variables in your Vercel project's settings. To do that, navigate to your Project, then the Settings tab to reach the Environment Variable panel. Set as key **`DOTENV_KEY`** and as value the decryption key returned in the previous step **`dotenv://:key_1234@dotenv.org/vault/.env.vault?environment=production`**.

![Vercel Environment Variable Settings](https://res.cloudinary.com/dotenv-org/image/upload/v1666956664/dotenv_vercel_environment_variable_settings_bvpjgg.png)


  <!-- <summary>Example</summary>
{% include helpers/screenshot.html url="https://res.cloudinary.com/dotenv-org/image/upload/v1666956664/dotenv_vercel_environment_variable_settings_bvpjgg.png" %} -->
</details>

## Commit and push
That's it!

Commit those changes safely to code and deploy to Vercel.

When the build runs, it will recognize the `DOTENV_KEY`, decrypt the .env.vault file, and load the `production` environment variables to `ENV`. If a `DOTENV_KEY` is not set when developing on local machine, for example, it will fall back to standard Dotenv functionality.