# RspecFlatErrorFormatter

[![Build Status](https://travis-ci.org/vittorius/rspec_flat_error_formatter.svg?branch=master)](https://travis-ci.org/vittorius/rspec_flat_error_formatter)

RSpec formater that produces errors output easily consumable by automated tools

## Rationale

Initially, this tool came out as an attempt to use the [Visual Studio Code tasks](https://code.visualstudio.com/docs/editor/tasks) for running the RSpec and scanning its output to locate and test the failing specs very quickly. VS Code offers the mechanism of [problem matchers](https://code.visualstudio.com/docs/editor/tasks#_defining-a-problem-matcher) that are, in fact, regular expressions to filter the output of a task line by line and feed the extracted info to the Problems view.

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'rspec_flat_error_formatter'
```

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install rspec_flat_error_formatter

## Usage

### Basic

Pass as a value of the `--format` (or `-f`) option to the RSpec

    $ bundle exec rspec --format RSpecFlatErrorFormatter


Or add as to the default set of options via `.rspec` file:

```
--format RSpecFlatErrorFormatter
```

Now run the tests as usual:

    $ bundle exec rspec

After you've configured the formatter to be used by RSpec, you can consume its output by the tools you have to automate your test results navigation.

### VS Code tasks

Here's the example of configuring `rspec` and `guard-rspec` using VS Code tasks to consume the output of RspecFlatErrorFormatter.

Create a `tasks.json` file with contents:

```json
{
  "version": "2.0.0",
  "tasks": [
    // rspec
    {
      "label": "rspec",
      "type": "shell",
      "command": "rspec",
      "group": "test",
      "presentation": {
        "echo": true,
        "reveal": "always"
      },
      "problemMatcher": {
        "fileLocation": ["relative", "${workspaceFolder}"],
        "pattern": {
          "regexp": "^(.*):(\\d*):\\s(error|info):\\s(.*)$",
          "file": 1,
          "line": 2,
          "severity": 3,
          "message": 4
        }
      }
    },
    // guard-rspec
    {
      "label": "guard",
      "type": "shell",
      "command": "bundle exec guard",
      "presentation": {
        "echo": true,
        "reveal": "always",
        "panel": "dedicated"
      },
      "isBackground": true,
      "problemMatcher": {
        "fileLocation": ["relative", "${workspaceFolder}"],
        "pattern": {
          "regexp": "^(.*):(\\d*):\\s(error|info):\\s(.*)$",
          "file": 1,
          "line": 2,
          "severity": 3,
          "message": 4
        },
        "background": {
          "activeOnStart": true,
          "beginsPattern": "^\\d{2}:\\d{2}:\\d{2} - INFO - Running",
          "endsPattern": "^Randomized with"
        }
      }
    }
  ]
}

```

## Development

After checking out the repo, run `bundle exec rake spec` to run the tests.

To release a new version, update the version number in `version.rb`, and then run `bundle exec rake release`, which will create a git tag for the version, push git commits and tags, and push the `.gem` file to [rubygems.org](https://rubygems.org).

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/vittorius/rspec_flat_error_formatter.

## License

The gem is available as open source under the terms of the [MIT License](https://opensource.org/licenses/MIT).

## Kudos

* [@sj26](https://github.com/sj26) for his beautiful [rspec_junit_formatter](https://github.com/sj26/rspec_junit_formatter) - tests organization and some other stuff in repo are under the huge influence of RSpec JUnit Formatter repo