# include

The goal of this project is to define a routine for (GNU) Bash programmers and
scripters to easily use external Bash symbols. This opens doors for writing
large and scalable Bash programs.

The name "include" was inspired by the `#include` directive in the C programming
language.

## Example

An example is in the [`example`](example) directory. The program `main.sh`
imports symbols from `lib.sh`, which contains the following:

```bash
#!/bin/bash
# lib.sh

a=1
b=2
local c=3

string="a b c"

function say_hello() {
    echo "Hello"
}
```

The `main.sh` script prints the external symbols and calls the external function
`say_hello`:

```bash
#!/bin/bash
# main.sh

source include

include lib.sh

echo "a: $a"
echo "b: $b"
echo "c: $c"

echo "$string"

say_hello
```

To run the example:

```bash
cd example
source main.sh
```

Your output should be:

```
a: 1
b: 2
c: 
a b c
Hello
```

As demonstrated, include is able to import external variables and functions.
Notice that `$c` is empty because `c` is defined as `local` in `lib.sh`.

## How to include include

Developing a Bash program and wants to use include? Here are some ways you can
include include!

### Include the include script at the top

```bash
function include() {
    source "$1" 2> /dev/null

    if [ $? -ne 0 ]; then
        echo "include: could not include $1" >&2
        exit 1
    fi
}
```

### Include the minimized include script at the top

```bash
function include() { source "$1" 2>/dev/null; if [ $? -ne 0 ]; then echo "include: could not include $1" >&2; exit 1; fi }
```

### Using an installer

You could write an installer for your Bash program and in the installer, install
the include script to somewhere like `$HOME/.local/bin` for example (or anything
as long as it is in the user's `$PATH`). Then in your Bash scripts, simply do:

```bash
source include
```

at the top.

If your project consists of multiple Bash files, this method is perhaps the best
way to go, in contrast to the above 2, in which case you will have to copy and
paste the include script multiple times.

### List include as a dependency

Simply list include as a dependency and tell your users to download include. Be
sure to `source include` in the scripts that need it.

## License

This software is in the public domain. For more information, visit
<https://unlicense.org>
