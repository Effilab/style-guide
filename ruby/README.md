# Ruby style guide

Add the following code to a `.rubocop.yml` file at the root of your ruby project.

```
inherit_from: https://raw.githubusercontent.com/Effilab/style-guide/master/ruby/.rubocop.yml
```

You'll need the [rubocop-performance](https://github.com/rubocop-hq/rubocop-performance) and [rubocop-rails](https://github.com/rubocop-hq/rubocop-rails) gems to enforce all the rules.

Please gitignore the cache file that will be generated the first time you run Rubocop.

Please don't add rules to the `.rubocop.yml` file in your project,
except if those are obvious (such as deactivating Rails linting if your
project isn't a Rails-like one).

Please use automatic linting in your text editor (see [here](https://effilab.atlassian.net/wiki/display/IT/Ruby+Linter+-+Rubocop)
for advice).
