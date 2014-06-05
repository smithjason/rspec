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


each spec requires this require
```ruby
require 'spec_helper'
```

Four Best Practices
```bash
- Describes a set of expectations
- Each it only expects one thing
- Each it is explicit
- Each it description begins with a verb, not should
```

be_valid matcher
```ruby
model = Model.new(params: value)
expect(model).to be_valid
```

### Testing Validations

.to have().errors_on()
```ruby
expect(Model.new(param: nil)).to have(1).errors_on(:param)
```
allows you to simply test validations on specific parameters

### Testing Instance Methods

.to eq(value)
```ruby
expect(model.method).to eq(value)
```

### Testing Class Methods and Scopes
happy example
```ruby
expect(Model.method(param)).to eq(expected_return)
```

failing example
```ruby
expect(Model.method(param)).to_not include(instance_variable)
```

### Before
code contained in before block runs before each example within the describe block it's in
```ruby
before :each do
  stuff
end
```
:each is the default behavior of before

### After

code contained in an after block runs after each example within the describe block it's in


## Chapter 4 - Generating Test Data with Factories

### FactoryGirl 

FactoryGirl define method 
```ruby
FactoryGirl.define do
  factory :contact do
    firstname "John"
    lastname "Doe"
    sequence(:email) { |n| "johndoe#{n}@example.com" }
  end
end
```
This makes it so that whenever we use *FactoryGirl.create(:model)* it uses what we define in the factory definition.

#### Sequences
A sequence automatically increments n inside the block, yielding higher n values for whatever you want to use them for.
This is essential for uniqueness validations on Models.  You can pass any data that you need

#### .create with custom param values
```ruby
FactoryGirl.create(:contact, email: "custom@email.com")
```
Just pass a secondary parameter via options hash

#### .build

Instantiates but does not save.

#### Simply Syntax

```ruby
RSpec.configure do |config|
  config.include FactoryGirl::Syntax::Methods
end
```
This allows you to write methods from FactoryGirl without typing FactoryGirl
```ruby
expect(build(:model)).to be_valid

expect(build(:model, param: nil)).to have(1).errors_on(:param)
```

#### Associations and inheretance in Factories
associated factory
```ruby
association :model
# associates the passed model with the defined factory. use like below

FactoryGirl.define do
  factory :phone do
    association :contact
    phone { '123-456-788' }
    phone_type 'whatevs'
  end
end
```
inhereted factory -
allows you to do things like `FactoryGirl.build(:type_model)` for ease of customization
```ruby
FactoryGirl.define do
  factory :phone do
    association :contact
    phone { 'whatevs' }
    
    factory :home_phone do
      phone_type 'home'
    end
    
    factory :work_phone do
      phone_type 'work
    end
  end
end
```










