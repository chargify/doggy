# Doggy

[![Build Status](https://travis-ci.org/Shopify/doggy.svg?branch=master)](https://travis-ci.org/Shopify/doggy)

Doggy manages your DataDog dashboards, alerts, monitors, and screenboards.

## Installation

Create a new git repo with an empty `objects` folder.

Add this line to your Gemfile:

```ruby
gem 'doggy'
```

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install doggy

## Authentication

To authenticate, you need to set API and APP keys for your DataDog account.

#### Environment variables

Export `DATADOG_API_KEY` and `DATADOG_APP_KEY` environment variables and `doggy` will catch them up automatically.

#### ejson

Set up `ejson` by putting `secrets.ejson` in your root object store.

#### json (plaintext)

If you're feeling adventurous, just put plaintext `secrets.json` in your root object store like this:

```json
{
  "datadog_api_key": "key1",
  "datadog_app_key": "key2"
}
```

## Usage

```bash
# Manually generates a deploy event with the current SHA of your git repo.
# This may be required before your first `doggy sync` if you get an error like:
# NoMethodError: undefined method `[]' for nil:NilClass
#   doggy/lib/doggy/model.rb:137:in `current_sha'
# Doing this implies that any objects currently defined locally are already defined on Datadog
$ doggy generate_deploy_event

# Syncs local changes to Datadog since last deploy and creates deploy event.
# Will look at the SHA of the latest deploy event and push any changes
# commited since that SHA.
# ONLY pushes any changed objects/deletes any deleted objects.
# Does NOT pull down any changes from Datadog.
$ doggy sync

# Download items. If no ID is given it will download all the items managed by dog.
$ doggy pull [IDs]

# Upload items to Datadog. If no ID is given it will push all items.
$ doggy push [IDs]

# Edit an item in WYSIWYG, will create a copy of the item on Datadog
# and open it your browser to edit. Once done, come back and enter `Y`.
# The copy will be deleted from Datadog.
# Any changes made will be updated in your local object file.
# It will NOT change the original item on Datadog. You will either need to:
# `doggy push ID` OR commit the changes and then do `doggy sync`.
$ doggy edit ID

# Delete selected items from both Datadog and local storage
$ doggy delete IDs

# Mute monitor(s) forever or with optional `--duration DURATION` (`1h`,`1d`, etc.)
# Also accepts optional `--scope` (this is Datadog scopes, such as `--scope 'role:mysql'`)
# to mute all monitors for a given scope.
$ doggy mute IDs

# Unmute monitor(s).
# Also accepts optional `--scope` (this is Datadog scopes, such as `--scope 'role:mysql'`)
# Or `--all-scopes` to unmute accross all scopes
$ doggy unmute IDs
```
Multiple IDs should be separated by space.

## Development

After checking out the repo, run `bundle install` to install dependencies.

To install this gem onto your local machine, run `bundle exec rake install`. To release a new version, update the version number in `version.rb`, and then run `bundle exec rake release` to create a git tag for the version, push git commits and tags, and push the `.gem` file to [rubygems.org](https://rubygems.org).

## Contributing

1. Fork it ( https://github.com/bai/doggy/fork )
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create a new Pull Request
