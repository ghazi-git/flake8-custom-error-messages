# flake8-custom-error-messages

A [flake8](https://github.com/pycqa/flake8) plugin to customize the error messages emitted by other flake8 plugins

## Installation

Install with `pip`

```shell
pip install flake8-custom-error-messages
```

## Configuration Options

The package has one configuration option `--custom-error-messages "<code> [<new_msg>" "<code> <new_msg>", ]` that
can have multiple values. Each one of them should start with the error code followed by a space and after that
comes the new message to be displayed. You can also interpolate the original message as well.

```shell
flake8 some_file.py --format=custom_error_messages --custom-error-messages "TST000 {original_message} For more info,\
    check the styleguide at https://some-domain.com" "DTZ005 Use django.utils.timezone.now() instead of datetime.now()"
```

Given that you might be overriding multiple error messages, adding that to a configuration file is better than
specifying them as part of the flake8 command. Each message should be in its own line (messages cannot span
multiple lines).
```ini
# setup.cfg or tox.ini or .flake8
[flake8]
format = custom_error_messages
custom-error-messages =
    TST000 {original_message} For more info, check the styleguide at https://some-domain.com
    DTZ005 Use django.utils.timezone.now() instead of datetime.now()
```

## Motivation

As a project grows and based on the libraries or framework it uses, it makes sense to change the default error
messages emitted by flake8 plugins to recommend best practices followed by the project in question rather than
a generic message. For example, adding a link to a style guide or best practices document adopted by the project.
Another example is when a framework provides helpers to better deal with the issue in question: instead of the
default messages emitted by [flake8-datetimez](https://github.com/pjknkda/flake8-datetimez#list-of-warnings)
and when the project already relies on Django, it's better to recommend using helpers from the `django.utils.timezone`
module.

## Usage with pre-commit

```yaml
repos:
  - repo: https://github.com/pycqa/flake8
    rev: '6.0.0'
    hooks:
      - id: flake8
        args: [
          --format,
          'custom_error_messages',
          --custom-error-messages,
          'DTZ005 Use django.utils.timezone.now() instead of datetime.now()',
          'TST000 {original_message} For more info, check the styleguide at https://some-domain.com'
        ]
        additional_dependencies: [ "flake8-custom-error-messages==0.1.1" ]
```

## License

This project is [MIT licensed](LICENSE).
