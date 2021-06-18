# iopromise-dalli

This gem provides a promise-based parallel interface to [dalli](https://github.com/petergoldstein/dalli) (memcached), based on [IOPromise](https://github.com/iopromise-ruby/iopromise).

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'iopromise-dalli'
```

And then execute:

    $ bundle install

Or install it yourself as:

    $ gem install iopromise-dalli

## Usage

The pattern for using `IOPromise::Dalli` is very similar to regular Dalli, except that everything returns promises, and the promises resolve to response objects that contain the `value` and `exist?` as separate control and data information.

```ruby
require 'iopromise/dalli'

client = IOPromise::Dalli.new('localhost:11211')

promises = [
  client.get('some_key'),
  client.set('another_key', 'something'),
]

promises << client.get('next_key').then do |response|
  # note that control (exist?) and data (value) are not mixed
  if response.exist?
    puts "#{response.key} exists with value #{response.value}"
  else
    puts "#{response.key} does not exist"
  end
end

# this will execute all 3 operations at the same time, similarly to an "mget",
# but not dependent on an explicit API, nor limited to just gets.
Promise.all(promises).sync
```

## Development

After checking out the repo, run `bin/setup` to install dependencies. Then, run `bundle exec rake spec` to run the tests. You can also run `bin/console` for an interactive prompt that will allow you to experiment.

To install this gem onto your local machine, run `bundle exec rake install`. To release a new version, update the version number in `version.rb`, and then run `bundle exec rake release`, which will create a git tag for the version, push git commits and the created tag, and push the `.gem` file to [rubygems.org](https://rubygems.org).

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/iopromise-ruby/iopromise-dalli. This project is intended to be a safe, welcoming space for collaboration, and contributors are expected to adhere to the [code of conduct](https://github.com/iopromise-ruby/iopromise-dalli/blob/main/CODE_OF_CONDUCT.md).

## License

The gem is available as open source under the terms of the [MIT License](https://opensource.org/licenses/MIT).

## Code of Conduct

Everyone interacting in the iopromise project's codebases, issue trackers, chat rooms and mailing lists is expected to follow the [code of conduct](https://github.com/iopromise-ruby/iopromise-dalli/blob/main/CODE_OF_CONDUCT.md).
