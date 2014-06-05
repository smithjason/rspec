# Everyday Rails RSpec notes

Gemfile setup
```ruby
group :development, :test do
  gem 'rspec-rails', '~> 2.14.0'
  gem 'factory_girl_rails', '~> 4.2.1'
end

group :test do
  gem 'faker', '~> 1.1.2'
  gem 'capybara', '~> 2.1.0'
  gem 'database_cleaner', '~> 1.0.1'
  gem 'launchy', '~> 2.3.0'
  gem 'selenium webdriver', '~> 2.39.0'
end
```

(Test) Database Setup - database.yml
```ruby
test:
  adapter: postgresql
  encoding: utf8
  database: whatevers_test
  pool: 5
  username:
  password:
  timeout: 5000
  
```

creates all the dbs
```bash
bundle exec rake db:create:all
```

installs rspec in your project
```bash
bundle exec rails generate rspec:install
```

add this line to .rspec file - makes formatting for rspec easier using documentation format
```bash
--format documentation
```

add to config/application.rb 
```ruby
config.generators do |g|
  g.test_framework :rspec,
    fixtures: true,
    view_specs: false,
    helper_specs: false,
    routing_specs: false,
    controller_specs: true,
    request_specs: false
  g.fixture_replacement :factory_girl, dir: "spec/factories"
end
```

make test database schema match dev db schema
```bash
rake db:test:clone
```

```bash
bundle exec rspec
```


## Chapter 3 - Model Specs

Model Specs should include tests for:
Model's create method when passed valid/invalid params
Data that fail validations
Class/Instance methods expected performance

Four Best Practices
```bash
- Describes a set of expectations
- Each it only expects one thing
- Each it is explicit
- Each it description begins with a verb, not should
```
