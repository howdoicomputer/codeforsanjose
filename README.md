![alt tag](http://imgur.com/Am5CznU)
## About

Currently the only brigade focused web solution is Laddr. I would like to build an alternative.

## Local Development

* Install Ruby 2.2.0 using either `rbenv` or `rvm`.
* Install `bundler`
* Execute `bundle install`
* Execute `rake assets:precompile`
* Execute `rake db:migrate`
* Execute `rails s`
* Access the front page at `127.0.0.1:3000`
* Access the admin page at `127.0.0.1:3000/admin`

Bonus round:
* Run tests via `rake test`

## Meetup Integration

A background job runs every hour that collects upcoming Meetup events for a Meetup group specified by its Meetup URL name in `config/settings.yml`.

Those events are stored in SQLite and are accessed from the `welcome_controller` and then rendered in the `app/views/shared/meetup_events` partial.

The Meetup API key is configured via an environment variable. You can set this in a POSIX compliant shell by running `export MEETUP_API_KEY=<your_key_here>` for local development or `rake figaro heroku:set -e production` if you're deploying to Heroku.

If you would like to pull down the latest set of events for an initial seeding of the database then there is a rake task to accomplish just that: `rake meetup_poll_job:perform_now`. However, because of uniqueness validation it'll only be useful for that initial seeding.

The polling job will run ever sixy minutes - as configured by the whenever schedule. Job scheduling timing is configured in the `config/schedule.rb` file. However, the whenever gem requires writing to the systems crontab. Setting your crontab for local development is not needed but this is to be done if the application is deployed.
