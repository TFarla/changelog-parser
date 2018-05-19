# Changelog parser

[![Packagist](https://img.shields.io/packagist/dt/TFarla/changelog-parser.svg?style=flat-square)](https://packagist.org/packages/tfarla/changelog-parser)
[![Travis](https://img.shields.io/travis/TFarla/changelog-parser.svg?style=flat-square)](https://travis-ci.org/TFarla/changelog-parser)
[![Coveralls github](https://img.shields.io/coveralls/github/TFarla/changelog-parser.svg?style=flat-square)](https://coveralls.io/github/TFarla/changelog-parser)
[![license](https://img.shields.io/github/license/mashape/apistatus.svg?style=flat-square)](https://opensource.org/licenses/MIT)

## Requirements
- php 7.1 or greater ([supported versions](http://php.net/supported-versions.php))

## Why
A changelog contains information about all the changes in a project.
This parsers can be used to extract information about a specific release.
That information may then be distributed towards your friends, clients, colleagues and other parties.

## Installation

```bash
composer require tfarla/changelog-parser
```

## Usage

Given we have the following markdown file:

```md
## 1.0.0 - 2018-12-20
### Added
- A cool new feature

### Changed
- that thing that was too complex
- slow code into fast code

### Removed
- A gastly bug
```

When we parse that markdown file using the `MarkdownParser`:

```php
<?php

use \TFarla\ChangelogParser\MarkdownParser;

$parser = new MarkdownParser();

$result = $parser->parse(file_get_contents('CHANGELOG.md'));

foreach ($result->getReleases() as $release) {
    echo $release->getVersion() . PHP_EOL;
    echo $release->getDate()->format('Y-m-d') . PHP_EOL;
    foreach ($release->getChanges() as $type => $changes) {
        echo $type . PHP_EOL;
        foreach ($changes as $change) {
            echo "- $change" . PHP_EOL;
        }
    }
}
```

Then we will receive the following output:
```bash
1.0.0
2018-12-20
Added
- A cool new feature
Changed
- that thing that was too complex
- slow code into fast code
Removed
- A gastly bug
```

Tested changelog formats can be found under the `tests/fixtures` directory.

## Running tests
This project uses [golden files](https://medium.com/soon-london/testing-with-golden-files-in-go-7fccc71c43d3) to assert expected behaviour.
These golden files should be part stored in the git repository and can be generated by setting the `GOLDEN` environment variable to `1`:
```bash
GOLDEN=1 composer test
```

## Contributing
Thanks for reading this far into the README and considering contributing to this project.
If you have any questions or suggestions feel free to create an issue.

If you want to modify the code then please follow these steps:

1. Fork it (https://github.com/TFarla/changelog-parser)
2. Create your feature branch (git checkout -b feature/fooBar)
3. Commit your changes (git commit -am 'Add some fooBar')
4. Push to the branch (git push origin feature/fooBar)
5. Create a new Pull Request