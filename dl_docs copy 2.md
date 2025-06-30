=== dl_abs (Arithmetic) ===
Synopsis
    dl_abs <list> | dl_abs <number>

Brief
    Computes the element-wise absolute value of a list or the absolute value of a single number. Returns a new list containing the result(s).

Inputs
    • Type …… DynList_or_Number  
    • Length … 1  
    • Element types … numeric  

Returns
    • Type …………… DynList (new object)  
    • Element type … Same as input (or float if input is mixed or float)  

Errors
    • TCL_ERROR: `dl_abs: invalid list operand` if the list is not numeric.  
    • TCL_ERROR if the input list is not found.  

Example
    dl_tcllist [dl_abs [dl_flist -1.1 1.2]]   ;# → 1.1 1.2
    dl_tcllist [dl_abs -5.5]                  ;# → 5.5

See also
    dl_negate, dl_sign 


=== dl_acos (Arithmetic / Trigonometric) ===
Synopsis
    dl_acos <list> | dl_acos <number>

Brief
    Computes the element-wise arc cosine of a list or a single number.
    Input values must be between -1.0 and 1.0. Values outside this range
    will return `NaN` (Not a Number). Returns a new list of floats (radians).

Inputs
    • Type …………… DynList_or_Number
    • Length ……… 1
    • Element types … numeric
    • Constraints … Elements must be in the range [-1.0, 1.0]. Values
                    outside this range will result in `NaN`.

Returns
    • Type …………… DynList (new object)
    • Element type … float
    • Notes ………… Values are in radians. An input value outside the valid
                    domain of acos (e.g., > 1.0) will produce a `NaN`
                    element in the output.

Errors
    • TCL_ERROR if the input list is not found or is not numeric.

Example
    dl_tcllist [dl_acos [dl_flist 0 0.5 1.1]]   ;# → 1.5708 1.0472 NaN

See also
    dl_cos, dl_asin, dl_atan 


=== dl_add (Arithmetic) ===
Synopsis
    dl_add <list1> <list2> [<list3>...] | dl_add <list> <number>

Brief
    Performs element-wise addition of two or more lists, or a list and a
    scalar number. Returns a new list.

Inputs
    • Type …………… DynList_or_Number
    • Length ……… 2+
    • Element types … numeric
    • Constraints … Inputs must be numeric. All list inputs must have the
                    same length. If one input is a scalar, it is added
                    to each element of the other list(s).

Returns
    • Type …………… DynList (new object)
    • Element type … Numeric (promoted to float if mixed types)

Errors
    • TCL_ERROR: `dl_add: unable to combine ...` if lists have different
      lengths or contain non-numeric data.
    • TCL_ERROR if an input list is not found.

Example
    dl_tcllist [dl_add [dl_ilist 1 2 3] [dl_ilist 10 20 30]]   ;# → 11 22 33
    dl_tcllist [dl_add [dl_ilist 1 2 3] 5]                      ;# → 6 7 8

See also
    dl_sub, dl_mult, dl_div, dl_increment 


=== dl_and (Logical) ===
Synopsis
    dl_and <list1> <list2> [<list3>...] | dl_and <list> <number>

Brief
    Performs element-wise logical AND on numeric lists. For each element,
    non-zero numbers are treated as true and zero as false. Returns a new
    list of 0s and 1s.

Inputs
    • Type …………… DynList_or_Number
    • Length ……… 2+
    • Element types … numeric
    • Constraints … Inputs must be numeric. All list inputs must have the
                    same length. If one input is a scalar, it is evaluated
                    against each element of the other list(s).

Returns
    • Type …………… DynList (new object)
    • Element type … integer
    • Notes ………… Result contains 0 (false) or 1 (true).

Errors
    • TCL_ERROR: `dl_and: unable to compare ...` if lists have different lengths.
    • TCL_ERROR if an input list is not found.
    • Note: The documented error for non-numeric string data was not
      reproducible. Instead, invalid operand types may produce a usage message.

Example
    dl_tcllist [dl_and [dl_ilist 5 0 -1] [dl_ilist 2 2 0]]   ;# → 1 0 0
    dl_tcllist [dl_and [dl_ilist 1 0 1 0] 1]                  ;# → 1 0 1 0

See also
    dl_or, dl_not, dl_eq


=== dl_andIndex (Logical / Indexing) ===
Synopsis
    dl_andIndex <list1> <list2> | dl_andIndex <list> <number>

Brief
    Performs element-wise logical AND on two numeric lists and returns a
    new list containing the indices where the result is true.

Inputs
    • Type …………… DynList_or_Number
    • Length ……… 2
    • Element types … numeric
    • Constraints … Inputs must be numeric. The two inputs must be two lists
                    of the same length, or one list and one scalar number.

Returns
    • Type …………… DynList (new object)
    • Element type … integer
    • Notes ………… A list of 0-based indices where the logical AND is true.

Errors
    • TCL_ERROR: `dl_andIndex: unable to compare ...` if lists have different lengths.
    • TCL_ERROR if an input list is not found.
    • Note: The documented error for non-numeric string data was not
      reproducible. Instead, invalid operand types may produce a usage message.

Example
    dl_tcllist [dl_andIndex [dl_ilist 1 0 1 0 1] [dl_ilist 1 1 0 0 1]]  ;# → 0 4
    dl_tcllist [dl_andIndex [dl_ilist 1 0 1 0] 1]                       ;# → 0 2

See also
    dl_and, dl_orIndex, dl_eqIndex, dl_select


=== dl_append (Manipulation) ===
Synopsis
    dl_append <list_name> <value> [<value2> ...]

Brief
    Appends one or more values to the end of a list. This command modifies
    the list in-place and does not return a value.

Inputs
    • Type …………… varied
    • Length ……… 2+
    • Element types … string (list_name), any (value_to_append), ...

Side Effects
    • The specified list is modified by appending the new value(s).

Errors
    • TCL_ERROR: `expected <type> but got <value>` if a value cannot be
      converted to the list's datatype.
    • TCL_ERROR if the list is not found.

Example
    set myInts [dl_ilist 1 2]
    dl_append $myInts 3 4
    dl_tcllist $myInts   ;# → 1 2 3 4

    # Appending a list to a list of lists (lol) flattens the result.
    set myLoL [dl_llist]
    dl_append $myLoL [dl_ilist 10] [dl_ilist 20]
    dl_tcllist $myLoL   ;# → 10 20

See also
    dl_prepend, dl_insert, dl_concat


=== dl_asin (Arithmetic / Trigonometric) ===
Synopsis
    dl_asin <list> | dl_asin <number>

Brief
    Computes the element-wise arc sine of a list or a single number.
    Input values must be between -1.0 and 1.0. Values outside this range
    will return `NaN` (Not a Number). Returns a new list of floats (radians).

Inputs
    • Type …………… DynList_or_Number
    • Length ……… 1
    • Element types … numeric
    • Constraints … Elements must be in the range [-1.0, 1.0]. Values
                    outside this range will result in `NaN`.

Returns
    • Type …………… DynList (new object)
    • Element type … float
    • Notes ………… Values are in radians. An input value outside the valid
                    domain of asin (e.g., > 1.0) will produce a `NaN`
                    element in the output.

Errors
    • TCL_ERROR if the input list is not found or is not numeric.

Example
    dl_tcllist [dl_asin [dl_flist 0 0.5 -1.1]]   ;# → 0.0 0.5236 NaN

See also
    dl_sin, dl_acos, dl_atan


=== dl_atan (Arithmetic / Trigonometric) ===
Synopsis
    dl_atan <list> | dl_atan <number>

Brief
    Computes the element-wise arc tangent of a list or a single number.
    Returns a new list of floats (radians).

Inputs
    • Type …………… DynList_or_Number
    • Length ……… 1
    • Element types … numeric

Returns
    • Type …………… DynList (new object)
    • Element type … float
    • Notes ………… Values are in radians (typically in the range -PI/2 to PI/2).

Errors
    • TCL_ERROR if the input list is not found or is not numeric.

Example
    dl_tcllist [dl_atan [dl_flist 0 1 -1 Inf]]   ;# → 0.0 0.7854 -0.7854 1.5708

See also
    dl_tan, dl_acos, dl_asin, dl_atan2


=== dl_atan2 (Arithmetic / Trigonometric) ===
Synopsis
    dl_atan2 y_list x_list | dl_atan2 y_val x_val | dl_atan2 y_list x_val | dl_atan2 y_val x_list

Brief
    Computes the element-wise arc tangent of y/x, using the signs of both
    arguments to determine the quadrant of the result. Returns a new list
    of floats (radians).

Inputs
    • Type …………… DynList_or_Number
    • Length ……… 2
    • Element types … numeric, numeric
    • Constraints … Inputs must be numeric and broadcastable.

Returns
    • Type …………… DynList (new object)
    • Element type … float
    • Notes ………… Values are in radians.

Errors
    • TCL_ERROR if an input list is not found or is not numeric.
    • Note: Using integer scalars for both y and x may produce incorrect
      results. Use floats (e.g., 1.0) for scalar calculations.

Example
    # list/list
    dl_tcllist [dl_atan2 [dl_flist 1 1 -1 -1] [dl_flist 1 -1 1 -1]]
    # → 0.7854 2.3562 -0.7854 -2.3562

    # val/val (use floats)
    dl_tcllist [dl_atan2 1.0 -1.0]
    # → 2.3562

    # list/val
    dl_tcllist [dl_atan2 [dl_flist 1 1 -1 -1] 1.0]
    # → 0.7854 0.7854 -0.7854 -0.7854

See also
    dl_tan, dl_acos, dl_asin, dl_atan


=== dl_bcycle (Manipulation / List of Lists) ===
Synopsis
    dl_bcycle dynlist_name [shift]

Brief
    Cycles the elements of the bottom-most sublists of a list of lists
    (or the elements of a simple list) by a specified 'shift' amount.
    Positive shift cycles right, negative cycles left. Default shift is 1.
    Returns a new list.

Inputs
    • Type …………… varied
    • Length ……… 1-2
    • Element types … string (dynlist_name), integer (shift_amount)

Returns
    • Type …………… DynList (new object)
    • Element type … Same as input list (preserves structure and element types)

Errors
    • TCL_ERROR if dynlist_name not found, or if shift (if provided) is not an integer.

Example
    set lol [dl_llist [dl_ilist 1 2 3] [dl_flist 1.1 2.2 3.3 4.4]]
    # lol is {{1 2 3} {1.1 2.2 3.3 4.4}}

    dl_tcllist [dl_bcycle $lol]
    # → {3 1 2} {4.4 1.1 2.2 3.3}

    dl_tcllist [dl_bcycle $lol 2]
    # → {2 3 1} {3.3 4.4 1.1 2.2}

    dl_tcllist [dl_bcycle $lol -1]
    # → {2 3 1} {2.2 3.3 4.4 1.1}

    set sl [dl_ilist 10 20 30 40]
    dl_tcllist [dl_bcycle $sl]      ;# → 40 10 20 30
    dl_tcllist [dl_bcycle $sl -1]   ;# → 20 30 40 10

See also
    dl_bshift, dl_cycle, dl_shift, dl_subshift


=== dl_bins (Utility / Histogramming) ===
Synopsis
    dl_bins start stop nbins

Brief
    Generates a list of bin centers. Given a range (start, stop) and a
    number of bins (nbins), it returns a list of nbins float values
    representing the center of each bin.

Inputs
    • Type …………… varied
    • Length ……… 3
    • Element types … numeric (start), numeric (stop), integer (nbins)

Returns
    • Type …………… DynList (new object)
    • Element type … float
    • Notes ………… Returns a list of `nbins` values.

Errors
    • TCL_ERROR: `dl_bins: unable to create bins list` if nbins is not a
      positive integer.

Example
    # Returns 5 bin centers from 0 to 10
    dl_tcllist [dl_bins 0 10 5]
    # → 1.0 3.0 5.0 7.0 9.0

    # Returns 6 bin centers from -1.5 to 1.5
    dl_tcllist [dl_bins -1.5 1.5 6]
    # → -1.25 -0.75 -0.25 0.25 0.75 1.25

See also
    dl_hist, dl_histLists, dl_cut


=== dl_bmins (Reduction / List of Lists) ===
Synopsis
    dl_bmins dynlist_name

Brief
    Computes the minimum value of each bottom-most sublist in a list.
    Returns a new, simple list containing the minimums. This command
    does not preserve the original list's structure.

Inputs
    • Type …………… DynList
    • Length ……… 1
    • Element types … numeric (potentially nested)

Returns
    • Type …………… DynList (new object)
    • Element type … Same as elements of bottom-most input sublists.

Errors
    • TCL_ERROR if dynlist_name is not found, or if any bottom-most list
      is empty or non-numeric.
    • Note: The related documented commands `dl_bmin`, `dl_bmax`, and
      `dl_bmean` were not found to be implemented.

Example
    set lol [dl_llist [dl_ilist 1 5 2] [dl_ilist 10 0 3]]
    dl_tcllist [dl_bmins $lol]   ;# → 1 0

    set simple_list [dl_ilist -1 7 3]
    dl_tcllist [dl_bmins $simple_list] ;# → -1

See also
    dl_min


=== dl_breverse (Manipulation / List of Lists) ===
Synopsis
    dl_breverse dynlist_name

Brief
    Reverses the elements of each bottom-most sublist in a (potentially
    nested) dynamic list. If the input is a simple list, it reverses
    that list. Returns a new list.

Inputs
    • Type …………… DynList
    • Length ……… 1
    • Element types … any (potentially nested)

Returns
    • Type …………… DynList (new object)
    • Element type … Same as input list (preserves structure and element types)

Errors
    • TCL_ERROR if dynlist_name is not found.

Example
    set lol [dl_llist [dl_ilist 1 2 3] [dl_flist 4.4 5.5]]
    # lol is {{1 2 3} {4.4 5.5}}
    dl_tcllist [dl_breverse $lol]
    # → {3 2 1} {5.5 4.4}

    set nested [dl_llist [dl_llist [dl_ilist 1 2]] [dl_ilist 3 4]]
    # nested is {{{1 2}} {3 4}}
    dl_tcllist [dl_breverse $nested]
    # → {{2 1}} {4 3}

See also
    dl_reverse, dl_reverseAll, dl_permute


=== dl_bshift (Manipulation / List of Lists) ===
Synopsis
    dl_bshift dynlist_name [shift]

Brief
    Shifts elements of the bottom-most sublists (or a simple list) by a
    specified amount. Positive shift moves right, negative shifts left.
    Elements shifted off the ends are lost. Vacant spots are filled with
    0 for numeric lists or an empty string for string lists.

Inputs
    • Type …………… varied
    • Length ……… 1-2
    • Element types … string (dynlist_name), integer (shift_amount)

Returns
    • Type …………… DynList (new object)
    • Element type … Same as input list (preserves structure and element types)

Errors
    • TCL_ERROR if dynlist_name is not found or if shift amount is invalid.

Example
    set lol [dl_llist [dl_ilist 1 2 3] [dl_flist 4.4 5.5]]
    # lol is {{1 2 3} {4.4 5.5}}

    dl_tcllist [dl_bshift $lol 1]
    # → {0 1 2} {0.0 4.4}

    dl_tcllist [dl_bshift $lol -1]
    # → {2 3 0} {5.5 0.0}

    set simple [dl_ilist 1 2 3 4]
    dl_tcllist [dl_bshift $simple 2]   ;# → 0 0 1 2

See also
    dl_bcycle, dl_shift, dl_cycle, dl_subshift


=== dl_bsums (Reduction / List of Lists) ===
Synopsis
    dl_bsums dynlist_name

Brief
    Computes the sum of each bottom-most sublist in a (potentially nested)
    dynamic list. The command replaces each sublist with its sum, which
    can reduce the overall nesting depth of the list.

Inputs
    • Type …………… DynList
    • Length ……… 1
    • Element types … numeric (potentially nested)

Returns
    • Type …………… DynList (new object)
    • Element type … Numeric (promoted to float if mixed types)
    • Notes ………… This command does not preserve the original list's
                    structure perfectly. It replaces each bottom-most list
                    with its sum, effectively removing one layer of nesting
                    at each location.

Errors
    • TCL_ERROR if dynlist_name is not found, or if any bottom-most list
      is empty or non-numeric.

Example
    set lol [dl_llist [dl_ilist 1 2 3] [dl_flist 10 20.5]]
    # lol is {{1 2 3} {10 20.5}}
    dl_tcllist [dl_bsums $lol]
    # → 6 30.5

    set nested [dl_llist [dl_llist [dl_ilist 1 2] [dl_ilist 3 4]] [dl_ilist 5 6]]
    # nested is {{{1 2} {3 4}} {5 6}}
    dl_tcllist [dl_bsums $nested]
    # → {3 7} 11

See also
    dl_sum


=== dl_bstds (Reduction / List of Lists) ===
Synopsis
    dl_bstds dynlist_name

Brief
    Computes the sample standard deviation of each bottom-most sublist in a
    (potentially nested) dynamic list. The command replaces each sublist
    with its standard deviation, which can reduce the nesting depth.

Inputs
    • Type …………… DynList
    • Length ……… 1
    • Element types … numeric (potentially nested)

Returns
    • Type …………… DynList (new object)
    • Element type … float
    • Notes ………… This command replaces each bottom-most list with its
                    sample standard deviation (n-1), effectively removing
                    one layer of nesting at each location.

Errors
    • TCL_ERROR if dynlist_name is not found, or if any bottom-most list
      has fewer than two elements or is non-numeric.

Example
    set lol [dl_llist [dl_ilist 1 2 3] [dl_ilist 10 20 30 40]]
    # lol is {{1 2 3} {10 20 30 40}}
    dl_tcllist [dl_bstds $lol]
    # → 1.0 12.9099

    set nested [dl_llist [dl_llist [dl_ilist 1 3] [dl_ilist 0 10 5]] [dl_ilist 4 5 6]]
    # nested is {{{1 3} {0 10 5}} {4 5 6}}
    dl_tcllist [dl_bstds $nested]
    # → {1.4142 5.0} 1.0

See also
    dl_std, dl_bmean, dl_hvar, dl_var


=== dl_bsort (Manipulation / List of Lists) ===
Synopsis
    dl_bsort dynlist_name

Brief
    Sorts the elements of each bottom-most sublist in a (potentially
    nested) dynamic list. If the input is a simple list, it sorts that
    list. Returns a new list with the same structure.

Inputs
    • Type …………… DynList
    • Length ……… 1
    • Element types … numeric or string (potentially nested)

Returns
    • Type …………… DynList (new object)
    • Element type … Same as input list (preserves structure and element types)

Errors
    • TCL_ERROR if dynlist_name is not found.
    • TCL_ERROR if sublists contain incompatible types for sorting.

Example
    set lol [dl_llist [dl_ilist 3 1 2] [dl_slist c a b]]
    # lol is {{3 1 2} {c a b}}
    dl_tcllist [dl_bsort $lol]
    # → {1 2 3} {a b c}

    set simple [dl_ilist 3 1 4 2]
    dl_tcllist [dl_bsort $simple]   ;# → 1 2 3 4

See also
    dl_sort, dl_bsortIndices, dl_sortIndices, dl_sortByList


=== dl_bsortIndices (Manipulation / List of Lists) ===
Synopsis
    dl_bsortIndices dynlist_name

Brief
    Returns a list of indices that would sort each bottom-most sublist
    of the input list. The command preserves the original list's structure.

Inputs
    • Type …………… DynList
    • Length ……… 1
    • Element types … numeric or string (potentially nested)

Returns
    • Type …………… DynList (new object)
    • Element type … integer
    • Notes ………… The returned list has the same structure as the input list,
                    but each element is replaced by its sort index.

Errors
    • TCL_ERROR if dynlist_name is not found.
    • TCL_ERROR if sublists contain incompatible types for sorting.

Example
    set lol [dl_llist [dl_ilist 30 10 20] [dl_slist c a b]]
    # lol is {{30 10 20} {c a b}}
    dl_tcllist [dl_bsortIndices $lol]
    # → {1 2 0} {1 2 0}

    set simple [dl_ilist 30 10 40 20]
    dl_tcllist [dl_bsortIndices $simple]   ;# → 1 3 0 2

See also
    dl_bsort, dl_sortIndices, dl_sort, dl_permute


=== dl_ceil (Arithmetic) ===
Synopsis
    dl_ceil <list> | dl_ceil <number>

Brief
    Computes the element-wise ceiling of a list or number (the smallest
    integer not less than each value). Returns a new list of integers.

Inputs
    • Type …………… DynList_or_Number
    • Length ……… 1
    • Element types … numeric

Returns
    • Type …………… DynList (new object)
    • Element type … integer

Errors
    • TCL_ERROR: `dl_ceil: invalid list operand` if the list is not numeric.

Example
    dl_tcllist [dl_ceil [dl_flist 1.2 2.0 -3.7 4]]   ;# → 2 2 -3 4
    dl_tcllist [dl_ceil -5.5]                          ;# → -5

See also
    dl_floor, dl_round, dl_int


=== dl_choose (Manipulation / Selection) ===
Synopsis
    dl_choose source_dynlist_name index_dynlist_name

Brief
    Selects elements from a source list based on 0-based indices
    provided in an index list. Returns a new list containing the chosen
    elements. Allows for repeated selection of elements.

Inputs
    • Source List …… The name of a DynList from which to choose elements.
    • Index List ……… The name of an integer DynList of 0-based indices.

Returns
    • Type …………… string (name of a new DynList)
    • Element type … Same as source list.

Errors
    • TCL_ERROR: `dl_choose: unable to select elements from list...` if the
      index list contains out-of-bounds indices.
    • TCL_ERROR if a list name is not found.

Example
    set source [dl_slist apple banana cherry date elderberry]
    set indices [dl_ilist 0 3 1 3 0]
    set chosen [dl_choose $source $indices]
    dl_tcllist $chosen   ;# → apple date banana date apple

See also
    dl_select, dl_get, dl_permute, dl_pickone


=== dl_clean (Memory Management) ===
Synopsis
    dl_clean

Brief
    Deletes all temporary dynamic lists in the current interpreter and resets
    the temporary list counter. Temporary lists are those with names
    starting with a '%' character.

Inputs
    This command takes no arguments.

Side Effects
    • All temporary dynamic lists are deleted.
    • The internal counter for generating temporary list names is reset.

Errors
    • Unlikely to error in normal use.

Example
    # Create a temporary list by performing an operation
    set result [dl_add [dl_ilist 1 2] [dl_ilist 3 4]]
    # $result now holds the name of a temporary list (e.g., %list0)

    # After running dl_clean, the list named by $result is deleted.
    dl_clean
    puts [dl_exists $result]   ;# → 0

See also
    dl_delete, dl_cleanReturns


=== dl_cleanReturns (Memory Management) ===
Synopsis
    dl_cleanReturns

Brief
    Deletes all 'return' dynamic lists in the current interpreter. Return
    lists are those created by `dl_return` and have names starting with
    a '>' character (e.g., >0<).

Inputs
    This command takes no arguments.

Side Effects
    • All 'return' dynamic lists are deleted.
    • The internal counter for generating return list names is reset.

Errors
    • Unlikely to error in normal use.

Example
    # This command is typically used within Tcl procs.
    proc my_proc {val} {
      set temp_list [dl_ilist $val]
      return [dl_return $temp_list]
    }
    set ret_list [my_proc 5]
    # $ret_list now holds the name of a return list (e.g., >0<)

    # After running dl_cleanReturns, the list named by $ret_list is deleted.
    dl_cleanReturns

See also
    dl_delete, dl_clean, dl_return


=== dl_clist (Creation) ===
Synopsis
    dl_clist [value1 value2 ...]

Brief
    Creates a new dynamic list of type character (byte) from a list of
    numeric character codes.

Inputs
    • Type …………… numeric
    • Length ……… 0+
    • Element types … Individual integer values (character codes). Floats
                    are truncated.

Returns
    • Type …………… string (name of a new character DynList)
    • Element type … char (byte)
    • Notes ………… Viewing the list with `dl_tcllist` will display the
                    numeric codes, not the string characters.

Errors
    • TCL_ERROR if non-numeric values are provided.
    • Note: Does not accept Tcl lists or other DynList names as arguments,
      contrary to some documentation.

Example
    # Creates a list containing the byte values for 'A', 'B', 'C'
    set charList [dl_clist 65 66 67]
    dl_tcllist $charList   ;# → 65 66 67

    # Float values are truncated
    set charList2 [dl_clist 97.2 98.9 99.5]
    dl_tcllist $charList2   ;# → 97 98 99

See also
    dl_create, dl_ilist, dl_flist, dl_slist, dl_llist


=== dl_collapse (Manipulation / Restructuring) ===
Synopsis
    dl_collapse list_of_lists_name

Brief
    Collapses a list of lists by one level, concatenating all sublists
    into a new single list. All sublists must be of the same data type.

Inputs
    • Type …………… string (dynlist_name)
    • Length ……… 1
    • Element types … A list of lists, where all sublists have the same type.

Returns
    • Type …………… string (name of new DynList)
    • Element type … Same as the sublists' element type.

Errors
    • TCL_ERROR: `dl_collapse: bad operand` if the input is not a list of
      lists, or if its sublists are not all of the same type.

Example
    set lol [dl_llist [dl_ilist 1 2] [dl_ilist 3 4 5] [dl_ilist 6]]
    set flat [dl_collapse $lol]
    dl_tcllist $flat   ;# → 1 2 3 4 5 6

    # This will fail, as sublists have different types:
    # set mix [dl_llist [dl_ilist 10 20] [dl_flist 30.3 40.4]]
    # dl_collapse $mix

See also
    dl_unpack, dl_deepUnpack, dl_concat, dl_pack


=== dl_combine (Creation / Concatenation) ===
Synopsis
    dl_combine dynlist1_name dynlist2_name [dynlist3_name ...]

Brief
    Creates a new list by concatenating two or more existing lists.
    All input lists must be of the same data type.

Inputs
    • Type …………… strings (dynlist_names)
    • Length ……… 2+
    • Element types … All lists must have the same data type.

Returns
    • Type …………… string (name of new DynList)
    • Element type … Same as the input lists' element type.

Errors
    • TCL_ERROR: `dl_combine: lists must be of the same type` if the input
      lists are not of the same type.

Example
    set l1 [dl_ilist 1 2]
    set l2 [dl_ilist 3 4]
    set combined [dl_combine $l1 $l2]
    dl_tcllist $combined   ;# → 1 2 3 4

    # This will fail, as lists have different types:
    # set l3 [dl_flist 5.5 6.6]
    # dl_combine $l1 $l3

See also
    dl_concat, dl_append


=== dl_concat (Manipulation / Concatenation) ===
Synopsis
    dl_concat dest_dynlist_name source_dynlist_name [source_dynlist_name2 ...]

Brief
    Concatenates one or more source lists onto a destination list. The
    destination list is modified in-place. All lists must be of the same
    data type.

Inputs
    • Destination List … The name of the DynList to be modified.
    • Source List(s) … One or more DynLists to append.
    • Constraints …… All lists must have the same data type.

Returns
    • string (the name of the modified destination DynList)

Side Effects
    • The destination list is modified.

Errors
    • TCL_ERROR: `dl_concat: lists must be of the same type` if lists have
      incompatible types.

Example
    set list1 [dl_ilist 1 2]
    set list2 [dl_ilist 3 4]
    set list3 [dl_ilist 5 6]
    dl_concat $list1 $list2 $list3
    dl_tcllist $list1   ;# → 1 2 3 4 5 6

See also
    dl_combine, dl_append


=== dl_conv (Arithmetic / Signal Processing) ===
Synopsis
    dl_conv data_dynlist kernel_dynlist

Brief
    [WARNING: This command is broken and does not perform a standard convolution.]
    It takes two lists as input and returns a list of the same length as the
    first list. The calculation appears to be a partial convolution followed
    by junk values.

Inputs
    • Data List ……… A 1D numeric DynList.
    • Kernel List …… A 1D numeric DynList.

Returns
    • A new DynList of floats, the same size as the input data list.

Side Effects
    • None.

Errors
    • The command's output does not match standard convolution behavior.
    • Additional list arguments are ignored.

Example
    set data [dl_ilist 1 2 3 4 5]
    set kernel [dl_ilist 1 0 -1]
    # The command incorrectly returns a list of the same length as the data.
    # The initial values are the result of a 'valid' convolution, but the
    # rest of the list contains incorrect values.
    dl_tcllist [dl_conv $data $kernel]
    # → -2.0 -2.0 -2.0 -2.0 4.0

    # A correct 'valid' convolution should be:
    # → -2.0 -2.0 -2.0

See also
    dl_conv2 (also broken), dl_diff


=== dl_conv2 (Arithmetic / Signal Processing) ===
Synopsis
    dl_conv2 data_dynlist b_coeffs a_coeffs

Brief
    [WARNING: This command is broken.]
    It is intended to perform IIR filtering but instead returns the first
    input list unmodified.

Inputs
    • Data List …… A 1D numeric DynList.
    • B Coeffs ……… Ignored.
    • A Coeffs ……… Ignored.

Returns
    • A new DynList containing an exact copy of the input data_dynlist.

Example
    set signal [dl_ilist 1 0 0 0 0]
    set b [dl_flist 1.0]
    set a [dl_flist 1.0 -0.8]
    # The command incorrectly returns the signal list unmodified.
    dl_tcllist [dl_conv2 $signal $b $a]
    # → 1.0 0.0 0.0 0.0 0.0

See also
    dl_conv (also broken), dl_diff


=== dl_cos (Arithmetic / Trigonometric) ===
Synopsis
    dl_cos <list> | dl_cos <number_in_radians>

Brief
    Computes the element-wise cosine of a list or a single number.
    Input values are assumed to be in radians.

Inputs
    • Type …………… DynList_or_Number
    • Length ……… 1
    • Element types … numeric

Returns
    • Type …………… DynList (new object)
    • Element type … float

Errors
    • TCL_ERROR if the input list is not numeric.

Example
    # Angles for 0, PI/2, PI, 2*PI
    set angles [dl_flist 0 1.5708 3.14159 6.28318]
    dl_tcllist [dl_cos $angles]
    # → 1.0 -3.6e-6 -1.0 1.0 (approximately)

    # cos(PI/4)
    dl_tcllist [dl_cos 0.7854]   ;# → 0.7071

See also
    dl_sin, dl_tan, dl_acos, dl_cosh


=== dl_cosh (Arithmetic / Hyperbolic) ===
Synopsis
    dl_cosh <list> | dl_cosh <number>

Brief
    Computes the element-wise hyperbolic cosine of a DynList or a single
    number. Returns a new list of floats.

Inputs
    • Type …………… DynList_or_Number
    • Length ……… 1
    • Element types … numeric

Returns
    • Type …………… DynList (new object)
    • Element type … float
    • Note …………… Resulting values are always >= 1.0.

Errors
    • TCL_ERROR if the input list is not numeric.
    • TCL_ERROR for math errors (e.g., overflow for large inputs).

Example
    set values [dl_flist 0 1 -1 2.5]
    dl_tcllist [dl_cosh $values]
    # → 1.0 1.54308 1.54308 6.13229

    dl_tcllist [dl_cosh 0]   ;# → 1.0

See also
    dl_sinh, dl_tanh, dl_cos


=== dl_countOccurences (Searching / Counting) ===
Synopsis
    dl_countOccurences <source_list> <search_list>

Brief
    [WARNING: This command is broken.]
    Counts the number of non-overlapping occurrences of a search list within
    a source list. However, the command fails to return any value.

Inputs
    • Source List … A DynList to search within.
    • Search List … A DynList to search for.

Returns
    • Nothing. The command appears to be non-functional.

Errors
    • The command does not return a value.
    • Expects exactly two arguments, both must be DynLists.

Example
    # The following command should return 3, but returns nothing.
    set mainList [dl_ilist 1 2 3 1 2 4 1 2 3 5]
    set subList [dl_ilist 1 2]
    dl_countOccurences $mainList $subList

    # This example is incorrect, as the second argument must be a list.
    # dl_countOccurences $mainList 3

See also
    dl_find, dl_findAll


=== dl_create (Creation) ===
Synopsis
    dl_create [value1 value2 ...]

Brief
    [WARNING: This command is buggy and its use is discouraged.]
    Creates a new DynList, attempting to infer the type from the arguments.
    However, it often fails with a "bad datatype" error. Use the
    type-specific creation commands like `dl_ilist`, `dl_flist`, and
    `dl_slist` instead for reliable list creation.

Inputs
    • Values … An optional sequence of literal values (e.g., numbers or strings).

Returns
    • The name of a new DynList.

Errors
    • TCL_ERROR `dl_create: bad datatype` is frequently returned for many
      valid-seeming inputs, including creating from Tcl lists or combining
      lists, which is contrary to its documentation.

Example
    # The command MAY work for simple, unambiguous cases:
    set ints [dl_create 1 2 3]
    dl_tcllist $ints   ;# → 1 2 3

    # However, many documented examples fail:
    # set list_from_tcl [dl_create {a b c}]  ;# FAILS
    # set combined [dl_create $ints $floats] ;# FAILS

See also
    dl_ilist, dl_flist, dl_slist, dl_llist (Recommended alternatives)


=== dl_cumprod (Arithmetic / Accumulation) ===
Synopsis
    dl_cumprod <list>

Brief
    [NOTE: The output of this command could not be verified.]
    Computes the cumulative product of elements in a numeric list.

Inputs
    • Type …………… DynList
    • Element types … numeric

Returns
    • A new DynList containing the cumulative products. The element type may be
      promoted to float.

Errors
    • TCL_ERROR if the list is not numeric.

Example
    # The following is the documented, but unverified, behavior:
    set values [dl_flist 1 2 3 4 0.5]
    dl_tcllist [dl_cumprod $values]
    # → 1.0 2.0 6.0 24.0 12.0

See also
    dl_cumsum, dl_prod, dl_sum


=== dl_cumsum (Arithmetic / Accumulation) ===
Synopsis
    dl_cumsum <list>

Brief
    [NOTE: The output of this command could not be verified.]
    Computes the cumulative sum of elements in a numeric list.

Inputs
    • Type …………… DynList
    • Element types … numeric

Returns
    • A new DynList containing the cumulative sums. The element type may be
      promoted to float.

Errors
    • TCL_ERROR if the list is not numeric.

Example
    # The following is the documented, but unverified, behavior:
    set values [dl_flist 1 2 3 4 -5.5]
    dl_tcllist [dl_cumsum $values]
    # → 1.0 3.0 6.0 10.0 4.5

See also
    dl_cumprod, dl_sum, dl_prod


=== dl_cycle (Manipulation / Permutation) ===
Synopsis
    dl_cycle <list> [shift_amount]

Brief
    [NOTE: The output of this command could not be verified.]
    Cycles the elements of a list, wrapping elements around. A positive
    shift moves elements to the right; a negative shift moves them to the left.
    The default shift is 1.

Inputs
    • List …………… A 1D DynList.
    • Shift Amount … An optional integer (default: 1).

Returns
    • A new DynList containing the cycled elements.

Example
    # The following is the documented, but unverified, behavior:
    set letters [dl_slist a b c d e]

    # Default shift of 1
    dl_tcllist [dl_cycle $letters]
    # → e a b c d

    # Shift of 2
    dl_tcllist [dl_cycle $letters 2]
    # → d e a b c

    # Shift of -1
    dl_tcllist [dl_cycle $letters -1]
    # → b c d e a

See also
    dl_shift, dl_permute, dl_reverse


=== dl_datatype (Introspection) ===
Synopsis
    dl_datatype <list>

Brief
    [NOTE: The output of this command could not be verified.]
    Returns a string representing the underlying data type of the list.

Inputs
    • List … The name of an existing DynList.

Returns
    • According to the documentation, a string identifier for the data type
      (e.g., "DF_LONG", "DF_FLOAT", "DF_STRING"). This could not be confirmed.

Example
    # The following is the documented, but unverified, behavior:
    set ints [dl_ilist 1 2 3]
    puts [dl_datatype $ints]
    # → "DF_LONG"

    set floats [dl_flist 1.0 2.5]
    puts [dl_datatype $floats]
    # → "DF_FLOAT"

See also
    dl_exists, dl_length, dl_depth


=== dl_deep_pack (Serialization) ===
Synopsis
    dl_deep_pack <list_of_lists>

Brief
    [WARNING: This command does not exist.]
    The documentation indicates this command should serialize a nested list,
    but it is not a valid command in the `essctrl` environment.

See also
    dl_deep_unpack (does not exist)


=== dl_deep_unpack (Serialization) ===
Synopsis
    dl_deep_unpack <packed_list>

Brief
    [WARNING: This command does not exist.]
    The documentation indicates this command should deserialize a list
    created by `dl_deep_pack`, but it is not a valid command.

See also
    dl_deep_pack (does not exist)


=== dl_delete (Memory Management) ===
Synopsis
    dl_delete <list_name> [list_name2 ...]

Brief
    [NOTE: The behavior of this command could not be verified.]
    Deletes one or more DynLists from memory.

Inputs
    • List Name(s) … One or more strings containing the names of the DynLists
      to be deleted.

Side Effects
    • The specified DynLists should be deallocated and their names removed.
      This could not be confirmed.

Example
    # The following is the documented, but unverified, behavior:
    set mylist [dl_ilist 1 2 3]
    # At this point, [dl_exists $mylist] should return 1

    dl_delete $mylist

    # At this point, [dl_exists $mylist] should return 0

See also
    dl_clean