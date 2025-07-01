# Guide to using `dl_library` functions with `essctrl`

This document outlines best practices and common pitfalls when running `dl_library` TCL functions from the shell using the `essctrl` utility.

## Recommended Workflow: Using a Function Library

The Tcl server that `essctrl` communicates with is **stateful**. This means you can define procedures in one command and call them in subsequent commands. This is the most powerful and maintainable way to work with `essctrl` for any non-trivial logic.

The workflow has three steps: **Define**, **Load**, and **Execute**.

### 1. Define Your Functions in a `.tcl` File

For developing and testing, it's recommended to use a dedicated temporary file: `/home/lab/tmp_library.tcl`. Place your reusable procedures here. Remember to use `dl_return` when a procedure needs to return a new `dl_library` list.

**Example `/home/lab/tmp_library.tcl`:**
```tcl
# A library of custom Tcl procedures for essctrl

# @proc create_and_double
# @brief Multiplies a dl_list by 2.
# @param input_list The name of a numeric dl_list.
# @returns The name of a new list containing the results.
proc create_and_double {input_list} {
    set doubled_list [dl_mult $input_list 2]
    # Use dl_return to safely return the new list from the proc.
    dl_return $doubled_list
}

# @proc multiply_by_10
# @brief Multiplies a single number by 10.
# @param value A numeric value.
# @returns The result.
proc multiply_by_10 {value} {
    if {![string is double -strict $value]} {
        error "Input must be a number. Got '$value'."
    }
    set result [expr {$value * 10}]
    return $result
}
```

### 2. Load the Library (Once Per Session)

Load your functions into the server's memory using the "atomic execution" method. You only need to do this once per session for your library file.

```bash
TCL_SCRIPT=$(cat /home/lab/tmp_library.tcl); essctrl -c "$TCL_SCRIPT"
```
The server will now remember these procedures for all subsequent `essctrl` calls.

### 3. Execute Functions as Needed

Now you can make simple, readable calls to your functions from the command line.

```bash
# Call a function that returns a new dl_list
essctrl -c 'dl_tcllist [create_and_double [dl_ilist 5 10 15]]'
# → 10 20 30

# Call a function that returns a simple value
essctrl -c 'return [multiply_by_10 7]'
# → 70
```

This approach keeps your temporary logic organized in a predictable place and your command-line calls clean and focused on the specific task at hand.

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

### Known Limitations & Unstable Features

#### Do NOT use `puts` or File I/O (`open`)

The `essctrl` Tcl environment is highly unstable when `puts` or file I/O commands (like `open`, `close`, `read`) are used. In our testing, using these commands often leads to `too many nested evaluations (infinite loop?)` errors, even in simple scripts.

**The only reliable way to view data is to return it as the final expression of your script.**

- To view `dl_library` lists, use `dl_tcllist`.
- To view simple Tcl variables, use `return`.

### Inspecting Simple Tcl Variables

The `dl_tcllist` command is for `dl_library` list objects. To view standard Tcl variables, the Tcl `return` command must be the last instruction in your script. The `puts` command will not work.

*   **To view a single variable:** Use `return $varName`.
    ```bash
    essctrl -c 'set a 42; return $a'
    # → 42
    ```
*   **To view multiple variables:** Combine them into a Tcl list and `return` the list.
    ```bash
    essctrl -c 'set v1 "hello"; set v2 99; return [list "Value 1:" $v1 "Value 2:" $v2]'
    # → {Value 1:} hello {Value 2:} 99
    ```
*   **To create a log of messages:** For debugging, you can build a list of messages throughout your script using `lappend`. This creates a sequential log of what happened.
    ```tcl
    # Tcl code to build a log
    set log [list]
    lappend log "Starting process..."
    set items_processed 42
    lappend log "Items processed: $items_processed"
    lappend log "Process complete."
    return $log
    ```
    Executing this with `essctrl` would look like:
    ```bash
    essctrl -c 'set log [list]; lappend log "Starting..."; set i 42; lappend log "Items: $i"; lappend log "Done."; return $log'
    # → {Starting...} {Items: 42} Done.
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

## Alternative Script Execution Methods

While using a function library is recommended for most cases, there are other ways to execute scripts from files.

### Method 1: Atomic Execution for One-Off Scripts

If you have a complex script with procedures that you only intend to run once, you can load and execute it in a single command. This is the same method used to load a library. The entire script is evaluated as a single unit, and only the final result is returned.

```bash
TCL_SCRIPT=$(cat your_script.tcl); essctrl -c "$TCL_SCRIPT"
```

### Method 2: Interactive Debugging (Line-by-Line)

This method is excellent for debugging simple, sequential scripts that **do not** contain multi-line procedure (`proc`) definitions. It executes the script as if you were typing it line-by-line into an interactive session, showing the result of *every* command.

You use shell redirection to pipe the script into `essctrl`:

```bash
essctrl < your_script.tcl
``` 
