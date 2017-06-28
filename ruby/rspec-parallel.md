# RSpec Parallel

Parallel tests are based on this project: https://github.com/grosser/parallel_tests
> Speedup RSpec by running parallel on multiple CPU cores.
ParallelTests splits tests into even groups (by number of lines or runtime) and runs each group in a single process with its own database.

## How to use locally
In order to properly work, parallel tests need to have one database by runner.
You must therefore create those DBs once and then migrate them when needed.

### ENV variable

An ENV variable is automatically set when specs are run.
<table>
  <tr><td>Process number</td><td>1</td><td>2</td><td>3</td></tr>
  <tr><td>ENV['TEST_ENV_NUMBER']</td><td>''</td><td>'2'</td><td>'3'</td></tr>
</table>
**Beware, first value is an empty string, not zero or one (configurable is really needed) !**

This variable is used in order to prevent clash during read/write operations, such as database or upload/download files which use their own folder to avoid conflict (see `spec/support/helpers/tmp_dir_cleanup.rb` for more infos).

```yaml
test:
  database: effilab_test<%= ENV['TEST_ENV_NUMBER'] %>
```

### Create multiple databases
`RAILS_ENV=test bundle exec rake parallel:create --verbose --trace`

### Migrate multiple databases
`RAILS_ENV=test bundle exec rake parallel:load_structure --verbose --trace`

> NB: You can specify the number databases with `parallel:XXX[n]`, *n* being the number of processes you want to run

### Run tests
`bundle exec parallel_rspec spec/ --runtime-log log/rspec/parallel_runtime_rspec.log`
Available option :
```
-n [PROCESSES]   How many processes to use, default: available CPUs
```

### Configuration
When run in parallel, RSpec loggers and configuration used are defined in `.rspec_parallel` file.

### Optimization
Test groups are not well balanced by default and will run for different times, making everything wait for the slowest group.
We use a specific logger to record test runtime and then use the recorded runtime to balance test groups more evenly.

This file is stored here: `log/rspec/parallel_runtime_rspec.log`

**Therefore, it should be updated when new tests are added to the suite !**
