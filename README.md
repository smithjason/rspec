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



## Chapter 3 - Model Specs
