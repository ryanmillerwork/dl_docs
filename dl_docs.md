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
    Counts non-overlapping occurrences of a <search_list> within a
    <source_list>.
    [WARNING: This command is buggy when the search list has more than
    one element.]

Inputs
    • Source List … A DynList to search within.
    • Search List … A DynList to search for. Both arguments must be lists.

Returns
    • A new `long` DynList containing the integer count.
    • If <search_list> has more than one element, the result is incorrect
      and contains duplicated values (e.g., `3 3`).

Example
    # Searching for a single element (works correctly):
    set mainList [dl_ilist 1 2 3 1 2 4 1 2 3 5]
    dl_tcllist [dl_countOccurences $mainList [dl_ilist 3]]
    # → 2

    set strList [dl_slist apple banana apple orange apple]
    dl_tcllist [dl_countOccurences $strList [dl_slist apple]]
    # → 3

    # Searching for a multi-element sub-list (BUGGY):
    set subList [dl_ilist 1 2]
    dl_tcllist [dl_countOccurences $mainList $subList]
    # → 3 3  (Correct answer is 3, but it is duplicated)

See also
    dl_find, dl_findAll


=== dl_create (Creation) ===
Synopsis
    dl_create [value1 value2 ...]

Brief
    [WARNING: This command is non-functional and should not be used.]
    It is intended to create a new DynList by inferring type from arguments,
    but it consistently fails with a `bad datatype` error for all tested
    inputs.

    Use the type-specific creation commands for reliable list creation:
    • `dl_ilist` for integers
    • `dl_flist` for floats
    • `dl_slist` for strings
    • `dl_llist` for lists of lists

Returns
    • This command fails before a list can be returned.

Example
    # The following documented examples all fail:
    # dl_create 1 2 3
    # dl_create 1.0 2.5
    # dl_create hello world

See also
    dl_ilist, dl_flist, dl_slist, dl_llist (Recommended alternatives)


=== dl_cumprod (Arithmetic / Accumulation) ===
Synopsis
    dl_cumprod <list>

Brief
    Computes the cumulative product of elements in a numeric list.

Inputs
    • Type …………… DynList
    • Element types … numeric

Returns
    • A new DynList containing the cumulative products. The element type may be
      promoted to float if the input is float.

Errors
    • TCL_ERROR if the list is not numeric.

Example
    set values [dl_flist 1 2 3 4 0.5]
    dl_tcllist [dl_cumprod $values]
    # → 1.0 2.0 6.0 24.0 12.0

    set ints [dl_ilist 2 3 -1 5]
    dl_tcllist [dl_cumprod $ints]
    # → 2 6 -6 -30

See also
    dl_cumsum, dl_prod, dl_sum


=== dl_cumsum (Arithmetic / Accumulation) ===
Synopsis
    dl_cumsum <list>

Brief
    Computes the cumulative sum of elements in a numeric list.

Inputs
    • Type …………… DynList
    • Element types … numeric

Returns
    • A new DynList containing the cumulative sums. The element type may be
      promoted to float if the input is float.

Errors
    • TCL_ERROR if the list is not numeric.

Example
    set values [dl_flist 1 2 3 4 -5.5]
    dl_tcllist [dl_cumsum $values]
    # → 1.0 3.0 6.0 10.0 4.5

    set ints [dl_ilist 10 20 -5 15]
    dl_tcllist [dl_cumsum $ints]
    # → 10 30 25 40

See also
    dl_cumprod, dl_sum, dl_prod


=== dl_cycle (Manipulation / Permutation) ===
Synopsis
    dl_cycle <list> [shift_amount]

Brief
    Cycles the elements of a list, wrapping elements around. A positive
    shift moves elements to the right; a negative shift moves them to the left.
    The default shift is 1.

Inputs
    • List …………… A 1D DynList.
    • Shift Amount … An optional integer (default: 1).

Returns
    • A new DynList containing the cycled elements.

Example
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
    Returns a string representing the underlying data type of the list.

Inputs
    • List … The name of an existing DynList.

Returns
    • A string identifier for the data type. Common values are:
      `long`, `float`, `string`, `list`.

Example
    set ints [dl_ilist 1 2 3]
    dl_datatype $ints
    # → long

    set floats [dl_flist 1.0 2.5]
    dl_datatype $floats
    # → float

    set strings [dl_slist "hello" "world"]
    dl_datatype $strings
    # → string

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
    Deletes one or more DynLists from memory.

Inputs
    • List Name(s) … One or more strings containing the names of the DynLists
      to be deleted.

Side Effects
    • The specified DynLists are deallocated and their names are removed.

Example
    set mylist [dl_ilist 1 2 3]
    # dl_exists $mylist → 1
    dl_delete $mylist
    # dl_exists $mylist → 0

See also
    dl_clean, dl_exists


=== dl_exists (Introspection) ===
Synopsis
    dl_exists <list_name>

Brief
    Checks if a DynList with the given name exists.

Inputs
    • List Name … A string containing the name of the DynList to check.

Returns
    • `1` if the list exists, `0` otherwise.

Example
    set mylist [dl_ilist 1]
    # dl_exists $mylist → 1
    dl_delete $mylist
    # dl_exists $mylist → 0

See also
    dl_delete, dl_clean


=== dl_deleteTrace (Introspection / Tracing) ===
Synopsis
    dl_deleteTrace <list_name>

Brief
    [WARNING: This command could not be tested.]
    Its purpose appears to be removing a trace from a DynList. However,
    without a corresponding command to create a trace, it is not possible
    to verify its functionality.

Inputs
    • List Name … A string containing the name of the DynList.

See also
    dl_traceVariable (hypothetical)


=== dl_depth (Introspection) ===
Synopsis
    dl_depth <list>

Brief
    Returns the 0-indexed maximum nesting depth of a list.

Inputs
    • List … The name of an existing DynList.

Returns
    • An integer representing the nesting depth. A simple list (e.g., from
      `dl_ilist`) has a depth of `0`. A list of lists has a depth of `1`.

Example
    set l1 [dl_ilist 1 2 3]
    dl_depth $l1
    # → 0

    set l2 [dl_llist $l1]
    dl_depth $l2
    # → 1

See also
    dl_datatype, dl_length, dl_llength


=== dl_diff (Arithmetic / Data Analysis) ===
Synopsis
    dl_diff <list> [lag]

Brief
    Computes the element-wise difference between lagged elements of a
    numeric list (list[i+lag] - list[i]). Returns a new list.

Inputs
    • List … A 1D numeric DynList.
    • Lag …… An optional positive integer (default: 1).

Returns
    • A new DynList containing the differences. The new list will be shorter
      than the input list by the `lag` amount.

Errors
    • TCL_ERROR if the list is not numeric or 1D.
    • TCL_ERROR if lag is not a positive integer.
    • TCL_ERROR if the list length is not greater than the lag.

Example
    set values [dl_flist 1 3 6 10 12.5]

    # Default lag of 1
    dl_tcllist [dl_diff $values]
    # → 2.0 3.0 4.0 2.5

    # Lag of 2
    dl_tcllist [dl_diff $values 2]
    # → 5.0 7.0 6.5

See also
    dl_idiff, dl_sub, dl_cumsum


=== dl_dir (Introspection / Registry) ===
Synopsis
    dl_dir [pattern]

Brief
    Returns a Tcl list of the internal names of all currently defined
    dynamic lists. An optional glob pattern can be used to filter the names.

Inputs
    • Pattern … An optional glob pattern to filter the internal list names.

Returns
    • A Tcl list of strings. Each string is the internal name of a dynamic
      list (e.g., `%list0`, `>0<).
    • Note: The pattern matches against the internal names, not the Tcl
      variable names that may hold them.

Side Effects
    • The output of `dl_dir` with no arguments can be very long if many
      lists are in memory. Use `dl_clean` to clear temporary lists.

Example
    # After a clean start
    dl_clean

    set my_integers [dl_ilist 1 2]
    set my_floats [dl_flist 3.0 4.0]

    # dl_dir will show the internal names, not "my_integers", etc.
    # The exact names (%list0, %list1) may vary.
    dl_dir
    # → {%list0% long 2 15} {%list1% float 2 15}

    # The pattern matches the internal name, not the variable name.
    dl_dir %*
    # → {%list0% long 2 15} {%list1% float 2 15}

See also
    dl_exists, dl_clean


=== dl_div (Arithmetic) ===
Synopsis
    dl_div <list_or_val1> <list_or_val2> [<list_or_val3>...]

Brief
    Performs element-wise division. The first argument is the dividend,
    which is divided by each subsequent argument in sequence.

Inputs
    • Operands … Two or more numeric DynLists or scalar values.
    • Constraints … Inputs must be numeric and broadcastable (i.e., have the
                    same length or be a scalar). Divisors cannot be zero.

Returns
    • A new DynList containing the results. The element type will be float
      unless all inputs are integers, in which case it will be integer
      (with truncation).

Errors
    • TCL_ERROR if lists have incompatible lengths.
    • TCL_ERROR for division by zero.

Example
    set l1 [dl_flist 10 20 30.0]
    set l2 [dl_flist 2 5 2.0]

    # List / List
    dl_tcllist [dl_div $l1 $l2]
    # → 5.0 4.0 15.0

    # List / Scalar
    dl_tcllist [dl_div $l1 2]
    # → 5.0 10.0 15.0

    # Chained integer division (note the truncation)
    set i1 [dl_ilist 10 20 30]
    set i2 [dl_ilist 2 5 2]
    dl_tcllist [dl_div 100 $i1 $i2]
    # → 5 1 1

See also
    dl_add, dl_sub, dl_mult, dl_recip


=== dl_dotimes (Control Structure) ===
Synopsis
    dl_dotimes <varName> <count> <scriptBody>

Brief
    A loop that executes a Tcl script a specified number of times. The
    iterator variable, `varName`, holds the current loop index (0 to N-1).

Inputs
    • varName …… A string to use as the iterator variable name.
    • count ……… An integer specifying the number of iterations. To loop over
                 a list, use `[dl_length $listName]` for this argument.
    • scriptBody … A string containing the Tcl script to execute on each iteration.

Returns
    • The result of the last command evaluated in the final iteration of the
      script body. Returns an empty string if the loop does not run.

Side Effects
    • The script body is executed `count` times.
    • The variable specified by `varName` is created and modified within the
      scope of the script body.

Errors
    • TCL_ERROR if `count` is not a valid non-negative integer.

Example
    # Create an empty list, then append to it 3 times
    set newList [dl_slist]
    dl_dotimes i 3 {
        dl_append $newList "iteration-$i"
    }
    dl_tcllist $newList
    # → iteration-0 iteration-1 iteration-2

    # Iterate based on another list's length
    set source [dl_slist a b c]
    set target [dl_slist]
    dl_dotimes idx [dl_length $source] {
        dl_append $target [dl_get $source $idx]
    }
    dl_tcllist $target
    # → a b c

See also
    dl_foreach, dl_length

=== dl_dump (Introspection / Debugging) ===
Synopsis
    dl_dump <list_name>

Brief
    Prints the contents of a dynamic list to standard output. This is a
    primary tool for debugging list contents, as Tcl's `puts` command does
    not work with the `essctrl` interface.

Inputs
    • list_name … The name of an existing DynList.

Side Effects
    • Prints the string representation of each list element to standard
      output, with each element on a new line.

Returns
    • This command does not return a value.

Errors
    • TCL_ERROR if the list is not found.
    • Note: The documented ability to write to a file channel was found to
      be non-functional. The command only prints to stdout.

Example
    # Dump a list of floats
    set myFloats [dl_flist 1.0 2.5 -3.0]
    dl_dump $myFloats
    # Output:
    # 1.000000
    # 2.500000
    # -3.000000

    # Dump a list of strings
    set myStrings [dl_slist hello "world with spaces"]
    dl_dump $myStrings
    # Output:
    # hello
    # world with spaces

See also
    dl_tcllist, dl_write

=== dl_dumpAsRow (Introspection / Debugging) ===
Synopsis
    dl_dumpAsRow <list_name>

Brief
    Prints the elements of a 1D list as a single, tab-delimited row to
    standard output.

Inputs
    • list_name … The name of an existing 1D DynList.

Side Effects
    • Prints the list elements as a single line to stdout.
    • Elements are separated by tab characters.

Returns
    • This command does not return a value.

Errors
    • TCL_ERROR if the list is not found or is not a 1D list.
    • Note: The documented ability to write to a file channel or specify a
      custom delimiter was found to be non-functional.

Example
    set rowData [dl_ilist 10 20 30 40]
    dl_dumpAsRow $rowData
    # Output (elements are separated by tabs):
    # 10      20      30      40

See also
    dl_dump, dl_dumpMatrix

=== dl_dumpMatrix (Introspection / Debugging) ===
Synopsis
    dl_dumpMatrix <matrix_list_name>

Brief
    Prints the contents of a 2D list (a list of lists) to standard output
    in a matrix format.

Inputs
    • matrix_list_name … The name of an existing 2D DynList. The sublists
                         are treated as rows.

Side Effects
    • Prints the matrix to stdout.
    • Rows are separated by newlines.
    • Elements within a row are separated by tab characters.

Returns
    • This command does not return a value.

Errors
    • TCL_ERROR if the list is not found or is not a 2D list.
    • Note: The documented optional arguments for specifying channels,
      delimiters, or prefixes/suffixes were found to be non-functional.

Example
    set matrix [dl_llist [dl_ilist 1 2 3] [dl_ilist 10 20 30]]
    dl_dumpMatrix $matrix
    # Output (columns are separated by tabs):
    # 1       2       3
    # 10      20      30

See also
    dl_dump, dl_dumpAsRow

=== dl_dumpMatrixInCols (Introspection / Debugging) ===
Synopsis
    dl_dumpMatrixInCols <matrix_list_name>

Brief
    Prints a *transposed* version of a 2D list (matrix) to standard output
    with elements aligned in columns.

Inputs
    • matrix_list_name … The name of an existing 2D DynList. The sublists
                         (rows) should have the same length for the alignment
                         to work correctly.

Side Effects
    • Prints a transposed representation of the matrix to stdout.
    • The columns of the original matrix are printed as rows.
    • Elements in the output rows are separated by tabs.

Returns
    • This command does not return a value.

Errors
    • TCL_ERROR if the list is not found or is not a 2D list.
    • Note: The documented optional arguments for specifying channels or
      delimiters were found to be non-functional.

Example
    set m [dl_llist [dl_slist Name Age City] [dl_slist Alice 30 London]]
    dl_dumpMatrixInCols $m
    # Output (note the transposition):
    # Name    Alice
    # Age     30
    # City    London

See also
    dl_dump, dl_dumpMatrix, dl_transpose

=== dl_eq (Logical / Comparison) ===
Synopsis
    dl_eq <list_or_val1> <list_or_val2>

Brief
    Performs an element-wise equality (==) comparison between two lists or
    between a list and a scalar value. Returns a new list of 0s (false)
    and 1s (true).

Inputs
    • list_or_val1 … A DynList or a scalar value.
    • list_or_val2 … A DynList or a scalar value.
    • Constraints …… Inputs must be broadcastable (have the same length or
                     one is a scalar).

Returns
    • Type …………… DynList (new object)
    • Element type … integer
    • Notes ………… Result contains 0 (false) or 1 (true) for each
                    element-wise comparison.

Errors
    • TCL_ERROR if lists have incompatible lengths.
    • TCL_ERROR if an input list name is not found.

Example
    # List vs List
    set l1 [dl_ilist 1 2 3 2 5]
    set l2 [dl_ilist 1 2 4 2 0]
    dl_tcllist [dl_eq $l1 $l2]   ;# → 1 1 0 1 0

    # List vs Scalar
    dl_tcllist [dl_eq $l1 2]      ;# → 0 1 0 1 0

    # String List vs String List
    set s1 [dl_slist a b c]
    set s2 [dl_slist a d c]
    dl_tcllist [dl_eq $s1 $s2]   ;# → 1 0 1

See also
    dl_noteq, dl_gt, dl_lt, dl_and

=== dl_eqIndex (Logical / Indexing) ===
Synopsis
    dl_eqIndex <list_or_val1> <list_or_val2>

Brief
    Performs an element-wise equality (==) comparison and returns a new
    list containing the 0-based indices where the result is true.

Inputs
    • list_or_val1 … A DynList or a scalar value.
    • list_or_val2 … A DynList or a scalar value.
    • Constraints …… Inputs must be broadcastable (have the same length or
                     one is a scalar).

Returns
    • Type …………… DynList (new object)
    • Element type … integer
    • Notes ………… A list of 0-based indices where the comparison is true.

Errors
    • TCL_ERROR if lists have incompatible lengths.
    • TCL_ERROR if an input list name is not found.

Example
    # List vs List
    set l1 [dl_ilist 1 2 3 2 5]
    set l2 [dl_ilist 1 2 4 2 0]
    dl_tcllist [dl_eqIndex $l1 $l2]   ;# → 0 1 3

    # List vs Scalar
    dl_tcllist [dl_eqIndex $l1 2]      ;# → 1 3

See also
    dl_eq, dl_noteqIndex, dl_gtIndex, dl_ltIndex

=== dl_exists (Introspection / Registry) ===
Synopsis
    dl_exists <list_name>

Brief
    Checks if a dynamic list with the specified name exists.

Inputs
    • list_name … A string containing the name of the DynList to check.

Returns
    • `1` if the list exists, `0` otherwise.

Errors
    • TCL_ERROR if the wrong number of arguments is provided.

Example
    # Check for a list that exists
    set myList [dl_ilist 1 2 3]
    dl_exists $myList
    # → 1

    # Check for a list that has been deleted
    set anotherList [dl_ilist 4 5]
    dl_delete $anotherList
    dl_exists $anotherList
    # → 0

    # Check for a name that was never a list
    dl_exists some_nonexistent_list
    # → 0

See also
    dl_dir, dl_datatype, dl_delete

=== dl_exp (Arithmetic / Transcendental) ===
Synopsis
    dl_exp <list> | dl_exp <number>

Brief
    Computes the element-wise natural exponential (e^x) of a numeric list
    or a single number. Returns a new list of floats.

Inputs
    • Type …………… DynList_or_Number
    • Length ……… 1
    • Element types … numeric

Returns
    • Type …………… DynList (new object)
    • Element type … float

Errors
    • TCL_ERROR if the input list is not found or is not numeric.
    • TCL_ERROR for math errors such as overflow for large input values.

Example
    # Get e^x for each element in a list
    set values [dl_flist 0 1 2 -1]
    dl_tcllist [dl_exp $values]
    # → 1.0 2.7183 7.3891 0.3679

    # Get e^x for a single number
    dl_tcllist [dl_exp 3]
    # → 20.0855

See also
    dl_log, dl_log10, dl_pow, dl_sqrt

=== dl_fill (Creation) ===
Synopsis
    dl_fill <value> <count> <start> <stop>

Brief
    Creates a new list containing a specified number of identical elements.

Inputs
    • value … The numeric value to fill the list with.
    • count … The number of elements in the new list.
    • start … The start of a nominal range.
    • stop …… The end of a nominal range.
    • Constraints … The command only succeeds if `count == stop - start + 1`.
                    If this condition is not met, the command may fail silently.
                    The `start` and `stop` values do not otherwise affect the
                    output.

Returns
    • Type …………… string (name of new DynList)
    • Element type … Matches the type of `value`.

Errors
    • The command may fail silently without a TCL_ERROR if the `count` and
      `start`/`stop` range constraint is not met.

Example
    # Creates a list of five 8s.
    # The constraint 5 == 4 - 0 + 1 is met.
    dl_tcllist [dl_fill 8 5 0 4]
    # → 8 8 8 8 8

    # Creates a list of three 7.0s.
    # The constraint 3 == 3.0 - 1.0 + 1 is met.
    dl_tcllist [dl_fill 7.0 3 1.0 3.0]
    # → 7.0 7.0 7.0

    # This will fail silently because 4 != 100 - 50 + 1
    # dl_fill 9.9 4 50 100

See also
    dl_ilist, dl_flist, dl_slist, dl_repeat

=== dl_find (Searching) ===
Synopsis
    dl_find <source_list> <search_item>

Brief
    Finds the first occurrence of a search item (which can be a sublist or
    a single value) within a source list. Returns the 0-based index of the
    start of the match, or -1 if not found.

Inputs
    • source_list … The name of the DynList to search within.
    • search_item … A literal value or the name of a DynList to search for.

Returns
    • An integer representing the 0-based index of the first match.
    • Returns `-1` if no match is found.

Errors
    • TCL_ERROR if a list name is not found.
    • TCL_ERROR for incorrect number of arguments.
    • Note: The documented optional `start_index` was found to be non-functional.

Example
    # Find a sublist within a list
    set mainList [dl_slist a b c a b d e]
    set subList [dl_slist a b]
    dl_find $mainList $subList
    # → 0

    # Find a single value within a list
    dl_find $mainList c
    # → 2

    # Search for an item that does not exist
    dl_find $mainList x
    # → -1

See also
    dl_findAll, dl_findIndices, dl_countOccurences

=== dl_findAll (Searching) ===
Synopsis
    dl_findAll <source_list> <search_item>

Brief
    Finds all non-overlapping occurrences of a search item (a sublist or a
    single value) within a source list. Returns a new list of the 0-based
    starting indices of all matches.

Inputs
    • source_list … The name of the DynList to search within.
    • search_item … A literal value or the name of a DynList to search for.

Returns
    • A new integer DynList of all 0-based starting indices.
    • Returns an empty list if no matches are found.

Errors
    • TCL_ERROR if a list name is not found.
    • TCL_ERROR for incorrect number of arguments.
    • Note: The documented optional `start_index` was found to be non-functional.

Example
    # Find all occurrences of a sublist
    set mainList [dl_slist a b c a b d a b]
    set subList [dl_slist a b]
    dl_tcllist [dl_findAll $mainList $subList]
    # → 0 3 6

    # Find all occurrences of a single value
    set letters [dl_slist a b c a d a]
    dl_tcllist [dl_findAll $letters a]
    # → 0 3 5

    # Search for an item that does not exist
    dl_tcllist [dl_findAll $letters x]
    # → (empty list)

See also
    dl_find, dl_findIndices, dl_countOccurences

=== dl_findIndices (Searching / Indexing) ===
Synopsis
    dl_findIndices <source_list_name> <search_list_name>

Brief
    [WARNING: This command is misleadingly named and has unusual behavior.]
    It finds the **last** occurrence of a search list within a source list,
    then returns a new list containing the indices of **each element** from
    that final match.

Inputs
    • source_list_name … The name of the DynList to search within.
    • search_list_name … The name of the DynList to search for.

Returns
    • A new integer DynList of indices. The indices correspond to the
      elements of the last found sublist, not the starting positions of
      all matches.
    • Returns an empty list if no match is found.

Errors
    • TCL_ERROR if either list name is not found.
    • TCL_ERROR for incorrect number of arguments.
    • Note: Does not accept literal values for the search item. Optional
      arguments like `start_index` or `-all` are not functional.

Example
    set data [dl_slist a b c a b d c a b]
    set query [dl_slist c a b]

    # The last match of {c a b} starts at index 6.
    # The command returns the indices of each element of that match.
    dl_tcllist [dl_findIndices $data $query]
    # → 6 7 8

    # Searching for a single-element list returns the index of the last one.
    set data2 [dl_slist apple pear apple banana apple]
    set query2 [dl_slist apple]
    dl_tcllist [dl_findIndices $data2 $query2]
    # → 4

See also
    dl_find, dl_findAll

=== dl_findPatterns (Searching / Regular Expressions) ===
Synopsis
    dl_findPatterns <source_list_name> <search_list_name>

Brief
    [WARNING: This command is broken and does not use regular expressions.]
    Despite its name, this command performs a simple element-wise search. It
    finds all elements in the source list that exactly match any of the
    elements in the search list.

Inputs
    • source_list_name … The name of the DynList to search within.
    • search_list_name … The name of a DynList containing literal strings to
                         search for. Regular expression patterns do not work.

Returns
    • A new integer DynList of all 0-based indices where an exact match was found.
    • Returns an empty list if no matches are found.

Errors
    • TCL_ERROR if a list name is not found.

Example
    # The command performs exact matches, not pattern matching.
    set myStrings [dl_slist Apple Pears Banana Apricot GRAPE]
    set query [dl_slist Apple GRAPE]
    dl_tcllist [dl_findPatterns $myStrings $query]
    # → 0 4

    # This example from the original docs fails because regex is not supported.
    # set files [dl_slist file1.txt data.csv image.png notes.TXT]
    # set txt_pattern [dl_slist {\.txt$} -nocase]
    # dl_findPatterns $files $txt_pattern
    # → (returns an empty list)

See also
    dl_findAll, dl_stringmatch

=== dl_first (Accessing Elements) ===
Synopsis
    dl_first <list_name>

Brief
    Returns the first element of a dynamic list. If the list is a list of
    lists, it returns the name of the first sublist.

Inputs
    • list_name … The name of a DynList. The list must not be empty.

Returns
    • The first element of the list, or the name of the first sublist if it
      is a list of lists.

Errors
    • TCL_ERROR if the list is not found.
    • TCL_ERROR if the list is empty.

Example
    # Get the first element of a string list
    set myStrings [dl_slist apple banana cherry]
    dl_first $myStrings
    # → apple

    # Get the first element of an integer list
    set myNums [dl_ilist 10 20 30]
    dl_first $myNums
    # → 10

    # Get the name of the first sublist
    set sub1 [dl_ilist 1 2]
    set sub2 [dl_ilist 3 4]
    set lol [dl_llist $sub1 $sub2]
    dl_first $lol
    # → %listN% (the internal name of the first sublist)

See also
    dl_last, dl_get, dl_pickone

=== dl_firstPos (Indexing) ===
Synopsis
    dl_firstPos <list_name>

Brief
    [WARNING: This command is misnamed and does not find a position.]
    It takes a list as input and returns a new integer list of the same
    length. The new list contains a `1` at the first position (index 0)
    and `0` for all other positions.

Inputs
    • list_name … The name of any existing DynList.

Returns
    • A new integer DynList of the same length as the input list. The first
      element is `1`, and all subsequent elements are `0`.

Errors
    • TCL_ERROR if the list is not found.
    • Note: The documented behavior of finding a value at an index is
      incorrect. The command does not accept a value to find.

Example
    set myStrings [dl_slist a b c d e]
    dl_tcllist [dl_firstPos $myStrings]
    # → 1 0 0 0 0

    set myNums [dl_ilist 10 20 30]
    dl_tcllist [dl_firstPos $myNums]
    # → 1 0 0

See also
    dl_lastPos

=== dl_first_index_list (Searching / Indexing) ===
Synopsis
    dl_first_index_list <source_dynlist_name> <search_values_dynlist_name> [start_index]

Brief
    [WARNING: This command does not exist.]
    The documentation indicates this command should find the index of the
    first element in a list that matches any value from a second list, but
    it is not a valid command in the `essctrl` environment.

See also
    dl_find, dl_firstPos, dl_index_list, dl_oneof

=== dl_flist (Creation) ===
Synopsis
    dl_flist [value1 value2 ...]

Brief
    Creates a new dynamic list of type float from a sequence of numeric values.

Inputs
    • valueN … A sequence of numeric values. Non-float values will be
               converted to floats.

Returns
    • Type …………… string (name of a new float DynList)
    • Element type … float
    • Notes ………… If called with no arguments, it creates an empty float list.

Errors
    • TCL_ERROR if any argument is not a valid number.
    • Note: Contrary to some documentation, this command does not accept Tcl
      lists or other DynList names as arguments.

Example
    # Create a list from numbers
    dl_tcllist [dl_flist 1 2.5 3]
    # → 1.0 2.5 3.0

    # Create an empty float list
    set empty [dl_flist]
    dl_length $empty
    # → 0

    # The following documented examples will fail:
    # dl_flist {1.0 2.0}
    # set other [dl_ilist 1 2]; dl_flist $other

See also
    dl_ilist, dl_slist, dl_llist, dl_float

=== dl_float (Conversion) ===
Synopsis
    dl_float <source_list>

Brief
    Converts the elements of a source list to float type, returning a new
    float DynList.

Inputs
    • source_list … The name of a numeric DynList or a Tcl list of numeric
                    values provided as a single string.

Returns
    • Type …………… string (name of a new float DynList)
    • Element type … float

Errors
    • TCL_ERROR if the source list is not found or contains non-numeric values.
    • Note: The documented optional `output_var_name` argument is not
      functional and should not be used. The command only accepts a single
      list argument.

Example
    # Convert an integer list to a float list
    set intList [dl_ilist 1 2 30]
    dl_tcllist [dl_float $intList]
    # → 1.0 2.0 30.0

    # Convert a Tcl list string to a float list
    dl_tcllist [dl_float {5 6.6 7}]
    # → 5.0 6.6 7.0

See also
    dl_flist, dl_int, dl_string

=== dl_floor (Arithmetic) ===
Synopsis
    dl_floor <list> | dl_floor <number>

Brief
    Computes the element-wise floor of a list or number (the largest
    integer less than or equal to each value). Returns a new list.

Inputs
    • Type …………… DynList_or_Number
    • Length ……… 1
    • Element types … numeric

Returns
    • Type …………… DynList (new object)
    • Element type … integer

Errors
    • TCL_ERROR if the input list is not found or is not numeric.

Example
    # Get the floor for each element in a list
    set values [dl_flist 1.7 2.0 -3.2 4.99]
    dl_tcllist [dl_floor $values]
    # → 1 2 -4 4

    # Get the floor of a single number
    dl_tcllist [dl_floor -5.5]
    # → -6

See also
    dl_ceil, dl_round, dl_int

=== dl_fmod (Arithmetic) ===
Synopsis
    dl_fmod <dividend_list_or_val> <divisor_list_or_val>

Brief
    Computes the element-wise floating-point remainder of x/y. The result
    has the same sign as the dividend (x). Returns a new list of floats.

Inputs
    • dividend … A numeric DynList or a scalar value.
    • divisor …… A numeric DynList or a scalar value.
    • Constraints … Inputs must be broadcastable. Divisor cannot be zero.

Returns
    • Type …………… DynList (new object)
    • Element type … float

Errors
    • TCL_ERROR if lists have incompatible lengths or if division by zero
      is attempted.

Example
    # List % List
    set l1 [dl_flist 5.0 -5.0 5.0 -5.0 2.7]
    set l2 [dl_flist 3.0 3.0 -3.0 -3.0 1.3]
    dl_tcllist [dl_fmod $l1 $l2]
    # → 2.0 -2.0 2.0 -2.0 0.1...

    # List % Scalar
    dl_tcllist [dl_fmod $l1 2.5]
    # → 0.0 -0.0 0.0 -0.0 0.2...

See also
    dl_div, dl_mod

=== dl_foreach (Control Structure) ===
Synopsis
    dl_foreach <varName> <list_name> <scriptBody>

Brief
    A loop that iterates over each element in a single list, assigning the
    element's value to `varName` and executing a Tcl script for each element.

Inputs
    • varName …… A string to use as the iterator variable name.
    • list_name …… The name of the DynList to iterate over.
    • scriptBody … A string containing the Tcl script to execute.

Returns
    • The result of the last command evaluated in the final iteration of the
      script body. Returns an empty string if the loop does not run.

Side Effects
    • The script body is executed for each element in the list.
    • The variable specified by `varName` is created and modified within the
      scope of the script body.

Errors
    • TCL_ERROR if the list is not found or if the wrong number of arguments
      is provided.

Example
    # Create an empty list, then append to it for each item in a source list
    set source [dl_slist a b c]
    set result [dl_slist]
    dl_foreach letter $source {
        dl_append $result "item-$letter"
    }
    dl_tcllist $result
    # → item-a item-b item-c

    # Sum the elements of a numeric list
    set numbers [dl_ilist 1 2 3 4 5]
    set sum 0
    dl_foreach n $numbers {
        set sum [expr {$sum + $n}]
    }
    # To see the result, ensure the variable is the last expression
    set sum
    # → 15

See also
    dl_dotimes

=== dl_fromto (Creation) ===
Synopsis
    dl_fromto <start> <stop> [step]

Brief
    Creates a new list of numbers starting from `start`, incrementing by
    `step`, up to but not including `stop`.

Inputs
    • start … The starting numeric value.
    • stop …… The exclusive ending numeric value. The sequence will stop
              before reaching this value.
    • step …… Optional numeric increment (default is 1 or -1 depending
              on the direction from start to stop).

Returns
    • A new numeric DynList. The type is float if any argument is a float.

Errors
    • TCL_ERROR: `dl_fromto: unable to create series` if the `step` is 0 or
      has the wrong sign for the range (e.g., a negative step for a
      start-to-stop range that is increasing).
    • TCL_ERROR: `dynlist "..." not found` if a non-numeric value is provided,
      as it is misinterpreted as a list name.

Example
    # Integer sequence, stop is not included
    dl_tcllist [dl_fromto 1 5]
    # → 1 2 3 4

    # Float sequence with a step
    dl_tcllist [dl_fromto 0 10 2.5]
    # → 0.0 2.5 5.0 7.5

    # Negative step, stop is not included
    dl_tcllist [dl_fromto 5 1]
    # → 5 4 3 2

See also
    dl_ilist, dl_flist, dl_fill

=== dl_gaussian2D (Creation) ===
Synopsis
    dl_gaussian2D <width> <height> <sigma>

Brief
    [WARNING: This command is misleadingly named.]
    It generates a 1D float list containing the values of a 2D Gaussian
    distribution. The values are flattened row by row into a single list.
    It does not return a 2D list (a list of lists).

Inputs
    • width …… Integer width of the conceptual 2D kernel.
    • height …… Integer height of the conceptual 2D kernel.
    • sigma …… Float standard deviation for the Gaussian function.

Returns
    • A new 1D float DynList of size `width * height`.

Errors
    • TCL_ERROR: `expected integer but got "..."` if width or height are
      not valid integers.
    • TCL_ERROR: `expected floating-point number but got "..."` if sigma
      is not a valid float.

Example
    # Generate a 3x3 Gaussian kernel, returned as a 1x9 list.
    set kernel_flat [dl_gaussian2D 3 3 1.0]
    dl_tcllist $kernel_flat
    # → 0.135... 0.367... 0.135... 0.367... 1.0 0.367... 0.135... 0.367... 0.135...

See also
    dl_ones, dl_fromto

=== dl_generateDynList (Creation / Advanced) ===
Synopsis
    dl_generateDynList <count> <script_body> [type_hint]

Brief
    [WARNING: This command does not exist.]
    The documentation indicates this command should generate a list by
    executing a script, but it is not a valid command in the `essctrl`
    environment.

See also
    dl_dotimes, dl_fromto

=== dl_get (Accessing Elements) ===
Synopsis
    dl_get <list_name> <index>

Brief
    Retrieves the element at a specific 0-based index from a list.

Inputs
    • list_name … The name of the DynList to access.
    • index ……… The 0-based integer index of the element to get.

Returns
    • The value of the element at the specified index.
    • If the element is itself a list, the name of that sublist is returned.

Errors
    • TCL_ERROR: `dl_get: index out of range` if the index is invalid.
    • WARNING: This command does not support multi-level indexing with
      multiple index arguments. Extra arguments are ignored in a buggy and
      unpredictable way and should not be used.

Example
    # Get a simple value from a list
    set letters [dl_slist a b c d e]
    dl_get $letters 2
    # → c

    # Get the name of a sublist
    set sub1 [dl_ilist 1 2]
    set lol [dl_llist $sub1]
    dl_get $lol 0
    # → %listN% (the internal name of sub1)

    # To access a nested element, use two separate dl_get calls
    set sub2 [dl_slist x y z]
    set lol2 [dl_llist [dl_ilist 1 2] $sub2]
    set sublist_name [dl_get $lol2 1]
    dl_get $sublist_name 2
    # → z

See also
    dl_first, dl_last, dl_choose, dl_index

=== dl_gt (Logical / Comparison) ===
Synopsis
    dl_gt <list_or_val1> <list_or_val2>

Brief
    Performs an element-wise greater than (>) comparison between two lists
    or a list and a scalar value.

Inputs
    • list_or_val1 … A numeric DynList or a scalar value.
    • list_or_val2 … A numeric DynList or a scalar value.
    • Constraints …… Inputs must be numeric and broadcastable (i.e., have
                     the same length or one is a scalar).

Returns
    • A new integer DynList of 0s (false) and 1s (true).

Errors
    • TCL_ERROR: `dl_gt: unable to compare...` if lists have different lengths.
    • TCL_ERROR if an input list name is not found.
    • Note: The documented support for chained comparisons (3+ arguments)
      is incorrect and will cause a usage error.

Example
    # List vs List
    set l1 [dl_ilist 5 2 8 1 9]
    set l2 [dl_ilist 3 7 8 0 2]
    dl_tcllist [dl_gt $l1 $l2]
    # → 1 0 0 1 1

    # List vs Scalar
    dl_tcllist [dl_gt $l1 4]
    # → 1 0 1 0 1

See also
    dl_lt, dl_gte, dl_eq, dl_gtIndex

=== dl_gtIndex (Logical / Indexing) ===
Synopsis
    dl_gtIndex <list_or_val1> <list_or_val2>

Brief
    Performs an element-wise greater than (>) comparison and returns a new
    list containing the 0-based indices where the result is true.

Inputs
    • list_or_val1 … A numeric DynList or a scalar value.
    • list_or_val2 … A numeric DynList or a scalar value.
    • Constraints …… Inputs must be numeric and broadcastable.

Returns
    • A new integer DynList of 0-based indices where the comparison is true.

Errors
    • TCL_ERROR: `dl_gtIndex: unable to compare...` if lists have different
      lengths.
    • TCL_ERROR if an input list name is not found.
    • Note: The documented support for chained comparisons (3+ arguments)
      is incorrect and will cause a usage error.

Example
    # List vs List
    set l1 [dl_ilist 5 2 8 1 9]
    set l2 [dl_ilist 3 7 8 0 2]
    dl_tcllist [dl_gtIndex $l1 $l2]
    # → 0 3 4

    # List vs Scalar
    dl_tcllist [dl_gtIndex $l1 4]
    # → 0 2 4

See also
    dl_ltIndex, dl_gteIndex, dl_eqIndex, dl_gt

=== dl_help (Introspection) ===
Synopsis
    dl_help [command_name]

Brief
    [WARNING: This command appears to be non-functional.]
    It is intended to provide help information for DynList commands, but it
    produces no output to standard out or standard error when called via
    `essctrl`.

Inputs
    • command_name … An optional string with the name of a `dl_` command.

Returns
    • Nothing. The command does not produce any observable output.

Example
    # Both of these commands produce no output.
    # dl_help
    # dl_help dl_add

See also
    dl_dir

=== dl_hist (Utility / Histogramming) ===
Synopsis
    dl_hist <data_list> <start> <stop> <nbins>

Brief
    Computes a histogram of a numeric data list. It bins the data into
    `nbins` even intervals between `start` and `stop` and returns the
    count in each bin.

Inputs
    • data_list … A numeric DynList.
    • start ……… The lower bound of the histogram range.
    • stop ……… The upper bound of the histogram range.
    • nbins ……… The number of bins to create.

Returns
    • A new integer DynList containing the count for each bin.

Errors
    • TCL_ERROR: `dl_hist: bad operand...` if the data list is not numeric.
    • TCL_ERROR: `expected integer but got "..."` if nbins is not an integer.
    • TCL_ERROR if start/stop are not numeric.

Example
    # Bins the data into 4 equal bins from 0 to 6
    # Bins are [0-1.5), [1.5-3), [3-4.5), [4.5-6)
    set data [dl_flist 1 1 2 2 2 3 4 5]
    dl_tcllist [dl_hist $data 0 6 4]
    # → 2 3 2 1

See also
    dl_bins, dl_histBins

=== dl_histBins (Utility / Histogramming) ===
Synopsis
    dl_histBins <data_list> <bin_edges_list>

Brief
    [WARNING: This command does not exist.]
    The documentation indicates this command should compute a histogram using
    predefined bin edges, but it is not a valid command in the `essctrl`
    environment.

See also
    dl_hist, dl_bins

=== dl_histLists (Utility / Histogramming) ===
Synopsis
    dl_histLists <list_of_lists> <nbins>

Brief
    [WARNING: This command does not exist.]
    The documentation indicates this command should compute histograms for a
    list of lists, but it is not a valid command in the `essctrl` environment.

See also
    dl_hist

=== dl_hmean / dl_hstd / dl_hvar (Reduction) ===

Brief
    [WARNING: These commands do not exist.]
    The documentation mentions `dl_hmean` (harmonic mean), `dl_hstd`
    (harmonic standard deviation), and `dl_hvar` (harmonic variance), but
    none of these are valid commands in the `essctrl` environment.

See also
    dl_mean, dl_std, dl_var

=== dl_idiff (Arithmetic) ===
Synopsis
    dl_idiff <list1> [list2]

Brief
    [WARNING: This command is misnamed and has inconsistent behavior.]
    This command's function changes drastically based on the number of
    arguments and does not perform a standard in-place difference.

Inputs & Behavior
    Case 1: Two Arguments (`dl_idiff <list1> <list2>`)
    • It computes an outer-product subtraction, returning a new flattened
      list. For each element in `list1`, it subtracts every element from
      `list2` and concatenates the results.
    • It does NOT modify `list1` in-place.
    • The size of the returned list is `len(list1) * len(list2)`.

    Case 2: One Argument (`dl_idiff <list>`)
    • It performs an undocumented and seemingly nonsensical transformation on
      the list, returning a new list. The logic is not apparent.

Returns
    • A new DynList. Its contents depend on the number of arguments.

Errors
    • TCL_ERROR if lists contain non-numeric data or have incompatible
      types for the operation.

Example (Two arguments)
    # Outer-product subtraction:
    set l1 [dl_ilist 10 20]
    set l2 [dl_ilist 1 2 3]
    dl_tcllist [dl_idiff $l1 $l2]
    # → 9 8 7 19 18 17
    # (10-1, 10-2, 10-3, 20-1, 20-2, 20-3)

Example (One argument)
    # Bizarre, undocumented transformation:
    set l1 [dl_ilist 10 20 30]
    dl_tcllist [dl_idiff $l1]
    # → -10 -20 10 -10 20 10

See also
    dl_diff, dl_sub

=== dl_ilist (Creation) ===
Synopsis
    dl_ilist [value1 value2 ...]

Brief
    Creates a new dynamic list of type long integer.

Inputs
    • valueN … A sequence of integer values.

Returns
    • The name of a new integer DynList.
    • If called with no arguments, it creates an empty integer list.

Errors
    • TCL_ERROR: `expected integer but got "..."` if any argument is not a
      valid integer. It does not truncate floats.

Example
    # Create a list of integers
    dl_tcllist [dl_ilist 1 -5 9]
    # → 1 -5 9

    # This will fail, as floats are not automatically truncated
    # dl_ilist 1 5 9.9

See also
    dl_flist, dl_slist, dl_llist

=== dl_increment / dl_incr (Arithmetic) ===
Synopsis
    dl_increment <list_name> [index]
    dl_incr <list_name> [index]

Brief
    Increments an element of a numeric list by 1 in-place. `dl_incr` is an
    alias for `dl_increment`.

    [WARNING: The whole-list operation is broken.]

Inputs & Behavior
    Case 1: With Index (`dl_increment <list_name> <index>`)
    • This is the only reliable way to use this command.
    • It correctly increments the integer element at the specified `index`
      by 1, modifying the list in-place.

    Case 2: Without Index (`dl_increment <list_name>`)
    • This usage is BUGGY. Instead of incrementing all elements, it appends
      a new element to the list, whose value is the original last element
      plus one. It should not be used.

Side Effects
    • The input list is modified.

Errors
    • TCL_ERROR: `dl_increment: index out of range` if the index is invalid.
    • TCL_ERROR: `dl_increment: list must be ints` if the list is not an
      integer list.

Example (Correct Usage)
    set nums [dl_ilist 5 10 15]
    dl_increment $nums 1
    dl_tcllist $nums
    # → 5 11 15

Example (Buggy Usage)
    set buggy_nums [dl_ilist 5 10 15]
    dl_increment $buggy_nums
    dl_tcllist $buggy_nums
    # → 5 10 15 16  (Incorrectly appends 15+1)

See also
    dl_add

=== dl_index (Creation / Sequence Generation) ===
Synopsis
    dl_index <list_name>

Brief
    Creates a new integer list containing the indices of the input list.
    The new list will be `0, 1, 2, ...` up to `length-1`.

Inputs
    • list_name … The name of any existing DynList.

Returns
    • A new integer DynList containing the sequence of indices.

Errors
    • TCL_ERROR: `dynlist "..." not found` if the list does not exist.
    • Note: This command is NOT an alias for `dl_get`. It takes only one
      argument and its function is completely different.

Example
    set letters [dl_slist a b c]
    dl_tcllist [dl_index $letters]
    # → 0 1 2

    set empty [dl_slist]
    dl_tcllist [dl_index $empty]
    # → (empty list)

See also
    dl_get, dl_fromto

=== dl_index_list (Searching / Indexing) ===
Synopsis
    dl_index_list <source_dynlist_name> <search_values_dynlist_name> [start_index]

Brief
    [WARNING: This command does not exist.]
    The documentation indicates this command should find the indices of
    elements in a source list that match values from a search list, but it
    is not a valid command in the `essctrl` environment.

See also
    dl_find, dl_oneof

=== dl_insert (Manipulation) ===
Synopsis
    dl_insert <list_name> <index> <value>

Brief
    Inserts a value into a list at a specified 0-based index. This command
    modifies the list in-place.

Inputs
    • list_name … Name of the DynList to modify.
    • index ……… 0-based integer index at which to insert the value.
    • value ……… The value to insert. Must be compatible with the list's type.

Side Effects
    • The input list is modified by inserting the new element.

Errors
    • TCL_ERROR: `dl_insert: insert position out of range` if index is invalid.
    • TCL_ERROR: `expected <type> but got "..."` if the value's type is
      incompatible with the list's type.

Example
    set letters [dl_slist a b d]
    dl_insert $letters 2 c
    dl_tcllist $letters
    # → a b c d

See also
    dl_append, dl_prepend, dl_replace

=== dl_int (Conversion) ===
Synopsis
    dl_int <source_list>

Brief
    Converts elements of a source list to integer type, returning a new
    integer list. Floats are truncated.

    [WARNING: This command has a critical bug.]

Inputs
    • source_list … A numeric DynList or a Tcl list of numeric values.

Returns
    • A new integer DynList.

Errors & Bugs
    • The optional `[tclvar]` argument documented in the usage message is
      non-functional and is ignored.
    • BUG: If the source list contains non-numeric strings, the command does
      not fail. Instead, it silently converts these strings to `0`, which
      can lead to data corruption.

Example (Correct Usage)
    # Convert a float list to an integer list (truncates)
    set floats [dl_flist 1.1 2.9 -3.5]
    dl_tcllist [dl_int $floats]
    # → 1 2 -3

Example (Buggy Behavior)
    # Non-numeric strings are silently converted to 0
    set strings [dl_slist "hello" "world" "5a"]
    dl_tcllist [dl_int $strings]
    # → 0 0 0

See also
    dl_ilist, dl_float, dl_string

=== dl_interleave (Manipulation / Restructuring) ===
Synopsis
    dl_interleave <list1> <list2> [<list3>...]

Brief
    Creates a new list by interleaving the elements of two or more lists.
    For example, `[1, 2]` and `[a, b]` would become `[1, a, 2, b]`.

Inputs
    • listN … Two or more DynLists.
    • Constraints … All input lists must have the same length and the
                    same data type.

Returns
    • A new DynList containing the interleaved elements.

Errors
    • TCL_ERROR: `dl_interleave: error interleaving lists` if the lists have
      different lengths or different data types.

Example
    set l1 [dl_ilist 1 3 5]
    set l2 [dl_ilist 2 4 6]
    dl_tcllist [dl_interleave $l1 $l2]
    # → 1 2 3 4 5 6

    # This will fail due to different lengths
    # set l3 [dl_ilist 1 2]
    # set l4 [dl_ilist 3 4 5]
    # dl_interleave $l3 $l4

See also
    dl_concat, dl_collapse

=== dl_irand (Creation / Random) ===
Synopsis
    dl_irand <size> <n>

Brief
    Creates a new integer list containing <size> random integers in the
    range [0, n).

Inputs
    • size … The number of random integers to generate.
    • n ……… The exclusive integer upper bound.

Returns
    • A new integer DynList of random numbers.

Errors
    • TCL_ERROR: `dl_irand: invalid size specification...` if size is not a
      valid positive integer.
    • TCL_ERROR: `expected integer but got "..."` if n is not a valid integer.

Example
    # Generate a list of 10 random numbers between 0 and 99
    set rands [dl_irand 10 100]
    dl_length $rands
    # → 10

See also
    dl_fromto, dl_fill

=== dl_isMatrix (Introspection / Type Checking) ===
Synopsis
    dl_isMatrix <list_name>

Brief
    [WARNING: This command appears to be non-functional.]
    It is intended to check if a list is a matrix, but it returns `0` (false)
    for all tested list types, including 1D lists and 2D lists of lists.
    It may be looking for a deprecated or unavailable internal matrix type.

Inputs
    • list_name … The name of a DynList to check.

Returns
    • `0` in all tested cases.

Example
    # Returns 0
    set l1 [dl_ilist 1 2 3]
    dl_isMatrix $l1

    # Also returns 0
    set m [dl_llist [dl_ilist 1 2] [dl_ilist 3 4]]
    dl_isMatrix $m

See also
    dl_datatype, dl_depth
