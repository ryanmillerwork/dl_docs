# Guide to using `dl_library` functions with `essctrl`

This document outlines best practices and common pitfalls when running `dl_library` TCL functions from the shell using the `essctrl` utility.

## Executing Commands

All commands are executed via the `essctrl` command-line tool with the `-c` flag, which takes a string of TCL commands to execute.

```bash
essctrl -c "tcl_command_goes_here"
```

## Viewing List Contents

The `dl_` functions often return dynamic list objects. To view the contents of these lists in a human-readable format, you must wrap the command with `dl_tcllist`.

```bash
# This will output the contents of the list returned by dl_abs
essctrl -c "dl_tcllist [dl_abs [dl_flist -1.1 1.2]]"
```

## Creating Lists

While a generic `dl_create` command exists, it's often better to use type-specific list creation functions to avoid "bad datatype" errors:

*   `dl_ilist` for integer lists (e.g., `dl_ilist 1 2 3`)
*   `dl_flist` for float lists (e.g., `dl_flist 1.0 2.0 3.0`)

## Working with Variables (The Shell Quoting Trap)

A common issue arises from the interaction between the `bash` shell and the TCL interpreter, as both use `$` for variable expansion.

### The Problem

When you use double quotes (`"..."`) in your shell command, `bash` will try to expand any `$` variables *before* passing the command to `essctrl`.

```bash
# INCORRECT - bash will replace $a and $b with empty strings
essctrl -c "set a [dl_flist 0.1 0.2]; set b [dl_flist 1.0 2.0]; dl_tcllist [dl_add $a $b]"
```
This fails because TCL never sees the variables `$a` and `$b`.

### The Solutions

There are three main ways to solve this:

1.  **Use single quotes:** This is often the simplest solution. The shell passes the string to `essctrl` literally, without interpreting the `$`. This avoids the need to escape special characters.

    ```bash
    # CORRECT - The shell passes $a and $b to TCL literally.
    essctrl -c 'set a [dl_flist 0.1 0.2]; set b [dl_flist 1.0 2.0]; dl_tcllist [dl_add $a $b]'
    ```

2.  **Escape the dollar signs:** If you must use double quotes, tell `bash` to treat the `$` as a literal character by escaping it with a single backslash (`\`).

    ```bash
    # CORRECT - The shell passes \$a and \$b to TCL, which then expands them correctly.
    essctrl -c "set a [dl_flist 0.1 0.2]; set b [dl_flist 1.0 2.0]; dl_tcllist [dl_add \$a \$b]"
    ```

3.  **Use Nested Commands:** The most robust method is often to avoid variables altogether and nest the commands directly. This bypasses any shell expansion issues.

    ```bash
    # CORRECT and often preferred for simplicity
    essctrl -c "dl_tcllist [dl_add [dl_flist 0.1 0.2] [dl_flist 1.0 2.0]]"
    ```

## Discovering Command Usage

A useful diagnostic technique for understanding how a `dl_library` function works is to call it without any arguments. The behavior can vary:

1.  **Usage Message:** Many commands will print a helpful usage message that shows the required and optional arguments. This is often the quickest way to learn a command's syntax.

    ```bash
    # This will print the usage for dl_add
    essctrl -c "dl_add"
    # Output: usage: dl_add list1 list2 [list3...]
    ```

2.  **Default Behavior:** Some commands might execute a default behavior. For example, list creation functions often create an empty list.

    ```bash
    # This creates an empty float list
    essctrl -c "dl_flist"
    # Output: %list...% 
    ```

=== How to See Return Values ===

A critical concept for testing is that both `essctrl` and the direct `netcat`
interface return the value of the *last evaluated expression*.

The common Tcl command `puts` **should not be used**, as it will not print
to the standard output in either interface and will result in seeing no
output.

**Correct Method:**

To see the result of a command, ensure it is the last command in your script.

```bash
# Get a datatype with essctrl
essctrl -c 'set mylist [dl_ilist 1]; dl_datatype $mylist'
# → long

# Get a datatype with netcat
echo 'set mylist [dl_ilist 1]; dl_datatype $mylist' | nc -N 127.0.0.1 2570
# → long
```

To see the contents of a list, the last command should be `dl_tcllist`.

```bash
essctrl -c 'set res [dl_cumsum [dl_flist 1 2 3]]; dl_tcllist $res'
# → 1.0 3.0 6.0
```

**Incorrect Method:**

Using `puts` will prevent you from seeing the result.

```bash
# This will produce NO output
essctrl -c 'set mylist [dl_ilist 1]; puts [dl_datatype $mylist]'
```

While both methods work, `netcat` can be simpler for scripts containing
shell-sensitive characters like `!`, as it avoids complex quoting. For most
cases, `essctrl -c '...'` is sufficient. 
