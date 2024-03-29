# bake

Bash make, with very lightweight approach

This is very similar to https://github.com/rumkin/bake project, but
slightly simpler implementation. No additional folders or files,
just a Bakefile with functions required.

If you want to use the interactive menu, install https://github.com/p-gen/smenu
`smenu` is installable via apt-get in debian/ubuntu based distros.

# Install

## Quick install:
```
curl https://raw.githubusercontent.com/moonq/bake/main/bake > ~/.local/bin/bake
chmod +x ~/.local/bin/bake
echo '. <( bake __autocomplete __ )' >> ~/.bashrc
```

## Update installation

Run  `bake __update__`

## Install from repository

Clone the repository.

Run the command: `./bake` in this folder. Installation prefix is required
to install, for example: `./bake  install ~/.local/`
Finally, insert the autocomplete as explained in installer.

Note: you can also just take the `bake` executable, and place in PATH, and
run `. <( bake __autocomplete __ )` to enable bash autocomplete.

# Usage

Create a Bakefile:

```
help() { # This help. Note the exact function declaration + docs format.
    _menu
}

default() { # Default function will be run if present, and no target passed
  # Extra help to be read with -h
    echo "I'm running!"
}

build() { # Build by project
    echo building..
    echo I can get arguments: "$1"
    echo I have global variables: "$somevar"
}

deploy() { # Deploy project
    build # I dont have similar dependency tree as GNU make, all commands are run
    if [[ ! -f somebinary.bin ]]; then
       # if build produces a file, we can check and not run building again..
       build
    fi
    echo Now deploying...
    _subcommand
}

_subcommand() {
    # if function starts with _, it's not listed as target, and
    # can be called by other functions
    echo subcommand called somewhere
}

echo foo # NOTE: Bakefile gets sourced when read!
somevar=value

if [ -z "$BAKEFILE" ]; then
   echo This part is run if the file is sourced and not run in Bake
fi

```

- run `bake` to get list of targets.
- run `bake build argument` to launch target with arguments
