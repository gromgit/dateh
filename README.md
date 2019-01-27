# dateh
A GNU date wrapper that adds several format specifiers that may prove useful to some folks.

The first set deals with relative date outputs (English only):

| Spec | Description |
| ---- | ----------- |
| `@{d}` | relative date, abbrev date names (e.g. `yesterday`, `next Fri`, `17 days ago`) |
| `@{D}` | like `@{d}`, only with full date names (e.g. `next Friday`, `17 days' time`) |
| `@{d+}` | like `@{d}`, but falls back to user-configurable date representation if outside 1 week's range (default: `%Y-%m-%d`) |
| `@{w}` | relative week (e.g. `last week`, `3 weeks' time`) |
| `@{m}` | relative month (e.g. `last month`, `3 months' time`) |
| `@{y}` | relative year (e.g. `last year`, `3 years' time`) |
| `@{h}` | auto-select relative representation (abbreviated day name) |
| `@{H}` | auto-select relative representation (full day name) |

*NOTE*: `@{d}` and `@{D}` outputs follow GNU `date` conventions for relative date input:
* `last XYZ` for dates up to 7 days ago
* `next XYZ` for dates up to 7 days in the future

The second set deals with ordinal day-of-month representations:

| Spec | Description |
| ---- | ----------- |
| `@{o}` | ordinal day-of-month, short-form (e.g. `29th`) |
| `@{O}` | ordinal day-of-month, long-form (e.g. `twenty-ninth`) |

## Dependencies

GNU `date` must be in your `PATH`.

## Installation

Copy `dateh` to a directory in your `PATH`.

## Usage

`dateh` takes the same options and format specifiers as GNU `date`, in addition to:
* `-h|--help` to print `dateh`-specific help
* `-H|--longhelp` to print both `dateh` and `date` help
* `-V|--version` to print `dateh` version

If the `DATEH_DEFAULT_FORMAT` environment variable is set, `dateh` uses its value as the fallback date representation for the `@{d+}` specifier (default: `%Y-%m-%d`).

## Examples

```
$ dateh
Sun Jan 27 20:05:00 +08 2019

$ dateh -d "now" "+@{D} %X"
today 20:05:00

$ dateh -d "now + 3 weeks" "+the @{O} of %B, %Y"
the seventeenth of February, 2019

$ DATEH_DEFAULT_FORMAT=%m/%d/%Y dateh -d "last month" \
     "+@{d+}, @{w}, @{m}, @{y} %H:%M %P"
12/27/2018, 4 weeks ago, last month, last year 20:05 pm
```
