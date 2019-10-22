# Howzit

A command-line reference tool for tracking project build systems

## Getting Started

Howzit is a simple, self-contained script (at least until I get stupid and make a gem out of it).

### Prerequisites

- Ruby 2.4+

It probably works on older Rubys, but is untested prior to 2.4.1.

### Installing

Save the script as `howzit` to a folder in your $PATH and make it executable with:

    chmod a+x howzit

## Usage

### Setup

Howzit relies on there being a file in the current directory with a name that starts with "build" and an extension of `.md`, `.txt`, or `.markdown`. This note contains sections such as "Build" and "Deploy" with brief notes about each topic in Markdown (or just plain text) format.

The sections of the notes are delineated by Markdown headings, level 2 or higher, with the heading being the title of the section. I split all of mine apart with h2s. For example, a short one from the little website I was working on yesterday:

```
## Build

gulp js: compiles and minifies all js to dist/js/main.min.js

gulp css: compass compile to dist/css/

gulp watch

gulp (default): [css,js]

## Deploy

gulp sync: rsync /dist/ to scoffb.local

## Package management

yarn

## Components

- UIKit
```

Howzit expects there to only be one header level used to split sections. Anything before the first header is ignored.

### @Commands

You can include commands that can be executed by howzit. Commands start at the beginning of a line anywhere in a section. Only one section's commands can be run at once, but all commands in the section will be executed when howzit is run with `-r`. Commands can include any of:

- `@run(COMMAND)`
    
    The command in parenthesis will be executed as is from the current directory of the shell
- `@copy(TEXT TO COPY)`

    On macOS this will copy the text within the parenthesis to the clipboard. An easy way to offer a shortcut to a longer build command while still allowing it to be edited prior to execution.
- `@open(FILE OR URL)`
    
    Will open the file or URL using the default application for the filetype. On macOS this uses the `open` command, on Windows it uses `start`, and on Linux it uses `xdg-open`, which may require separate installation.

### Using howzit

Run `howzit` on its own to view the current folder's buildnotes.

Include a section name to see just that section, or no argument to display all.

    howzit build

Use `-l` to list all sections.

    howzit -l

Use `-r` to execute any @copy, @run, or @open commands in the given section. Options can come after the section argument, so to run the commands from the last section you viewed, just hit the up arrow to load the previous command and add `-r`.

    howzit build -r

Other options:

```
Usage: howzit [OPTIONS] [SECTION]

Options:
    -c, --create                     Create a skeleton build note in the current working directory
    -r, --run                        Execute @run, @open, or @copy command for given section
    -l, --list                       List available sections
        --[no-]color                 Colorize output (default on)
        --[no-]md-highlight          Highlight Markdown syntax (default on), requires mdless or mdcat
        --[no-]pager                 Paginate output (default on)
    -w, --wrap COLUMNS               Wrap to specified width (default 80, 0 to disable)
    -h, --help                       Display this screen
    -v, --version                    Display version number

```

## Author

**Brett Terpstra** - [brettterpstra.com](https://brettterpstra.com)

## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details.

## Changelog

### 1.0.1

- Allow section matching within title, not just at start
- Remove formatting of section text for better compatibility with mdless/mdcat
- Add @run() syntax to allow executable commands
- Add @copy() syntax to copy text to clipboard
- Add @url/@open() syntax to open urls/files, OS agnostic (hopefully)
- Add support for mdless/mdcat
- Add support for pager
- Offer to create skeleton buildnotes if none found
- Set iTerm 2 marks for navigation when paging is disabled
- Wrap output with option to specify width (default 80, 0 to disable)
