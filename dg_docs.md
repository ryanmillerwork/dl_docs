=== dg_addExistingList (Manipulation) ===
Synopsis
    dg_addExistingList dyngroup list_name [new_name]

Brief
    Moves an existing standalone DynList into a DynGroup. After this operation, the original standalone list is deleted and can no longer be accessed by its original name.

Inputs
    • dyngroup …… The name of the destination DynGroup.
    • list_name …… The name of the existing standalone DynList to add.
    • new_name …… Optional. A new name for the list within the DynGroup. If not provided, the list keeps its original name.

Returns
    • The fully qualified name of the new list within the group (e.g., `myGroup:myList`).

Errors
    • TCL_ERROR if the `dyngroup` or `list_name` cannot be found.
    • TCL_ERROR: `dg_addNewList: dynlist "<new_name>" already exists in dyngroup "<dyngroup>"` if `new_name` is provided and already exists in the `dyngroup`.

Usage Note
    This command is difficult to use interactively via `essctrl` because of challenges in passing dynamically generated list names. The more common pattern is to create lists directly inside a group (e.g., `dl_set myGroup:myList [dl_ilist]`).

Example
    # Conceptual example
    # 1. Create a group
    dg_create myGroup

    # 2. Create a standalone list
    set mylist [dl_ilist 1 2 3]

    # 3. Add the list to the group with a new name
    dg_addExistingList myGroup \$mylist myNewList

    # 4. The list is now accessible as 'myGroup:myNewList'
    #    and the original \$mylist is gone.

See also
    dg_copyExistingList, dg_addNewList, dg_remove, dl_set

=== dg_addNewList (Manipulation) ===
Synopsis
    dg_addNewList dyngroup name [[datatype] [size]]

Brief
    Creates and appends a new, empty DynList to a DynGroup.

Inputs
    • dyngroup …… The name of the destination DynGroup.
    • name ………… The name for the new list within the group.
    • datatype …… Optional. A string for the list's data type (e.g., `long`, `float`). Defaults to `long` if not provided.
    • size ………… Optional. An integer specifying the initial memory allocation capacity. This does NOT populate the list; the list is always created empty.

Returns
    • The fully qualified name of the new list within the group (e.g., `myGroup:newList`).

Errors
    • TCL_ERROR if the `dyngroup` is not found.
    • TCL_ERROR if a list with the same `name` already exists in the group.
    • TCL_ERROR: `dg_addNewList: bad datatype` if the `datatype` is invalid.

Example
    # Create a group
    dg_create myGroup

    # Add a new list with default type (long)
    dg_addNewList myGroup listOne

    # Add a new integer list with an initial capacity of 10
    dg_addNewList myGroup listTwo long 10

    # Verify list creation
    dl_tcllist [dg_listnames myGroup]
    # → listOne listTwo

    # Verify the type and that the list is empty despite the 'size' argument
    dl_datatype myGroup:listOne
    # → long
    dl_length myGroup:listOne
    # → 0
    dl_datatype myGroup:listTwo
    # → long
    dl_length myGroup:listTwo
    # → 0

See also
    dg_addExistingList, dg_remove, dl_create

=== dg_append (Manipulation) ===
Synopsis
    dg_append onto_group from_group

Brief
    Appends the contents of one DynGroup to another. For this to succeed, both groups must be "compatible," meaning they must contain the exact same set of list names. The `from_group` is not modified or deleted.

Inputs
    • onto_group … The name of the DynGroup to be modified (the destination).
    • from_group … The name of the DynGroup to read from (the source).

Side Effects
    • The lists within `onto_group` are modified to contain the appended data from `from_group`.

Errors
    • TCL_ERROR if either group is not found.
    • TCL_ERROR: `incompatible dyngroups` if the groups do not have the same list names (schema).

Example
    # Create two groups with identical list names
    dg_create group1; dg_create group2
    dg_addNewList group1 listA; dg_addNewList group1 listB
    dg_addNewList group2 listA; dg_addNewList group2 listB

    # Add data to the lists
    dl_append group1:listA 10; dl_append group1:listB "a"
    dl_append group2:listA 20; dl_append group2:listB "b"

    # group1:listA is now {10}
    # group1:listB is now {"a"}

    # Append group2 onto group1
    dg_append group1 group2

    # Verify the appended contents
    dl_tcllist group1:listA
    # → 10 20
    dl_tcllist group1:listB
    # → a b

    # The source group still exists
    dg_exists group2
    # → 1

See also
    dg_create, dg_copy

=== dg_copy (Creation) ===
Synopsis
    dg_copy from_group to_group

Brief
    Creates a new DynGroup (`to_group`) as a complete and independent copy of an existing DynGroup (`from_group`). The command fails if `to_group` already exists.

Inputs
    • from_group … The name of the existing DynGroup to copy.
    • to_group …… The name for the new destination DynGroup.

Returns
    • The name of the new DynGroup (`to_group`).

Side Effects
    • A new DynGroup is created with copies of all lists and data from the source.
    • The source group (`from_group`) is not modified.

Errors
    • TCL_ERROR if `from_group` cannot be found.
    • TCL_ERROR: `group <to_group> already exists` if the destination group name is already in use (including if it's the same as `from_group`). This contradicts some older documentation.

Example
    # Create a source group with data
    dg_create groupA
    dg_addNewList groupA list1 long
    dl_append groupA:list1 10 20

    # Copy the group
    dg_copy groupA groupB

    # Verify the new group's contents
    dl_tcllist [dg_listnames groupB]
    # → list1
    dl_tcllist groupB:list1
    # → 10 20

    # The source group is unaffected
    dg_exists groupA
    # → 1

    # Attempting to overwrite fails
    dg_copy groupA groupB
    # → dg_copy: group groupB already exists

See also
    dg_create, dg_append, dg_addExistingList

=== dg_create (Creation) ===
Synopsis
    dg_create [groupname] [increment]

Brief
    Creates a new, empty DynGroup.

Inputs
    • groupname … Optional. A string name for the new group. If not provided, a unique name is generated automatically (e.g., `group0`, `group1`, etc.).
    • increment … Optional. An integer providing a hint for the initial memory allocation size. This does not change the group's behavior, only its internal performance characteristics.

Returns
    • The name of the newly created DynGroup.

Errors
    • TCL_ERROR: `tclPutGroup: group <groupname> already exists` if the specified `groupname` is already in use.

Example
    # Create a named group
    dg_create my_database

    # Verify it exists
    dg_exists my_database
    # → 1

    # Create an automatically named group
    # This will return "group0" (assuming no other "groupN" names exist)
    dg_create

    # Attempting to create a group that already exists will fail
    dg_create my_database
    # → tclPutGroup: group my_database already exists

See also
    dg_copy, dg_delete, dg_addExistingList, dg_addNewList

=== dg_delete (Memory Management) ===
Synopsis
    dg_delete dyngroup_name1 [dyngroup_name2 ...] | dg_delete ALL

Brief
    Deletes one or more DynGroups, or all existing DynGroups.

Inputs
    • dyngroup_nameN … One or more names of DynGroups to delete.
    • ALL …………………… A literal keyword to delete all DynGroups currently in memory.

Side Effects
    • The specified DynGroups and all the lists they contain are deallocated and their names are removed.

Errors
    • TCL_ERROR: `dg_delete: dyngroup "<name>" not found` if a specified group name does not exist. This error is NOT thrown when using the `ALL` keyword, which will complete silently even if no groups exist.

Example
    # Setup
    dg_create g1; dg_create g2; dg_create g3

    # Delete a single group
    dg_delete g1
    dg_dir
    # → g2 g3

    # Delete multiple groups
    dg_delete g2 g3
    dg_dir
    # →

    # Delete all groups
    dg_create g1; dg_create g2
    dg_delete ALL
    dg_dir
    # →

See also
    dg_create, dg_clean

=== dg_dir (Inspection) ===
Synopsis
    dg_dir

Brief
    Returns a Tcl list of the names of all currently defined DynGroups.

Inputs
    None.

Returns
    • A Tcl list (space-delimited string) containing the names of all existing DynGroups. If no groups exist, it returns an empty string.

Example
    # With no groups defined
    dg_dir
    # → "" (empty string)

    # With groups defined
    dg_create my_group
    dg_create another_group
    dg_dir
    # → my_group another_group

    # Capturing the list in a Tcl variable
    set group_list [dg_dir]
    puts "Found [llength $group_list] groups"

See also
    dg_exists, dg_listnames

=== dg_dump (Inspection) ===
Synopsis
    dg_dump dyngroup [fileID [separator]]

Brief
    Prints the contents (data rows) of a DynGroup to the screen or a Tcl channel. This command only prints the data; it does not print a header row with the list names.

Inputs
    • dyngroup …… The name of the DynGroup to dump.
    • fileID ………… Optional. The name of an open Tcl channel to write to. If not provided, output is sent to `stdout`.
    • separator …… Optional. The ASCII integer value of the character to use as a separator between columns. Defaults to `9` (Tab). This argument can only be used if `fileID` is also provided.

Usage Note
    When a `separator` is used, string values in the output will be automatically enclosed in double quotes.

Returns
    • This command does not return a value.

Errors
    • TCL_ERROR if the `dyngroup` is not found.
    • TCL_ERROR: `can not find channel named "<fileID>"` if the provided channel is not valid.

Example
    # Setup a group with two lists and two rows
    dg_create my_data
    dg_addNewList my_data col_A string
    dg_addNewList my_data col_B string
    dl_append my_data:col_A "row1_A" "row2_A"
    dl_append my_data:col_B "row1_B" "row2_B"

    # Dump to stdout with default tab separator
    dg_dump my_data
    # → row1_A    row1_B
    #   row2_A    row2_B

    # Conceptual example for writing to a file with a comma separator
    # set chan [open "output.csv" w]
    # dg_dump my_data \$chan 44
    # close \$chan

See also
    dg_dumpListNames, dg_write, dl_dump

=== dg_exists (Inspection) ===
Synopsis
    dg_exists name

Brief
    Checks if a DynGroup with the given name exists.

Inputs
    • name … The string name of the DynGroup to check.

Returns
    • `1` if the group exists.
    • `0` if the group does not exist.

Errors
    • TCL_ERROR: `usage: dg_exists name` if the `name` argument is missing.

Example
    dg_create my_group
    dg_exists my_group
    # → 1

    dg_exists non_existent_group
    # → 0

See also
    dg_dir, dl_exists

=== dg_fromString (Creation) ===
Synopsis
    dg_fromString string_variable_name new_group_name

Brief
    Creates a new DynGroup by deserializing a string that was previously created with `dg_toString`.

Inputs
    • string_variable_name … The name of the Tcl variable holding the binary string representation of a DynGroup.
    • new_group_name ………… The name for the newly created DynGroup.

Returns
    • The `new_group_name` on success.

Errors
    • TCL_ERROR if the data in the source variable is not valid Base64 or is a malformed group string.

Usage Note
    This command overwrites any existing DynGroup that has the same `new_group_name`. It is part of a serialization pair with `dg_toString`. While care must be taken to manage variables within a single `essctrl` call, a round-trip operation is straightforward. See the example in `dg_toString`.

See also
    dg_toString, dg_fromString64, dg_create

=== dg_toString (Serialization) ===
Synopsis
    dg_toString dyngroup varname

Brief
    Serializes an entire DynGroup into a binary string and stores it in a Tcl variable.

Inputs
    • dyngroup … The name of the DynGroup to serialize.
    • varname …… The name of the Tcl variable that will store the resulting binary string.

Returns
    • The size of the created binary string in bytes.

Side Effects
    • The Tcl variable `varname` is created or overwritten with the binary data.

Errors
    • TCL_ERROR if the `dyngroup` is not found.

Usage Note
    This command is part of a serialization pair with `dg_fromString`. Using this pair from the `essctrl` command line is challenging. These commands are more reliably used within Tcl scripts.

Example
    # Conceptual example of a serialization round-trip
    # 1. Create a source group
    dg_create source_g
    dg_addNewList source_g data string
    dl_append source_g:data "val1"

    # 2. Serialize to a variable named 'group_data'
    dg_toString source_g group_data

    # 3. Deserialize from the variable to a new group
    dg_fromString group_data dest_g

    # 4. Verify
    # dl_tcllist dest_g:data → val1

See also
    dg_fromString, dg_toString64, dg_write

=== dg_json (Serialization) ===
Synopsis
    dg_json dyngroup [row_index]
    dg_toJSON dyngroup [row_index]

Brief
    Converts a DynGroup, or a single row from it, into a JSON string. `dg_toJSON` is an alias for this command.

Inputs
    • dyngroup …… The name of the DynGroup to serialize.
    • row_index …… Optional. The integer index of a single row to serialize.

Returns
    A JSON string. The format depends on whether a `row_index` is provided:
    • **Without `row_index`**: A single JSON object is returned, where keys are the list names and values are arrays of the entire columns (a "columnar" format).
    • **With `row_index`**: A single JSON object representing just that row is returned.
    • If `row_index` is out of bounds, an empty JSON object `{}` is returned.

Errors
    • TCL_ERROR if the `dyngroup` is not found.

Example
    # Setup a group
    dg_create stats
    dg_addNewList stats id long
    dg_addNewList stats name string
    dl_append stats:id 1 2
    dl_append stats:name "foo" "bar"

    # Get the whole group as columnar JSON
    dg_json stats
    # → {"id":[1,2],"name":["foo","bar"]}

    # Get a single row as a standard JSON object
    dg_json stats 0
    # → {"id":1,"name":"foo"}

    # An out-of-bounds index returns an empty object
    dg_json stats 99
    # → {}

See also
    dg_fromString, dg_toString, dl_json

=== dg_listnames (Inspection) ===
Synopsis
    dg_listnames dyngroup

Brief
    Returns a new DynList containing the names of all lists within a specified DynGroup.

Inputs
    • dyngroup … The name of the DynGroup to inspect.

Returns
    • A new DynList of type `string`. The contents of this list are the names of the lists inside the `dyngroup`. Because this returns a DynList object, you typically need to process it with another command (like `dl_tcllist` or `dl_delete`).

Errors
    • TCL_ERROR if the `dyngroup` is not found.

Example
    dg_create my_group
    dg_addNewList my_group list_A
    dg_addNewList my_group list_B

    # Get the list of names and print it
    dl_tcllist [dg_listnames my_group]
    # → list_A list_B

    # The result is a new list that must be managed
    set names [dg_listnames my_group]
    # ... use the $names list ...
    dl_delete $names

See also
    dg_dir, dg_tclListnames, dg_dumpListNames

=== dg_read (Creation) ===
Synopsis
    dg_read file [new_group_name]

Brief
    Reads a DynGroup from a file, creating a new group in memory. It can automatically decompress `.dgz` (GZip) and `.lz4` files.

Inputs
    • file …………………… The path to the file to read.
    • new_group_name … Optional. A name to assign to the newly created group. If not provided, the group's original name from the file is used.

Returns
    • The name of the newly created DynGroup.

Errors
    • TCL_ERROR if the `file` cannot be found or read.
    • TCL_ERROR if the file is not a valid DynGroup format.

Usage Note
    If `new_group_name` is provided and a group with that name already exists, this command will overwrite the existing group without warning.

See also
    dg_write, dg_create, dg_fromString

=== dg_write (Serialization) ===
Synopsis
    dg_write dyngroup [filename]

Brief
    Writes a DynGroup to a file. Compression is determined by the filename extension.

Inputs
    • dyngroup … The name of the DynGroup to write.
    • filename … Optional. The path to the output file.
                 - If not provided, defaults to `<dyngroup>.dgz` (GZip compressed).
                 - If ends with `.dgz` or has `z` as the last letter of the extension, uses GZip.
                 - If ends with `.lz4`, uses LZ4 compression.
                 - Otherwise, writes as an uncompressed binary file.

Returns
    • This command does not return a value.

Errors
    • TCL_ERROR: `dyngroup "<dyngroup>" not found` if the `dyngroup` is not found.
    • TCL_ERROR if the `filename` cannot be written to.

Usage Note
    When providing a `filename`, it is strongly recommended to use an absolute path (e.g., `/tmp/my_group.dg`) to ensure the file is created in a predictable location, as the command's working directory may not be what you expect.

Example
    # Create a group
    dg_create my_group
    dg_addNewList my_group data
    dl_append my_group:data 1 2 3

    # Write to a GZip compressed file using an absolute path
    dg_write my_group /tmp/my_group.dgz

    # Write to an uncompressed file
    dg_write my_group /tmp/my_group.dg

    # Read it back in
    dg_delete my_group
    dg_read /tmp/my_group.dg
    dl_tcllist my_group:data
    # → 1 2 3

See also
    dg_read, dg_toString, dg_json

=== dg_remove (Manipulation) ===
Synopsis
    dg_remove dyngroup list_name

Brief
    Removes a specific list from a DynGroup. The list and all its data are permanently deleted.

Inputs
    • dyngroup …… The name of the DynGroup to modify.
    • list_name …… The name of the list to remove from the group.

Returns
    • The name of the modified `dyngroup`.

Errors
    • TCL_ERROR if the `dyngroup` is not found.
    • TCL_ERROR: `dg_remove: list "<list_name>" not found in group "<dyngroup>"` if the `list_name` is not found within the group.

Example
    # Setup a group with two lists
    dg_create my_group
    dg_addNewList my_group list_A
    dg_addNewList my_group list_B
    dg_listnames my_group
    # → list_A list_B

    # Remove one of the lists
    dg_remove my_group list_A
    dg_listnames my_group
    # → list_B

See also
    dg_addExistingList, dg_addNewList, dg_delete

=== dg_rename (Manipulation) ===
Synopsis
    dg_rename old_name new_name

Brief
    Renames either a DynGroup or a specific list within a DynGroup.

Inputs
    • old_name … The current name of the item to rename.
                 - To rename a group: `group_name`
                 - To rename a list: `group_name:list_name` or `group_name:list_index`
    • new_name … The new string name for the item.

Returns
    • The `new_name` on success.

Errors
    • TCL_ERROR if `old_name` (group or list) is not found.
    • TCL_ERROR: `dg_rename: list "<group_name>:<new_name>" already exists` if attempting to rename a list to a `new_name` that already exists in the group.

Example
    # --- Group Rename ---
    dg_create group_A
    dg_rename group_A group_B
    dg_exists group_B  ;# → 1
    dg_exists group_A  ;# → 0

    # --- List Rename by Name ---
    dg_addNewList group_B list_X
    # group_B now contains {list_X}
    dg_rename group_B:list_X list_Y
    # group_B now contains {list_Y}

    # --- List Rename by Index ---
    # list_Y is at index 0
    dg_rename group_B:0 list_Z
    # group_B now contains {list_Z}

See also
    dg_copy, dg_delete

=== dg_reset (Manipulation) ===
Synopsis
    dg_reset dyngroup_name1 [dyngroup_name2 ...]

Brief
    Resets one or more DynGroups by clearing all data from their lists. This command makes all lists within the group empty (length 0), but it does **not** remove the lists themselves from the group.

Inputs
    • dyngroup_nameN … One or more names of DynGroups to reset.

Side Effects
    • All data is cleared from all lists within the specified group(s). The groups and their list schemas remain.

Returns
    • The name of the last DynGroup processed.

Errors
    • TCL_ERROR: `dyngroup "<dyngroup_name>" not found` if any specified `dyngroup_name` is not found.

Example
    # Setup a group with a list containing data
    dg_create my_group
    dg_addNewList my_group my_list
    dl_append my_group:my_list 1 2 3
    dl_length my_group:my_list
    # → 3

    # Reset the group
    dg_reset my_group

    # The group and its list still exist...
    dg_exists my_group          ;# → 1
    dl_tcllist [dg_listnames my_group] ;# → my_list

    # ...but the list is now empty
    dl_length my_group:my_list
    # → 0

See also
    dg_delete, dg_remove, dg_create

=== dg_tempname (Utility) ===
Synopsis
    dg_tempname
    dg_tmpname

Brief
    Generates and returns a unique, temporary name suitable for a new DynGroup. `dg_tmpname` is an alias for this command.

Details
    This command uses a global, incrementing counter to produce names in the format `groupN` (e.g., `group0`, `group1`). It does **not** create the group, it only provides the name. This same counter is used by other commands that generate default names, such as `dg_create` when called without arguments.

Inputs
    None.

Returns
    • A unique string name.

Example
    # Get a temporary name
    dg_tempname
    # → group0

    # Get another one
    dg_tmpname
    # → group1

    # The counter is shared with other commands
    dg_create
    # (creates group2 and returns "group2")

    # The next temp name will be "group3"
    dg_tempname
    # → group3

See also
    dg_create, dg_clean

=== dg_fromString64 (Creation) ===
Synopsis
    dg_fromString64 base64_string_varname new_group_name

Brief
    Creates a new DynGroup by deserializing a Base64 encoded string that was previously created with `dg_toString64`.

Inputs
    • base64_string_varname … The name of the Tcl variable holding the Base64 string representation of a DynGroup.
    • new_group_name ……………… The name for the newly created DynGroup.

Returns
    • The `new_group_name` on success.

Errors
    • TCL_ERROR if the data in the source variable is not valid Base64 or is a malformed group string.

Usage Note
    This command overwrites any existing DynGroup that has the same `new_group_name`. It is part of a serialization pair with `dg_toString64`. All operations must be performed in a single `essctrl` call for the Tcl variables to be passed correctly.

See also
    dg_toString64, dg_fromString, dg_create

=== dg_toString64 (Serialization) ===
Synopsis
    dg_toString64 dyngroup varname

Brief
    Serializes an entire DynGroup into a Base64 encoded string and stores it in a Tcl variable.

Inputs
    • dyngroup … The name of the DynGroup to serialize.
    • varname …… The name of the Tcl variable that will store the resulting Base64 string.

Returns
    • The length of the created Base64 string.

Side Effects
    • The Tcl variable `varname` is created or overwritten with the Base64 string data.

Errors
    • TCL_ERROR if the `dyngroup` is not found.

Usage Note
    This command is part of a serialization pair with `dg_fromString64`. All operations must be performed in a single `essctrl` call for the Tcl variables to be passed correctly.

Example
    # Create a source group, serialize it, and restore it to a new group
    # all within a single essctrl call. The final `list` command is used
    # to bundle the results into a single string that essctrl can print.
    essctrl -c ' \\
        dg_delete ALL; \\
        dg_create source_g; \\
        dg_addNewList source_g data int; \\
        dl_append source_g:data 1 2 3; \\
        \\
        # Serialize to a variable named b64_data
        dg_toString64 source_g b64_data; \\
        \\
        # Deserialize to a new group
        dg_fromString64 $b64_data dest_g; \\
        \\
        # Verify the result by returning a Tcl list of the group dir,
        # the list names in the new group, and the data itself.
        list [dg_dir] [dl_tcllist [dg_listnames dest_g]] [dl_tcllist dest_g:data] \\
    '
    # → {source_g dest_g} data {1 2 3}

See also
    dg_fromString64, dg_toString, dg_json

=== dg_clean (Memory Management) ===
Synopsis
    dg_clean

Brief
    Deletes all temporary DynGroups, which are groups with names prefixed by "group" (e.g., `group0`, `group12`). This is useful for cleaning up groups created automatically by commands like `dg_create` without a name.

Inputs
    None.

Side Effects
    • Any DynGroup with a name starting with "group" is deallocated and removed.

Errors
    • This command does not produce an error if no temporary groups exist.

Example
    # Create two temporary groups and one named group
    dg_create
    # → group0
    dg_create
    # → group1
    dg_create my_database
    # → my_database

    dg_dir
    # → group0 group1 my_database

    # Clean the temporary groups
    dg_clean

    dg_dir
    # → my_database

See also
    dg_delete, dg_tempname

=== dg_copyExistingList (Manipulation) ===
Synopsis
    dg_copyExistingList dyngroup list_name [new_name]

Brief
    Copies a standalone DynList into a DynGroup. The original standalone list is unaffected.

Inputs
    • dyngroup …… The name of the destination DynGroup.
    • list_name …… The name of the existing standalone DynList to copy.
    • new_name …… Optional. A new name for the list within the DynGroup. If not provided, the list keeps its original name.

Returns
    • The fully qualified name of the new list within the group (e.g., `myGroup:myList`).

Errors
    • TCL_ERROR if the `dyngroup` or `list_name` cannot be found.
    • TCL_ERROR if `new_name` is provided and already exists in the `dyngroup`.

Example
    # 1. Create a group and a standalone list
    dg_create myGroup
    set original_list [dl_ilist 10 20]

    # 2. Copy the list into the group with a new name
    dg_copyExistingList myGroup $original_list copied_list
    # → myGroup:copied_list

    # 3. Verify the new list exists in the group
    dl_tcllist [dg_listnames myGroup]
    # → copied_list
    dl_tcllist myGroup:copied_list
    # → 10 20

    # 4. Verify the original list still exists
    dl_exists $original_list
    # → 1

See also
    dg_addExistingList, dg_addNewList, dg_remove

=== dg_dumpListNames (Inspection) ===
Synopsis
    dg_dumpListNames dyngroup [fileID]

Brief
    Prints the names of the lists within a DynGroup to the screen or a Tcl channel, along with their column index.

Inputs
    • dyngroup …… The name of the DynGroup to inspect.
    • fileID ………… Optional. The name of an open Tcl channel to write to. If not provided, output is sent to `stdout`.

Returns
    • This command does not return a value.

Errors
    • TCL_ERROR if the `dyngroup` is not found.
    • TCL_ERROR: `can not find channel named "<fileID>"` if the provided channel is not valid.

Usage Note
    When using the `fileID` option with `essctrl`, it is recommended to use an absolute file path (e.g., `/tmp/output.txt`) to avoid potential issues with the command's working directory.

Example
    # Setup a group
    dg_create my_data
    dg_addNewList my_data first_column
    dg_addNewList my_data second_column

    # Dump the list names to stdout
    dg_dumpListNames my_data
    # → %%%
    #   %%% Dyngroup:   my_data
    #   %%%
    #   %%% Col  0:     first_column
    #   %%% Col  1:     second_column

    # Conceptual example for writing to a file
    # set chan [open "/tmp/list_names.txt" w]
    # dg_dumpListNames my_data $chan
    # close $chan

See also
    dg_dump, dg_listnames, dg_tclListnames

=== dg_tclListnames (Inspection) ===
Synopsis
    dg_tclListnames dyngroup

Brief
    Returns a Tcl list (a space-delimited string) containing the names of all lists within a specified DynGroup.

Details
    This command is a convenient alternative to `dg_listnames` because it returns a simple Tcl list directly, which can be used in loops or other Tcl commands without needing to be converted with `dl_tcllist` or freed with `dl_delete`.

Inputs
    • dyngroup … The name of the DynGroup to inspect.

Returns
    • A Tcl list (string) of the list names. If the group is empty, it returns an empty string.

Errors
    • TCL_ERROR if the `dyngroup` is not found.

Example
    # Setup a group
    dg_create my_group
    dg_addNewList my_group list_A
    dg_addNewList my_group list_B

    # Get the list of names
    dg_tclListnames my_group
    # → list_A list_B

    # The result can be used directly in a Tcl loop
    # foreach name [dg_tclListnames my_group] {
    #   puts "Found list: $name"
    # }

See also
    dg_listnames, dg_dir, dg_dumpListNames

=== dg_select (Manipulation) ===
Synopsis
    dg_select dyngroup selection_mask_list

Brief
    Filters a DynGroup **in-place** using a boolean mask. For each list within the group whose length matches the mask's length, this command filters its elements, keeping only those where the corresponding mask value is `1`.

Inputs
    • dyngroup ……………… The name of the DynGroup to modify.
    • selection_mask_list … A numeric DynList of 0s and 1s.

Side Effects
    • The `dyngroup` is modified in-place.
    • Only lists whose length exactly matches the length of the `selection_mask_list` are affected. Other lists are ignored.

Returns
    • The name of the modified `dyngroup`.

Errors
    • TCL_ERROR if the `dyngroup` or `selection_mask_list` is not found.

Example
    # Create a group with lists of different lengths
    dg_create my_group
    dl_set my_group:four_items [dl_ilist 10 20 30 40]
    dl_set my_group:five_items [dl_slist a b c d e]
    dl_set my_group:also_four [dl_flist 1.1 2.2 3.3 4.4]

    # Create a mask of length 4
    set mask [dl_ilist 1 0 1 0]

    # Perform the in-place selection
    dg_select my_group $mask

    # Check the results: only the lists with length 4 were filtered
    # dl_tcllist my_group:four_items → 10 30
    # dl_tcllist my_group:also_four → 1.1 3.3
    # dl_tcllist my_group:five_items → a b c d e (unchanged)

See also
    dg_choose, dl_select

=== dg_choose (Creation) ===
Synopsis
    dg_choose source_group index_list [ref_list_name]

Brief
    Creates a new DynGroup by choosing rows from a `source_group` using a list of 0-based indices. This command is useful for creating a new group that is a subset or re-ordered version of another.

Inputs
    • source_group …… The name of the group to read from.
    • index_list ………… A DynList of integer indices to select. Repeated indices are allowed.
    • ref_list_name …… WARNING: This optional argument appears to be non-functional in testing and is ignored.

Returns
    • The name of a **new, temporary DynGroup** containing the selected rows. This group typically needs to be renamed with `dg_rename`.

Errors
    • TCL_ERROR if `source_group` or `index_list` cannot be found.
    • TCL_ERROR if `index_list` contains an out-of-bounds index.

Example
    # Create a source group
    dg_create source
    dl_set source:names [dl_slist alice bob carol dave]
    dl_set source:scores [dl_ilist 100 95 80 98]

    # Create a list of indices to choose
    set indices [dl_ilist 3 0 0 1]

    # Create a new group by choosing rows
    set temp_g [dg_choose source $indices]

    # Rename the new group
    dg_rename $temp_g chosen_subset

    # Check the results
    # dl_tcllist chosen_subset:names  → dave alice alice bob
    # dl_tcllist chosen_subset:scores → 98 100 100 95

See also
    dg_select, dl_choose
