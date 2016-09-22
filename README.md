[![Build Status](https://travis-ci.org/simple-finance/elected.svg?branch=master)](https://travis-ci.org/simple-finance/elected)
[![Code Climate](https://codeclimate.com/github/simple-finance/elected/badges/gpa.svg)](https://codeclimate.com/github/simple-finance/elected)
[![security](https://hakiri.io/github/simple-finance/elected/master.svg)](https://hakiri.io/github/simple-finance/elected/master)
[![Gitter](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/simple-finance/elected?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)

# Elected - A ruby distributed leader election through Redis locks. 

[![Join the chat at https://gitter.im/simple-finance/elected](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/simple-finance/elected?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

> Elect a leader out of many processes and run code only in one of your servers at a time.
>
> This library depends heavily on the [redlock](https://github.com/leandromoreira/redlock-rb) ruby gem to obtain distributed locks through Redis.

Elected is a Spanglish-fluent gem so expect Spanish and English names all over. Pardon my French!

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'elected'
```

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install elected

## Documentation

[RubyDoc](http://www.rubydoc.info/gems/elected/frames)

## Usage example

```ruby
  # Config your redis urls or use the default REDIS_URL environment variable by default.
  Elected.redis_urls = ['redis://localhost:6379', 'redis://someotherhost:6379']
  # Create a leader manager with a key and timeout (in milliseconds)
  mgr = Elected::Senado.new 'my_app_name:stage', 30_000
  # Are we the leaders?
  mgr.leader?
  => true
  
  # Then in some other app/shell
  mgr = Elected::Senado.new 'my_app_name:stage', 30_000
  mgr.leader?
  => false
  # But if we wait over 30 seconds
  mgr.leader?
  => true
```

Elected, like Redlock, works seamlessly with [redis sentinel](http://redis.io/topics/sentinel), which is supported in redis 3.2+. It also allows clients to set any other arbitrary options on the Redis connection, e.g. password, driver, and more.

```ruby
Elected.redis_urls= [ 'redis://localhost:6379', Redis.new(:url => 'redis://someotherhost:6379') ]
Elected.electorado
```

## Run tests

Make sure you have at least 1 redis instances up.

    $ rake rspec

## Disclaimer

The hard work of securing a distributed lock is all done through the great [redlock](https://github.com/leandromoreira/redlock-rb) gem. Thanks to [Leandro Moreira](https://github.com/leandromoreira) for his hard work. 
This code, thanks to Redlock, implements an algorithm which is currently a proposal, it was not formally analyzed. 
Make sure to understand how it works before using it in your production environments. 
You can see discussion about this approach at [reddit](http://www.reddit.com/r/programming/comments/2nt0nq/distributed_lock_using_redis_implemented_in_ruby/).

## Development

After checking out the repo, run `bin/setup` to install dependencies. 
Then, run `rake spec` to run the tests. You can also run `bin/console` for an interactive prompt that will allow you to experiment.

To install this gem onto your local machine, run `bundle exec rake install`. 
To release a new version, update the version number in `version.rb`, and then run `bundle exec rake release`, which will create a git tag for the version, push git commits and tags, and push the `.gem` file to [rubygems.org](https://rubygems.org).

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/simple-finance/elected. 
This project is intended to be a safe, welcoming space for collaboration, and contributors are expected to adhere to the [Contributor Covenant](contributor-covenant.org) code of conduct.


## License

The gem is available as open source under the terms of the [MIT License](http://opensource.org/licenses/MIT).

