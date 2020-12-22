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
gg<Ctrl v>G0I<something><Esc>
gg<Ctrl v>G$A<something><Esc>
```

## Record and execute from file
perform  `test.keys` to file:
```
vim -s test.keys myInput.file
```
or to multiple files:
```
for file in *.txt; do
        vim -s keys "$file"
done
```


## edit multiple files

1. Record the edit motions to a macro.
2. Use `:args filename*.*` to open all files that are to be modified into the buffer.
(Use `:args` to see the list of all files.)
3. `:argdo norm@aw` to apply macro to all files and save changes.
4. `:wal` to make sure everything is saved. 

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
:sort u        sort and remove duplicate
:%!column -t
:%!sort -k2nr    column 2 asnumber reverse
:%!sort -k4 -bk3g    sort by the the 4th column (-k4), followed by the 3rd column, but this time we require a few more switches. We ignore leading blank spaces (b), and this time we sort using a general numeric sort (g).
```

## The `c_CTRL-R` family of maps

1. You need not type variable names when using `:s/`, just use `CTRL-R` `CTRL-W` to insert the word under the cursor into the command line (caveat: this does not escape special characters, like `*` does).
2. After making a small change using `ce sth <Esc>`, you can search for the old word using `/ CTRL-R - <Enter>`. Then use `.` to repeat the change.
3. If you need to make changes to the search pattern `CTRL-R` `/` will place the existing search into the command line.
4. Finally, you can put the contents of any register using `CTRL-R` `{register}` in input mode

## Basic Calculation
in insert mode, `Ctrl R = 2 + 2 <CR> `

## Modifying Registers
`"qp` paste the contents of the register to the current cursor position

`I` enter insert mode at the begging of the pasted line

`^` add the missing motion to return to the front of the line

`<Escape>` return to visual mode

`"qyy yank` this new modified macro back into the q register

`dd` delete the pasted register from the file your editing

## small shortcuts
Swap the current word with the next: `dawwP`

`ea` - insert (append) at the end of the word

Swap the current word with the next: dawwP

`ZZ` or :x  - write (save) and quit

 `ZQ` - quit and throw away unsaved changes

## Replay macro
```
@@
```

## Incrementing numbers

Vim has `Ctrl-X` and `Ctrl-A` commands to decrement and increment numbers. When used with visual mode, you can increment numbers across multiple lines.

If you have these HTML elements:
```
<div id="app-1"></div>
<div id="app-1"></div>
<div id="app-1"></div>
<div id="app-1"></div>
<div id="app-1"></div>
```

It is a bad practice to have several ids having the same name, so let's increment them to make them unique:
- Move your cursor to the *second* "1".
- Start block-wise visual mode and go down 3 lines (`Ctrl-V 3j`). This highlights the remaining  "1"s.
- Run `g Ctrl-A`.

You should see this result:
```
<div id="app-1"></div>
<div id="app-2"></div>
<div id="app-3"></div>
<div id="app-4"></div>
<div id="app-5"></div>
```

`g Ctrl-A` increments numbers on multiple lines. `Ctrl-X/Ctrl-A` can increment letters too. If you run:

```
:set nrformats+=alpha
```

The `nrformats` option instructs Vim which bases are considered as "numbers" for `Ctrl-A` and `Ctrl-X` to increment and decrement. By adding `alpha`, an alphabetical character is now considered as a number. If you have the following HTML elements:
```
<div id="app-a"></div>
<div id="app-a"></div>
<div id="app-a"></div>
<div id="app-a"></div>
<div id="app-a"></div>
```

Put your cursor on the second "app-a". Use the same technique as above (`Ctrl-V 3j` then `g Ctrl-A`) to increment the ids.
```
<div id="app-a"></div>
<div id="app-b"></div>
<div id="app-c"></div>
<div id="app-d"></div>
<div id="app-e"></div>
```

## copy all lines containing args to line 2
```:g/args/t2```

## see a list of editable command histry
```q:```

## go to start / end in visual mod
v_o v_O

