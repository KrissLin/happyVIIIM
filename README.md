# happyVIIIM
vim cheatsheet XD

## Delete all lines containing pattern

```
:g/pattern/d
```

Delete all lines that are empty

```
:g/^$/d
```

Delete all lines that are empty or contain only whitespace:

```
:g/^\s*$/d
```

Delete all lines that do **not** contain a pattern `g!` (equivalent to `v`)

```
:g!/^\s*$/d
```

or

```
:v/^\s*$/d
```

Using `\|` as or: Delete all lines except those contain "error", "warn" or "fail"

```
:v/error\|warn\|fail/d
```

## Counting occurrences of a pattern

```
:%s/pattern/gn
```


## Replace space with newline

```
:%s/ /<Ctrl v><Enter>/g
```

## Search

`*`to search for the current word under cursor

`[I` to list all occurrences of word under the cursor (same as `:g/`


for complex search use

```
:g/pattern
```
or
```
:il pattern
```

search "pattern" and open a clickable list

```
:w
;vimgrep /pattern/ %
:copen
```

## When searching:

`.`, `*`, `\`, `[`, `^`, and `$` are metacharacters.

`+`, `?`, `|`, `&`, `{`, `(`, and `)` must be escaped to use their special function.

`\/` is / (use backslash + forward slash to search for forward slash)

`\t` is tab, `\s` is whitespace (space or tab)

`\n` is newline, `\r` is CR (carriage return = Ctrl-M = ^M)

After an opening `[`, everything until the next closing `]` specifies a [/collection](http://vimdoc.sourceforge.net/cgi-bin/help?tag=%2Fcollection). Character ranges can be represented with a `-`; for example a letter a, b, c, or the number 1 can be matched with `[1a-c]`. Negate the collection with `[^` instead of `[`; for example `[^1a-c]` matches any character except a, b, c, or 1.

`\{#\}` is used for repetition. `/foo.\{2\}` will match foo and the two following characters. The `\` is not required on the closing `}` so `/foo.\{2}` will do the same thing.

`\(foo\)` makes a backreference to foo. Parenthesis without escapes are literally matched. Here the `\` is required for the closing `\)`.

## When replacing:

`\r` is newline, `\n` is a null byte (0x00).

`\&` is ampersand (& is the text that matches the search pattern).

`\0` inserts the text matched by the entire pattern

`\1` inserts the text of the first backreference. `\2` inserts the second backreference, and so on.


## Copy or delete matching lines

```
qaq
:g/pattern/y A
:let @+ = @a
```

The first of the following commands deletes all lines containing *pattern*, while the second deletes all lines matching the last search:

```
:g/pattern/d
:g//d
```

Use `:v//d` to delete all lines that do *not* match the last search pattern, or `:v/pattern/d` to delete all lines that do *not* match the given pattern.

## Insert line numbers before each line

```
:%s/^/\=printf('%-4d', line('.'))
```

## Insert something before or after each line

```
gg<Ctrl v>G0A<something><Esc>
gg<Ctrl v>G$A<something><Esc>
```

## Record and execute from file
write all keystrokes in `test.keys`
```
vim -s test.keys myInput.file
```

| magics for saving | and reading and etc. |
| ----------------------------- | --------------------------------------------------- |
| :w >>foo                      | append the whole buffer to a file                   |
| :.w >>foo                     | append current line to a file                       |
| :$r foo                       | read foo into the end of the buffer                 |
| :0r foo                       | read foo into the start, moving existing lines down |
| :.,$w foo                     | write current line and below to a file              |
| :r !ls                        | read ls output into cursor position                 |
| :w !wc                        | send buffer to wc and display output                |
| :.!tr ‘A-Za-z’ ‘N-ZA-Mn-za-m’ | apply ROT-13 to current line                        |
| :w\|so %                      | chain commands: write and then source buffer        |
| :e!                           | throw away unsaved changes, reload buffer           |
| :hide edit foo                | edit foo, hide current buffer if dirty              |

# Sorting columns of text

```
:%!column -t
:%!sort -k2nr    column 2 asnumber reverse
:%!sort -k4 -bk3g    sort by the the 4th column (-k4), followed by the 3rd column, but this time we require a few more switches. We ignore leading blank spaces (b), and this time we sort using a general numeric sort (g).
```

## The `c_CTRL-R` family of maps

1. You need not type variable names when using `:s/`, just use `CTRL-R` `CTRL-W` to insert the word under the cursor into the command line (caveat: this does not escape special characters, like `*` does).
2. After making a small change using `ce`, you can search for the old word using `/` `CTRL-R` `-`. Then use `.` to repeat the change.
3. If you need to make changes to the search pattern `CTRL-R` `/` will place the existing search into the command line.
4. Finally, you can put the contents of any register using `CTRL-R` `{register}`

## Basic Calculation
in insert mode, `Ctrl R = 2 ?+ 2 <CR> `
