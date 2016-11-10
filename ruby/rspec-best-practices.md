# RSpec best practices

**Status** : *draft*

----------

## General

- Filenames must end with `_spec.rb` to be taken into account automatically
- Split unit tests that share a functional and group them into a single file for factoring and share behavior / initialization (a little longer to execute, but cleaner in code and easier in case of error)
- Avoid testing too much behaviors in one test, split if needed
- If Sidekiq workers must be tested, run with inline mode block for immediate execution (global configuration is fake!)
- Use the `expect` syntax
- To initialize variable, prefer `let` over instance variables in a before block (lazy load, memoized)
- Prefer request type of testing or feature instead of controller test (increasingly deprecated with rails 5)
- Do not mix tests types in a single file
- Specs are usually placed in a canonical directory structure that describes their purpose:
  - Controller specs reside in the `spec/controllers` directory
	- Feature specs reside in the `spec/features` directory
	- Helper specs reside in the `spec/helpers` directory
	- Lib specs reside in the `spec/lib` directory
	- Mailer specs reside in the `spec/mailers` directory
	- Model specs reside in the `spec/models` directory
	- Pundit policy specs reside in the `spec/policies` directory
  - Request specs reside in the `spec/requests` directory. The directory can also be named integration or api.
	- Routing specs reside in the `spec/routing` directory
  - View specs reside in the `spec/views` directory
  - Worker specs reside in the `spec/workers` directory
- If changes need to made to an AdWords account, check it for real using API
- For others HTTP services, WebMock can be use instead

## Feature tests

This apply most specifically to feature test driven by Capybara

- Functional tests must be placed in the directory `./spec/features`
- No need to specify the type of feature test, RSpec automatically add it when using `RSpec.feature`
- Do not overload tests by logging start / stop, output is sufficiently clear
- With features tests, the application functional is verified through the control of headless browser
  - Not to be confused with other types of tests that do not require to control the interface
- When it comes to visiting an URL, prefer the route methods (`_path/_url`) over string interpolation
- The meta `js: true` must be set when declaring the feature to specify the use of the javascript driver of capybara (poltergeist) only if the test requires javascript execution or HTTP calls out (API, OAuth, etc.)
- Keep a clear description of hierarchy in its labels (easier to read the code and output)
  - `feature`: functionality domain
  - `describe`: grouping scenarios sharing a common initialization for instance
  - `scenario` (alias of `it`) help to identify immediately that this is a feature test (closer to acceptance tests)
- Labels must be succinct but clear to act as documentation
- By default, Capybara waits the appearance of an element in the page for a given time (eg opening of modal)
- In the case of an AJAX call without noticeable change to the view, we can still force to wait for jQuery running requests with the `wait_for_ajax` method (which checks JQuery internal variable)
- Use `page.save_screenshot('screenshot.png', full: true)` for debugging capybara feature

## Configuration and Support folder

### Main configuration files

- `rails_helper.rb`
  - General Rails RSpec configuration and plugins loading
  - Each main support configuration files are required here
  - `.rspec` file auto require this file upon `rspec` command call
- `spec_helper.rb`
  - General RSpec configuration
  - required by `rails_helper`

### Support folder

#### Files
- `capybara.rb` Capybara and Poltergeist configuration
- `database_cleaner.rb` DatabaseCleaner strategy and hooks configuration
- `devise.rb` include Devise test helper for specific spec types
- `extra_config.rb` main extra configuration file
  - require and load helpers
  - `include / extend` RSpec classes with helpers for specifics spec types
- `factory_girl.rb` FactoryGirl configuration and syntax enhancements
- `shoulda_matchers.rb` Shoulda::Matchers configuration
- `sidekiq.rb` Sidekiq running strategy
  - Jobs are `fake!` by default
  - Jobs are executed `inline!` for `:feature` specs
- `tmp_dir_cleanup.rb` helps `tmp/exports` clean between tests run
- `warden.rb` enable Warden test mode to stub login as user in specs
- `web_mock.rb` WebMock configuration, allow net connection to external resources

#### Directories
- `config` misc configuration files (eg. *.yml*)
- `helpers` tests helpers
  - data initialization
  - factory creation helper
  - common behaviour
  - mocks
  - etc.
- `shared_examples` RSpec shared examples
  - all files loaded by `rails_helper`
