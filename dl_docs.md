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
    dl_create <type> [value1 value2 ...]

Brief
    Creates a new dynamic list of a specified `type` with optional initial
    values. This is the general-purpose list constructor and is generally
    stricter about type matching than the specialized `dl_ilist`, `dl_flist`,
    etc. commands.

Inputs
    • type ……… A string specifying the list's data type. Common types are:
                 `long` (or `int`), `float`, `string`, `list`, `char`.
    • valueN … Optional. A sequence of initial values for the list.

Returns
    • The name of a new DynList.

Errors
    • TCL_ERROR: `bad datatype` if the `type` string is invalid.
    • TCL_ERROR if any provided value is incompatible with the specified type.

Type-Specific Behavior
    • `long` or `int`: Requires all values to be valid integers. Does not truncate floats.
      `dl_create long 1 2.5` → `expected integer but got "2.5"`
    • `float`: Accepts integers and floats, promoting integers to floats.
      `dl_tcllist [dl_create float 1 2.5]` → `1.0 2.5`
    • `string`: Accepts any value and converts it to its string representation.
      `dl_tcllist [dl_create string 1 2.5 hi]` → `1 2.5 hi`
    • `list`: Requires all values to be names of existing DynLists.
      `dl_create list $l1 "foo"` → `dynlist "foo" not found`
    • `char`: Requires all values to be valid integers (character codes). Does
      not truncate floats.
      `dl_create char 65 66.5` → `expected integer but got "66.5"`

Example
    # Create an integer list (type 'long')
    dl_tcllist [dl_create long 1 2 3]
    # → 1 2 3

    # Create a list of lists
    set l1 [dl_ilist 1]; set l2 [dl_slist a b]
    set lol [dl_create list $l1 $l2]
    dl_tcllist [dl_lengths $lol]
    # → 1 2

See also
    dl_ilist, dl_flist, dl_slist, dl_llist, dl_datatype


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
    • list_or_val1 … A DynList or a scalar value.
    • list_or_val2 … A DynList or a scalar value.
    • Constraints …… Inputs must be broadcastable (have the same length or
                     one is a scalar).

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
    dl_lt, dl_lte, dl_eq, dl_noteq, dl_gtIndex

=== dl_gtIndex (Logical / Indexing) ===
Synopsis
    dl_gtIndex <list_or_val1> <list_or_val2>

Brief
    Performs an element-wise greater than (>) comparison and returns a new
    list containing the 0-based indices where the result is true.

Inputs
    • list_or_val1 … A DynList or a scalar value.
    • list_or_val2 … A DynList or a scalar value.
    • Constraints …… Inputs must be broadcastable.

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
    dl_gt, dl_ltIndex, dl_gteIndex, dl_eqIndex, dl_noteqIndex

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

=== dl_last (Accessing Elements) ===
Synopsis
    dl_last <list_name>

Brief
    Returns the last element of a dynamic list. If the list is a list of
    lists, it returns the name of the last sublist.

Inputs
    • list_name … The name of a DynList.

Returns
    • The last element of the list, or the name of the last sublist if it
      is a list of lists.

Errors
    • TCL_ERROR if the list is not found.
    • Note: The command should error if the list is empty, but it was found
      to fail silently (produces no output and no TCL error).

Example
    # Get the last element of a string list
    set myStrings [dl_slist apple banana cherry]
    dl_last $myStrings
    # → cherry

    # Get the name of the last sublist and then its contents
    set sub1 [dl_ilist 1 2]
    set sub2 [dl_ilist 3 4]
    set lol [dl_llist $sub1 $sub2]
    set last_sublist [dl_last $lol]
    dl_tcllist $last_sublist
    # → 3 4

See also
    dl_first, dl_get, dl_pickone

=== dl_lastPos (Indexing) ===
Synopsis
    dl_lastPos <list_name>

Brief
    [WARNING: This command is misnamed and does not find a position.]
    It takes a list as input and returns a new integer list of the same
    length. The new list contains a `1` at the last position (index -1)
    and `0` for all other positions.

Inputs
    • list_name … The name of any existing DynList.

Returns
    • A new integer DynList of the same length as the input list. The last
      element is `1`, and all preceding elements are `0`.

Errors
    • TCL_ERROR if the list is not found.
    • Note: The documented behavior of finding a value at an index is
      incorrect. The command does not accept a value to find.

Example
    set myStrings [dl_slist a b c d e]
    dl_tcllist [dl_lastPos $myStrings]
    # → 0 0 0 0 1

    set myNums [dl_ilist 10 20 30]
    dl_tcllist [dl_lastPos $myNums]
    # → 0 0 1

See also
    dl_firstPos

=== dl_last_index_list (Searching / Indexing) ===
Synopsis
    dl_last_index_list <source_dynlist_name> <search_values_dynlist_name> [end_index]

Brief
    [WARNING: This command does not exist.]
    The documentation indicates this command should find the index of the
    last element in a list that matches any value from a second list, but
    it is not a valid command in the `essctrl` environment.

See also
    dl_first_index_list, dl_lastPos, dl_find, dl_oneof

=== dl_length (Introspection) ===
Synopsis
    dl_length <list_name>

Brief
    Returns the number of elements in the top level of a dynamic list.

Inputs
    • list_name … The name of an existing DynList.

Returns
    • An integer representing the number of elements. If it is a list of
      lists, it returns the count of sublists.

Errors
    • TCL_ERROR if the list is not found.

Example
    # Get the length of a simple list
    set simpleList [dl_ilist 10 20 30 40]
    dl_length $simpleList
    # → 4

    # Get the length of a list of lists
    set sub1 [dl_slist a b]
    set sub2 [dl_slist c d e]
    set listOfLists [dl_llist $sub1 $sub2]
    dl_length $listOfLists
    # → 2

See also
    dl_depth, dl_llength

=== dl_lengths (Introspection / List of Lists) ===
Synopsis
    dl_lengths <list_of_lists_name>

Brief
    Returns a new integer list containing the lengths of each sublist
    within a list of lists.

Inputs
    • list_of_lists_name … The name of a DynList that is a list of lists.

Returns
    • A new integer DynList where each element is the length of the
      corresponding sublist in the input list.

Errors
    • TCL_ERROR if the input list is not found.
    • TCL_ERROR: `dl_lengths: bad operand` if the input is not a list of lists.

Example
    set subA [dl_ilist 1 2 3]
    set subB [dl_slist x y]
    set subC [dl_ilist 10]
    set subD [dl_slist]
    set lol [dl_llist $subA $subB $subC $subD]
    dl_tcllist [dl_lengths $lol]
    # → 3 2 1 0

See also
    dl_length, dl_depth

=== dl_lgamma (Arithmetic / Special Functions) ===
Synopsis
    dl_lgamma <list> | dl_lgamma <number>

Brief
    Computes the element-wise natural logarithm of the absolute value of the
    Gamma function, `log(|Γ(x)|)`. Returns a new list of floats.

Inputs
    • Type …………… DynList_or_Number
    • Element types … numeric

Returns
    • A new float DynList.
    • For non-positive integer inputs (0, -1, -2, ...), where the Gamma
      function has poles, the command returns `Inf`.

Errors
    • TCL_ERROR if the input is not numeric or a list is not found.

Example
    # For a list of values
    set values [dl_flist 0.5 1 2 3.5 -1.5]
    dl_tcllist [dl_lgamma $values]
    # → 0.572365 0.0 0.0 1.20097 0.860047

    # For a single value
    dl_tcllist [dl_lgamma 5]
    # → 3.17805

    # For a non-positive integer
    dl_tcllist [dl_lgamma -2]
    # → Inf

See also
    dl_exp, dl_log

=== dl_llength (Introspection / List of Lists) ===
Synopsis
    dl_llength <list_of_lists_name>

Brief
    [WARNING: This command is not an alias for `dl_length`.]
    It is an alias for `dl_lengths` and returns a new integer list containing
    the lengths of each sublist within a list of lists.

Inputs
    • list_of_lists_name … The name of a DynList that is a list of lists.

Returns
    • A new integer DynList where each element is the length of the
      corresponding sublist in the input list.

Errors
    • TCL_ERROR if the input list is not found.
    • TCL_ERROR if the input is not a list of lists.

Example
    set subA [dl_ilist 1 2 3]
    set subB [dl_slist x y]
    set lol [dl_llist $subA $subB]
    dl_tcllist [dl_llength $lol]
    # → 3 2

See also
    dl_lengths, dl_length, dl_depth

=== dl_llist (Creation / List of Lists) ===
Synopsis
    dl_llist [dynlist_name1 dynlist_name2 ...]

Brief
    Creates a new dynamic list designed to hold other dynamic lists as its
    elements (a list of lists). It can be initialized with existing lists.

Inputs
    • dynlist_nameN … Optional. One or more names of existing DynLists to
                      add to the new list of lists.

Returns
    • The name of a new DynList of type List.
    • If called with no arguments, it creates an empty list of lists.

Errors
    • TCL_ERROR: `dynlist "..." not found` if any provided list name does
      not exist.
    • Note: Does not accept a Tcl list of list names, contrary to some
      documentation.

Example
    # Create an empty list of lists
    set lol1 [dl_llist]
    dl_length $lol1
    # → 0

    # Create a list of lists from existing lists
    set listA [dl_ilist 1 2]
    set listB [dl_slist hello world]
    set lol2 [dl_llist $listA $listB]
    dl_tcllist [dl_lengths $lol2]
    # → 2 2

See also
    dl_ilist, dl_flist, dl_slist, dl_append, dl_depth

=== dl_local (Creation / Scoping) ===
Synopsis
    dl_local <new_list_name> <source_list_to_copy>

Brief
    Creates a new dynamic list that is local to the current Tcl procedure
    (`proc`) scope. It creates the new list by copying an existing source
    list. This command is essential for creating temporary, modifiable
    lists inside `proc`s without affecting global lists.

Inputs
    • new_list_name ………… The string name for the new local list.
    • source_list_to_copy … The name of an existing DynList to copy. This
                              can be a temporary list created on the fly (e.g.,
                              `[dl_ilist]`).

Returns
    • This command does not return a value. It creates the new list as a
      side effect.

Side Effects
    • A new DynList with the given name is created in the local scope.

Usage Notes
    • This command is intended for use inside a Tcl `proc`. Lists created
      with `dl_local` are generally not accessible from the global scope
      after the `proc` finishes.
    • To return a list from a `proc`, use `dl_return`.

Example
    # This proc finds the indices of given IDs within a shape list.
    proc get_shape_indices { shape_list shape_ids } {
        # Create a new empty integer list named "inds", local to this proc.
        # It copies an empty temporary list created by [dl_ilist].
        dl_local inds [dl_ilist]

        # Iterate and append found indices to the local list.
        foreach s [dl_tcllist $shape_ids] {
            dl_append $inds [dl_find $shape_list $s]
        }

        # Return the populated local list.
        dl_return $inds
    }

See also
    dl_return, dl_create, dl_ilist, dl_flist, dl_slist

=== dl_log (Arithmetic / Transcendental) ===
Synopsis
    dl_log <list> | dl_log <number>

Brief
    Computes the element-wise natural logarithm (ln) of a numeric list or
    a single number. Returns a new list of floats.

Inputs
    • Type …………… DynList_or_Number
    • Element types … numeric

Returns
    • A new float DynList.
    • For an input of `0`, the result is `-Inf`.
    • For negative inputs, the result is `NaN` (Not a Number).

Errors
    • TCL_ERROR if the input is not numeric or a list is not found.
    • Note: The command does not produce a TCL error for out-of-domain
      inputs (zero or negative numbers), contrary to some documentation.

Example
    # For a list of positive values
    set values [dl_flist 1 2.71828 10 0.5]
    dl_tcllist [dl_log $values]
    # → 0.0 1.0 2.30259 -0.693147

    # For out-of-domain values
    dl_tcllist [dl_log [dl_flist 0 -5]]
    # → -Inf NaN

See also
    dl_exp, dl_log10

=== dl_log10 (Arithmetic / Transcendental) ===
Synopsis
    dl_log10 <list> | dl_log10 <number>

Brief
    Computes the element-wise base-10 logarithm of a numeric list or a
    single number. Returns a new list of floats.

Inputs
    • Type …………… DynList_or_Number
    • Element types … numeric

Returns
    • A new float DynList.
    • For an input of `0`, the result is `-Inf`.
    • For negative inputs, the result is `NaN` (Not a Number).

Errors
    • TCL_ERROR if the input is not numeric or a list is not found.
    • Note: The command does not produce a TCL error for out-of-domain
      inputs.

Example
    # For a list of values
    set values [dl_flist 1 10 100 0.1]
    dl_tcllist [dl_log10 $values]
    # → 0.0 1.0 2.0 -1.0

    # For out-of-domain values
    dl_tcllist [dl_log10 [dl_flist 0 -10]]
    # → -Inf NaN

See also
    dl_log, dl_exp

=== dl_lt (Logical / Comparison) ===
Synopsis
    dl_lt <list_or_val1> <list_or_val2>

Brief
    Performs an element-wise less than (<) comparison between two lists or
    a list and a scalar value. Returns a new list of 0s (false) and 1s (true).

Inputs
    • list_or_val1 … A DynList or a scalar value.
    • list_or_val2 … A DynList or a scalar value.
    • Constraints …… Inputs must be broadcastable (have the same length or
                     one is a scalar).

Returns
    • A new integer DynList containing 0s and 1s.

Errors
    • TCL_ERROR: `dl_lt: unable to compare...` if lists have different lengths.
    • TCL_ERROR if an input list name is not found.
    • Note: Does not support chained comparisons (3+ arguments).

Example
    # List vs List
    set l1 [dl_ilist 1 5 3 9 0]
    set l2 [dl_ilist 2 5 1 8 2]
    dl_tcllist [dl_lt $l1 $l2]
    # → 1 0 0 0 1

    # List vs Scalar
    dl_tcllist [dl_lt $l1 3]
    # → 1 0 0 0 1

See also
    dl_gt, dl_lte, dl_eq, dl_noteq, dl_ltIndex

=== dl_ltIndex (Logical / Indexing) ===
Synopsis
    dl_ltIndex <list_or_val1> <list_or_val2>

Brief
    Performs an element-wise less than (<) comparison and returns a new
    list containing the 0-based indices where the result is true.

Inputs
    • list_or_val1 … A DynList or a scalar value.
    • list_or_val2 … A DynList or a scalar value.
    • Constraints …… Inputs must be broadcastable.

Returns
    • A new integer DynList of 0-based indices where the comparison is true.

Errors
    • TCL_ERROR if lists have incompatible lengths.
    • Note: Does not support chained comparisons (3+ arguments).

Example
    # List vs List
    set l1 [dl_ilist 1 5 3 9 0]
    set l2 [dl_ilist 2 5 1 8 2]
    dl_tcllist [dl_ltIndex $l1 $l2]
    # → 0 4

    # List vs Scalar
    dl_tcllist [dl_ltIndex $l1 3]
    # → 0 4

See also
    dl_lt, dl_gtIndex, dl_lteIndex, dl_eqIndex

=== dl_lte (Logical / Comparison) ===
Synopsis
    dl_lte <list_or_val1> <list_or_val2>

Brief
    Performs an element-wise less than or equal to (<=) comparison between
    two lists or a list and a scalar value. Returns a new list of 0s and 1s.

Inputs
    • list_or_val1 … A DynList or a scalar value.
    • list_or_val2 … A DynList or a scalar value.
    • Constraints …… Inputs must be broadcastable.

Returns
    • A new integer DynList containing 0s and 1s.

Errors
    • TCL_ERROR if lists have incompatible lengths.
    • Note: Does not support chained comparisons (3+ arguments).

Example
    # List vs List
    set l1 [dl_ilist 1 5 3 8 0]
    set l2 [dl_ilist 2 5 1 8 2]
    dl_tcllist [dl_lte $l1 $l2]
    # → 1 1 0 1 1

    # List vs Scalar
    dl_tcllist [dl_lte $l1 3]
    # → 1 0 1 0 1

See also
    dl_lt, dl_gt, dl_gte, dl_eq

=== dl_lteIndex (Logical / Indexing) ===
Synopsis
    dl_lteIndex <list_or_val1> <list_or_val2>

Brief
    Performs an element-wise less than or equal to (<=) comparison and
    returns a new list containing the 0-based indices where the result is true.

Inputs
    • list_or_val1 … A DynList or a scalar value.
    • list_or_val2 … A DynList or a scalar value.
    • Constraints …… Inputs must be broadcastable.

Returns
    • A new integer DynList of 0-based indices where the comparison is true.

Errors
    • TCL_ERROR if lists have incompatible lengths.
    • Note: Does not support chained comparisons (3+ arguments).

Example
    # List vs List
    set l1 [dl_ilist 1 5 3 8 0]
    set l2 [dl_ilist 2 5 1 8 2]
    dl_tcllist [dl_lteIndex $l1 $l2]
    # → 0 1 3 4

    # List vs Scalar
    dl_tcllist [dl_lteIndex $l1 3]
    # → 0 2 4

See also
    dl_lte, dl_ltIndex, dl_gtIndex, dl_eqIndex

=== dl_max (Reduction) ===
Synopsis
    dl_max <list_name>

Brief
    Returns the maximum value from a 1D numeric list.

Inputs
    • list_name … The name of a 1D numeric DynList.

Returns
    • A single numeric value representing the maximum element in the list.

Errors
    • TCL_ERROR if the list is not found or is not numeric.
    • Note: The command fails silently (produces no output) if the list
      is empty, contrary to some documentation that suggests it should error.

Example
    set myInts [dl_ilist 10 50 20 5]
    dl_max $myInts
    # → 50

    set myFloats [dl_flist -1.0 2.5 -5.0 0.0]
    dl_max $myFloats
    # → 2.5

See also
    dl_min, dl_max_positions, dl_sum, dl_mean

=== dl_max_positions (Reduction / Indexing) ===
Synopsis
    dl_max_positions <list_name>

Brief
    [WARNING: This command does not exist.]
    The documentation indicates this command should return the indices of
    all occurrences of the maximum value in a list, but it is not a valid
    command in the `essctrl` environment.

See also
    dl_max, dl_min_positions, dl_eqIndex

=== dl_mean (Reduction) ===
Synopsis
    dl_mean <list_name>

Brief
    Calculates the arithmetic mean (average) of the elements in a 1D
    numeric list.

Inputs
    • list_name … The name of a 1D numeric DynList.

Returns
    • A single float value representing the arithmetic mean.
    • Returns `0.0` if the input list is empty.

Errors
    • TCL_ERROR if the list is not found or is not numeric.
    • Note: Does not produce an error for an empty list, contrary to some
      documentation.

Example
    set myInts [dl_ilist 10 20 30 40 50]
    dl_mean $myInts
    # → 30.0

    set myFloats [dl_flist 1.0 1.5 2.0 2.5 3.0]
    dl_mean $myFloats
    # → 2.0

See also
    dl_sum, dl_std, dl_var

=== dl_mean_list (Reduction) ===
Synopsis
    dl_mean_list <list_name>

Brief
    [WARNING: This command does not exist.]
    The documentation indicates this command should calculate the mean of a
    list and return the result as a new single-element list, but it is not
    a valid command in the `essctrl` environment.

See also
    dl_mean, dl_sum, dl_std

=== dl_min (Reduction) ===
Synopsis
    dl_min <list_name>

Brief
    Returns the minimum value from a 1D numeric list.

Inputs
    • list_name … The name of a 1D numeric DynList.

Returns
    • A single numeric value representing the minimum element in the list.

Errors
    • TCL_ERROR if the list is not found or is not numeric.
    • Note: Fails silently (produces no output) if the list is empty.

Example
    set myInts [dl_ilist 10 50 20 5 30]
    dl_min $myInts
    # → 5

    set myFloats [dl_flist -1.0 2.5 -5.0 0.0]
    dl_min $myFloats
    # → -5.0

See also
    dl_max, dl_min_positions, dl_sum, dl_mean

=== dl_min_positions (Reduction / Indexing) ===
Synopsis
    dl_min_positions <list_name>

Brief
    [WARNING: This command does not exist.]
    The documentation indicates this command should return the indices of
    all occurrences of the minimum value in a list, but it is not a valid
    command in the `essctrl` environment.

See also
    dl_min, dl_max_positions, dl_eqIndex

=== dl_mod (Arithmetic) ===
Synopsis
    dl_mod <dividend_list_or_val> <divisor_list_or_val>

Brief
    Computes the element-wise integer remainder of `x / y`. The result has
    the same sign as the dividend (`x`).

Inputs
    • dividend … A numeric DynList or a scalar value.
    • divisor …… A numeric DynList or a scalar value.
    • Constraints … Inputs must be numeric and broadcastable. Divisor
                    cannot be zero.

Returns
    • A new integer DynList containing the remainders.

Errors
    • TCL_ERROR if lists have incompatible lengths or if division by zero
      is attempted.

Example
    # The sign of the result matches the sign of the dividend (the first list)
    set l1 [dl_ilist 5 -5 5 -5]
    set l2 [dl_ilist 3 3 -3 -3]
    dl_tcllist [dl_mod $l1 $l2]
    # → 2 -2 2 -2

See also
    dl_fmod, dl_div

=== dl_mult (Arithmetic) ===
Synopsis
    dl_mult <list_or_val1> <list_or_val2> [<list_or_val3>...]

Brief
    Performs element-wise multiplication of two or more lists or numbers.

Inputs
    • Operands … Two or more numeric DynLists or scalar values.
    • Constraints … Inputs must be numeric and broadcastable.

Returns
    • A new DynList containing the results.

Errors
    • TCL_ERROR if lists have incompatible lengths.

Example
    # List * List
    set l1 [dl_ilist 1 2 3]
    set l2 [dl_ilist 10 20 30]
    dl_tcllist [dl_mult $l1 $l2]
    # → 10 40 90

    # Chained multiplication
    dl_tcllist [dl_mult $l1 $l2 [dl_ilist 2 2 2]]
    # → 20 80 180

See also
    dl_add, dl_sub, dl_div

=== dl_negate (Arithmetic) ===
Synopsis
    dl_negate <list> | dl_negate <number>

Brief
    Computes the element-wise negation (changes sign) of a numeric list
    or a single number. Returns a new list.

Inputs
    • Type …………… DynList_or_Number
    • Element types … numeric

Returns
    • A new DynList containing the negated values.

Errors
    • TCL_ERROR if the input is not numeric.

Example
    set myList [dl_flist -2 0 3.5 -1.2]
    dl_tcllist [dl_negate $myList]
    # → 2.0 -0.0 -3.5 1.2

    dl_tcllist [dl_negate 5]
    # → -5

See also
    dl_abs, dl_sub

=== dl_not (Logical) ===
Synopsis
    dl_not <list> | dl_not <number>

Brief
    Performs element-wise logical NOT on a numeric list or number. Non-zero
    values are treated as true (result `0`), and zero is treated as false
    (result `1`).

Inputs
    • Type …………… DynList_or_Number
    • Element types … numeric

Returns
    • A new integer DynList containing 0s and 1s.

Errors
    • TCL_ERROR if the input is not numeric.

Example
    set l1 [dl_flist 1 0 -2.5 0.0 7]
    dl_tcllist [dl_not $l1]
    # → 0 1 0 1 0

    # `not 0` is 1
    dl_tcllist [dl_not 0]
    # → 1

    # `not non-zero` is 0
    dl_tcllist [dl_not 100]
    # → 0

See also
    dl_and, dl_or, dl_eq

=== dl_noteq (Logical / Comparison) ===
Synopsis
    dl_noteq <list_or_val1> <list_or_val2>

Brief
    Performs an element-wise inequality (!=) comparison. Returns a new list
    of 0s (false) and 1s (true).

Inputs
    • list_or_val1 … A DynList or a scalar value.
    • list_or_val2 … A DynList or a scalar value.
    • Constraints …… Inputs must be broadcastable and of compatible types.
                     Numeric types (int, float) can be compared with each
                     other. String lists can be compared with other string
                     lists. Comparing numeric and string lists is not allowed.

Returns
    • A new integer DynList containing 0s and 1s.

Errors
    • TCL_ERROR: `dl_noteq: unable to compare...` if lists have incompatible
      lengths or data types.
    • Note: Does not support chained comparisons (3+ arguments).

Example
    # Integer list vs Float list
    set l1 [dl_ilist 1 2 3]
    set l2 [dl_flist 1.0 5.0 3.0]
    dl_tcllist [dl_noteq $l1 $l2]
    # → 0 1 0

    # String list vs String list
    set s1 [dl_slist a b c]
    set s2 [dl_slist a d c]
    dl_tcllist [dl_noteq $s1 $s2]
    # → 0 1 0

See also
    dl_eq, dl_gt, dl_lt, dl_noteqIndex

=== dl_noteqIndex (Logical / Comparison / Indexing) ===
Synopsis
    dl_noteqIndex <list_or_val1> <list_or_val2>

Brief
    Performs an element-wise inequality (!=) comparison and returns a new
    list of 0-based indices where the result is true.

Inputs
    • list_or_val1 … A DynList or a scalar value.
    • list_or_val2 … A DynList or a scalar value.
    • Constraints …… Inputs must be broadcastable and of compatible types.
                     Numeric types (int, float) can be compared with each
                     other. String lists can be compared with other string
                     lists.

Returns
    • A new integer DynList of 0-based indices where the comparison is true.

Errors
    • TCL_ERROR: `dl_noteqIndex: unable to compare...` if lists have
      incompatible lengths or data types.
    • Note: Does not support chained comparisons (3+ arguments).

Example
    # Integer list vs Float list
    set l1 [dl_ilist 1 2 3]
    set l2 [dl_flist 1.0 5.0 3.0]
    dl_tcllist [dl_noteqIndex $l1 $l2]
    # → 1

    # String list vs String list
    set s1 [dl_slist a b c]
    set s2 [dl_slist a d c]
    dl_tcllist [dl_noteqIndex $s1 $s2]
    # → 1

See also
    dl_noteq, dl_eqIndex, dl_gtIndex, dl_ltIndex

=== dl_oneof (Logical / Membership) ===
Synopsis
    dl_oneof <source_list> <allowed_values_list>

Brief
    Checks for each element in a source list if it is present in a list of
    allowed values. Returns a new list of 0s (not found) and 1s (found).

Inputs
    • source_list ……… The DynList to check elements from.
    • allowed_values_list … A DynList containing the set of allowed values.
    • Constraints ……… Both lists must be of a compatible type (e.g., both
                        numeric or both string).

Returns
    • A new integer DynList of the same length as the source list, containing
      1s for matching elements and 0s for non-matching ones.

Errors
    • TCL_ERROR if either list is not found.
    • TCL_ERROR if the lists are not of a compatible type.

Example
    # String membership
    set src [dl_slist a b c d a e]
    set allowed [dl_slist a d g]
    dl_tcllist [dl_oneof $src $allowed]
    # → 1 0 0 1 1 0

    # Numeric membership
    set nums_src [dl_ilist 10 20 30 40 10]
    set nums_allowed [dl_ilist 10 30 60]
    dl_tcllist [dl_oneof $nums_src $nums_allowed]
    # → 1 0 1 0 1

See also
    dl_find, dl_eqIndex

=== dl_ones (Creation) ===
Synopsis
    dl_ones <count>

Brief
    Creates a new integer list of a given `count`, with all elements
    initialized to 1.

Inputs
    • count … The number of elements in the new list.

Returns
    • A new integer DynList.

Errors
    • TCL_ERROR if `count` is not a non-negative integer.
    • Note: The optional `-type` flag mentioned in some documentation is
      not supported. The command only creates integer lists.

Example
    dl_tcllist [dl_ones 5]
    # → 1 1 1 1 1

See also
    dl_zeros, dl_fill, dl_create

=== dl_or (Logical) ===
Synopsis
    dl_or <list_or_val1> <list_or_val2> [<list_or_val3>...]

Brief
    Performs an element-wise logical OR operation on two or more numeric
    lists or numbers. Non-zero values are true (1), zero is false (0).

Inputs
    • Operands … Two or more numeric DynLists or scalar values.
    • Constraints … Inputs must be numeric and broadcastable.

Returns
    • A new integer DynList containing 0s and 1s.

Errors
    • TCL_ERROR if lists have incompatible lengths or are not numeric.

Example
    set l1 [dl_ilist 1 0 1 0]
    set l2 [dl_ilist 1 1 0 0]

    # Two-list OR
    dl_tcllist [dl_or $l1 $l2]
    # → 1 1 1 0

    # Chained OR
    set l3 [dl_ilist 0 0 0 1]
    dl_tcllist [dl_or $l1 $l2 $l3]
    # → 1 1 1 1

See also
    dl_and, dl_not, dl_orIndex

=== dl_orIndex (Logical / Indexing) ===
Synopsis
    dl_orIndex <list_or_val1> <list_or_val2>

Brief
    Performs an element-wise logical OR on two numeric lists/values and
    returns a new list containing the 0-based indices where the result is true.

Inputs
    • list_or_val1 … A numeric DynList or a scalar value.
    • list_or_val2 … A numeric DynList or a scalar value.
    • Constraints …… Inputs must be numeric and broadcastable.

Returns
    • A new integer DynList of 0-based indices where the logical OR is true.

Errors
    • TCL_ERROR if lists have incompatible lengths or are not numeric.
    • WARNING: The documented support for chained comparisons (3+ arguments)
      is incorrect and will cause an `unable to compare` error. Only use
      with two arguments.

Example
    set l1 [dl_ilist 1 0 1 0 0]
    set l2 [dl_ilist 1 1 0 0 0]
    dl_tcllist [dl_orIndex $l1 $l2]
    # → 0 1 2

See also
    dl_or, dl_andIndex, dl_eqIndex

=== dl_rename (List Management) ===
Synopsis
    dl_rename <old_name> <new_name>

Brief
    Renames an existing dynamic list.

Inputs
    • old_name … The current name of an existing DynList.
    • new_name … The new string name for the list.

Returns
    • The `new_name` is returned on success.

Side Effects
    • The list is accessible via the `new_name` and no longer accessible
      via the `old_name`.
    • Any Tcl variables that held the `old_name` will now refer to a
      non-existent list.

Errors
    • TCL_ERROR if the `old_name` does not correspond to an existing list.
    • WARNING: If `new_name` is already in use by another list, this command
      does NOT error. It silently overwrites and deletes the list that
      previously had `new_name`.

Example
    # Create a list, its internal name is stored in the variable 'listA'
    set listA [dl_ilist 1 2 3]

    # Rename the list to have the literal name "my_renamed_list"
    dl_rename $listA my_renamed_list

    # Now, access the list by its new, literal name
    dl_tcllist my_renamed_list
    # → 1 2 3

    # The old Tcl variable $listA is now a stale reference.
    # The following command would fail:
    # dl_exists $listA

See also
    dl_delete, dl_create

=== dl_bmax / dl_bmean / dl_bmin (Reduction / List of Lists) ===

Brief
    [WARNING: These commands do not exist.]
    The documentation mentions `dl_bmax` (maximum of each sublist), `dl_bmean`
    (mean of each sublist), and `dl_bmin` (minimum of each sublist), but
    none of these are valid commands in the `essctrl` environment. The command
    `dl_bmins` exists for finding the minimums.

See also
    dl_bmins, dl_bsums, dl_bstds

=== dl_bshiftcycle / dl_bstd (Manipulation / List of Lists) ===

Brief
    [WARNING: These commands do not exist.]
    The documentation mentions `dl_bshiftcycle` and `dl_bstd` (standard
    deviation of each sublist), but they are not valid commands in the
    `essctrl` environment. `dl_bstds` exists for standard deviations.

See also
    dl_bshift, dl_bstds

=== dl_char (Conversion) ===
Synopsis
    dl_char <source_list>

Brief
    Converts the elements of a source list to character (byte) type,
    returning a new char DynList. Floats are truncated, and non-numeric
    strings are silently converted to 0.

Inputs
    • source_list … The name of a numeric DynList or a Tcl list of numeric
                    values provided as a single string.

Returns
    • Type …………… string (name of a new char DynList)
    • Element type … char
    • Note …………… `dl_tcllist` will display the numeric byte values, not
                    ASCII characters.

Errors & Bugs
    • The optional `[tclvar]` argument documented in the usage message is
      non-functional and is ignored. It simply processes its first argument.
    • BUG: If the source list contains non-numeric strings, the command does
      not fail. Instead, it silently converts these strings to `0`, which
      can lead to unexpected data.

Example
    # Convert an integer list to a char list
    dl_tcllist [dl_char [dl_ilist 65 66 67]]
    # → 65 66 67

    # Convert a Tcl list string (truncates floats)
    dl_tcllist [dl_char {72.2 73.9 74.5}]
    # → 72 73 74

    # Non-numeric strings are silently converted to 0
    set strings [dl_slist "A" "B" "C"]
    dl_tcllist [dl_char $strings]
    # → 0 0 0

See also
    dl_clist, dl_int, dl_float

=== dl_pack (Manipulation / Restructuring) ===
Synopsis
    dl_pack <list_name>

Brief
    Packs each element of a simple list into its own single-element sublist,
    creating a new list of lists. This is the functional inverse of
    `dl_collapse`.

Inputs
    • list_name … The name of a simple (depth 0) DynList to pack.

Returns
    • A new DynList of type `list` and depth `1`.
    • The new list will have the same length as the input list.

Errors
    • TCL_ERROR if the list is not found.
    • TCL_ERROR if the input list is not a simple list (e.g., already a
      list of lists).

Example
    set simple [dl_ilist 10 20 30]
    set packed [dl_pack $simple]

    # The new list has 3 elements, each is a list
    dl_length $packed
    # → 3

    # The sublists each have a length of 1
    dl_tcllist [dl_lengths $packed]
    # → 1 1 1

    # To see the full structure, you'd need to iterate
    # The conceptual structure is: {{10} {20} {30}}

See also
    dl_collapse, dl_unpack, dl_llist

=== dl_parzen (Statistics / Broken) ===
Synopsis
    dl_parzen <data_list> <start> <stop> <pilot_sd> <nsd> <resolution>

Brief
    [WARNING: This command is broken.]
    It is intended to perform Parzen-window kernel density estimation, but
    instead of returning a density curve (a list of numbers), it returns
    only a single, seemingly meaningless, floating-point value.

Inputs
    • data_list … A numeric DynList.
    • start ……… Start of the estimation range.
    • stop ……… End of the estimation range.
    • pilot_sd … Pilot standard deviation.
    • nsd ………… Number of standard deviations.
    • resolution … The desired number of points in the output curve.

Returns
    • A single-element list containing a single float value, regardless of
      the `resolution` parameter.

Example
    # This should return a list of 16 numbers, but returns only one.
    set data [dl_flist 1 2 3 8 9 10]
    set result [dl_parzen $data 0 15 1.0 3 16]
    dl_length $result
    # → 1

See also
    dl_hist, dl_parzenLists (likely also broken)

=== dl_parzenLists (Statistics / Broken) ===

Brief
    [WARNING: This command does not exist.]
    Following the pattern of other related commands, `dl_parzenLists` is
    mentioned in the function list but is not a valid command in the
    `essctrl` environment.

See also
    dl_parzen

=== dl_pickone (Manipulation / Selection) ===
Synopsis
    dl_pickone <list_name>

Brief
    Selects and returns a single random element from a list.

Inputs
    • list_name … The name of a non-empty DynList.

Returns
    • A single value chosen randomly from the elements of the list. The
      type of the returned value matches the element type of the list.

Errors
    • TCL_ERROR if the list is not found.
    • Note: The command fails silently (produces no output and no error) if
      the input list is empty.

Example
    # Returns a random letter from the list
    dl_pickone [dl_slist a b c d e]
    # → (e.g.) c

    # Returns a random number from the list
    dl_pickone [dl_ilist 10 20 30 40 50]
    # → (e.g.) 40

See also
    dl_choose, dl_randchoose, dl_first, dl_last

=== dl_pushTemps / dl_popTemps (Memory Management) ===
Synopsis
    dl_pushTemps
    dl_popTemps

Brief
    Provides a stack-based mechanism to save and restore the state of
    temporary list creation. `dl_pushTemps` saves the current state of the
    temporary list registry. `dl_popTemps` restores the most recently saved
    state, deleting any temporary lists created since the corresponding push.

Inputs
    These commands take no arguments.

Side Effects
    • `dl_pushTemps`: Creates a savepoint for the temporary list registry.
    • `dl_popTemps`: Deletes all temporary lists (names starting with '%')
      created after the last `dl_pushTemps` call.

Errors
    • `dl_popTemps: popped empty templist stack` if `dl_popTemps` is called
      more times than `dl_pushTemps`.

Example
    # Save the current state (e.g., no temporary lists)
    dl_pushTemps

    # Create some temporary lists
    set temp1 [dl_flist 1.1 2.2]
    set temp2 [dl_add $temp1 5]

    # At this point, two temporary lists exist.

    # Restore the saved state, deleting temp1 and temp2
    dl_popTemps

    # Now, temp1 and temp2 no longer exist.
    dl_exists $temp1   ;# → 0

See also
    dl_clean, dl_delete

=== dl_prepend (Manipulation) ===
Synopsis
    dl_prepend <list_name> <value1> [<value2> ...]

Brief
    Adds one or more values to the beginning of a list. This command
    modifies the list in-place. Note that multiple values are prepended
    sequentially, so they will appear in reverse order at the start of the
    list.

Inputs
    • list_name … The name of the DynList to modify.
    • valueN …… One or more values to prepend. They must be compatible with
                 the list's data type.

Side Effects
    • The specified list is modified.

Errors
    • TCL_ERROR if the list is not found.
    • TCL_ERROR: `expected <type> but got "..."` if a value cannot be
      converted to the list's datatype.

Example
    set myLetters [dl_slist c d e]
    dl_prepend $myLetters a b
    # Note the reverse order of "a" and "b"
    dl_tcllist $myLetters
    # → b a c d e

See also
    dl_append, dl_insert, dl_concat

=== dl_randchoose (Creation / Random) ===
Synopsis
    dl_randchoose <m> <n>

Brief
    Creates a new list containing `n` unique random integers chosen from
    the range `[0, m-1]`. This is useful for generating random indices for
    sampling from another list.

Inputs
    • m … An integer defining the exclusive upper bound of the range `[0, m-1]`.
    • n … The number of unique integers to choose from the range.
    • Constraints … `n` must be less than or equal to `m`.

Returns
    • A new integer DynList of length `n`.

Errors
    • TCL_ERROR: `dl_randchoose: n must be <= m` if `n` is greater than `m`.

Example
    # Choose 5 unique random numbers from the range [0, 19]
    dl_tcllist [dl_randchoose 20 5]
    # → (e.g.) 15 3 11 18 1

    # Choose 10 unique random numbers from the range [0, 9] (a shuffle)
    dl_tcllist [dl_randchoose 10 10]
    # → (e.g.) 3 8 0 5 2 9 1 6 7 4

See also
    dl_choose, dl_pickone, dl_shuffle, dl_irand

=== dl_randfill (Creation / Random) ===
Synopsis
    dl_randfill <size>

Brief
    Creates a new integer list containing a random permutation of integers
    from `0` to `size-1`. This is a convenient way to generate a shuffled
    list of indices.

Inputs
    • size … A positive integer specifying the size of the list to create.

Returns
    • A new integer DynList of length `size`.

Errors
    • TCL_ERROR if `size` is not a valid positive integer.

Example
    # Create a shuffled list of indices from 0 to 4
    dl_tcllist [dl_randfill 5]
    # → (e.g.) 3 0 2 4 1

Notes
    • This command is functionally equivalent to `dl_randchoose N N`.

See also
    dl_randchoose, dl_shuffle

=== dl_rank (Counting / Obscure) ===
Synopsis
    dl_rank <list_name>

Brief
    [WARNING: This command is severely misnamed.]
    It does NOT compute the rank of elements in the traditional sense. Instead,
    it returns a list indicating the occurrence count of each element. For
    each item in the input list, the corresponding output value is its
    0-indexed "seen" count.

Inputs
    • list_name … The name of a DynList. Can be numeric or string type.

Returns
    • A new integer list of the same length as the input.
    • For each element, the value is 0 if it's the first time that element
      has been seen, 1 if it's the second time, 2 for the third, and so on.

Example
    # Count occurrences of each number
    set my_list [dl_ilist 10 20 10 30 20 10]
    dl_tcllist [dl_rank $my_list]
    # → 0 0 1 0 1 2

    # The first 10 is the 0th occurrence. The first 20 is the 0th.
    # The second 10 is the 1st occurrence. The first 30 is the 0th.
    # The second 20 is the 1st occurrence. The third 10 is the 2nd.

    # Works with strings as well
    set my_strings [dl_slist a b a c a b]
    dl_tcllist [dl_rank $my_strings]
    # → 0 0 1 0 2 1

See also
    dl_unique, dl_countOccurences

=== dl_recip (Arithmetic) ===
Synopsis
    dl_recip <list>

Brief
    Computes the element-wise reciprocal (1/x) of a numeric list.

    [WARNING: This command has a critical bug.]

Inputs
    • list … A numeric DynList.

Returns
    • A new float DynList containing the reciprocals.

Errors & Bugs
    • TCL_ERROR if the list is not numeric.
    • BUG: For any element with a value of 0, the command incorrectly
      returns `0.0` instead of `Inf` (infinity) or producing an error.
      This can lead to silent incorrect results in subsequent calculations.

Example
    # Correctly calculates reciprocals for non-zero numbers
    set nums [dl_flist 1 2 4 -5]
    dl_tcllist [dl_recip $nums]
    # → 1.0 0.5 0.25 -0.2

    # Incorrectly handles division by zero
    set nums_with_zero [dl_ilist 2 1 0 -1 -2]
    dl_tcllist [dl_recip $nums_with_zero]
    # → 0.5 1.0 0.0 -1.0 -0.5  (The '0.0' is wrong)

See also
    dl_div, dl_mult

=== dl_regexp (Searching / Regular Expressions) ===
Synopsis
    dl_regexp <list_name> <regex_pattern>

Brief
    Applies a regular expression to each element of a string list. For each
    element, it returns a sublist containing the full match and any captured
    substrings.

Inputs
    • list_name …… The name of a DynList of strings.
    • regex_pattern … A Tcl-style regular expression.

Returns
    • A new list of lists. For each element in the input list:
      - If the pattern does not match, the corresponding sublist is empty.
      - If the pattern matches, the corresponding sublist contains:
        1. The entire matched string.
        2. The string from the first capturing group (if any).
        3. The string from the second capturing group (if any), etc.

Example
    set my_strings [dl_slist "cat" "car" "dog" "cathy"]
    set pattern {^ca(t)}

    # dl_regexp returns a list of lists.
    set results [dl_regexp $my_strings $pattern]

    # Use dl_tcllist to see the structure:
    dl_tcllist $results
    # → {{cat t}} {} {} {{cat t}}

    # The result for "cat" is {cat t} because the full match is "cat" and
    # the first capture group (t) matched "t".
    # The result for "car" and "dog" is {} because they did not match.
    # The result for "cathy" is also {cat t} because the regex only matches
    # the "cat" part.

Notes
    • This command is for extracting matched data. To simply check if a
      pattern matches (i.e., get a true/false result), use `dl_regmatch`.

See also
    dl_regmatch, dl_stringmatch, dl_findPatterns

=== dl_regmatch (Searching / Regular Expressions) ===
Synopsis
    dl_regmatch <list_name> <regex_pattern>

Brief
    Performs an element-wise regular expression match on a string list and
    returns a new list of booleans (0s and 1s) indicating if a match was
    found for each element.

Inputs
    • list_name …… The name of a DynList of strings.
    • regex_pattern … A Tcl-style regular expression.

Returns
    • A new integer DynList of the same length as the input.
    • Each element is `1` if the pattern matched the corresponding string,
      and `0` otherwise.

Example
    set my_strings [dl_slist "cat" "car" "dog" "cathy"]
    set pattern {^ca(t|r)}

    dl_tcllist [dl_regmatch $my_strings $pattern]
    # → 1 1 0 1

Notes
    • This command is for checking for matches. To extract the matched
      substrings and captured groups, use `dl_regexp`.

See also
    dl_regexp, dl_stringmatch, dl_eq

=== dl_replicate (Manipulation / Repetition) ===
Synopsis
    dl_replicate <list_name> <n>

Brief
    Creates a new list by repeating (concatenating) an entire list `n` times.

Inputs
    • list_name … The name of the DynList to replicate.
    • n …………… A positive integer number of times to replicate.

Returns
    • A new DynList containing the replicated items.

Errors
    • TCL_ERROR if `n` is not a valid positive integer.

Example
    # Replicate the list {a b} three times
    dl_tcllist [dl_replicate [dl_slist a b] 3]
    # → a b a b a b

See also
    dl_repeat, dl_fill, dl_cycle

=== dl_repeat / dl_repeatElements (Manipulation / Repetition) ===
Synopsis
    dl_repeat <list_name> <n>
    dl_repeatElements <list_name> <n>

Brief
    Creates a new list by repeating each element of a source list `n` times.
    `dl_repeat` and `dl_repeatElements` are aliases for the same functionality.

Inputs
    • list_name … The name of the DynList to use as a source.
    • n …………… A positive integer number of times to repeat each element.

Returns
    • A new DynList containing the repeated elements.

Errors
    • TCL_ERROR if `n` is not a valid positive integer.

Example
    # Repeat each element of {a b} three times
    dl_tcllist [dl_repeat [dl_slist a b] 3]
    # → a a a b b b

See also
    dl_replicate, dl_fill

=== dl_replace (Manipulation) ===
Synopsis
    dl_replace <source_list> <mask_list> <replacement_list>

Brief
    Creates a new list by replacing elements from a source list based on a
    boolean mask. For each element, if the mask is true (1), the element is
    taken from the replacement list; otherwise, it's taken from the source list.

    [WARNING: This command has unusual requirements for its arguments.]

Inputs
    • source_list …… The original DynList.
    • mask_list ……… A DynList of 0s and 1s, the same length as `source_list`.
    • replacement_list … A DynList of replacement values, which MUST be the
                       same length as `source_list`.

Returns
    • A new DynList with the replaced elements.

Errors
    • TCL_ERROR: `unable to replace elements...` if `source_list`,
      `mask_list`, and `replacement_list` do not all have the same length.

Example
    # Let's replace 'b' and 'd' in the source list.
    set source [dl_slist a b c d e]

    # The mask must have a '1' at the indices for 'b' and 'd'.
    set mask [dl_ilist 0 1 0 1 0]

    # The replacement list must be the same size as the source.
    # The values at non-masked indices ('A', 'C', 'E') are ignored.
    set replacements [dl_slist A X C Y E]

    # The new list takes 'a' from source, 'X' from replacements, 'c' from
    # source, 'Y' from replacements, and 'e' from source.
    dl_tcllist [dl_replace $source $mask $replacements]
    # → a X c Y e

See also
    dl_replaceByIndex, dl_put

=== dl_replaceByIndex (Manipulation) ===
Synopsis
    dl_replaceByIndex <source_list> <index_list> <replacement_list>

Brief
    Creates a new list by replacing elements in a source list at specified
    indices with new values.

Inputs
    • source_list …… The original DynList.
    • index_list ……… A DynList of 0-based integer indices where replacements
                     should occur.
    • replacement_list … A DynList of new values. The length of this list
                       must be equal to the length of the `index_list`.

Returns
    • A new DynList with the elements at the specified indices replaced.

Errors
    • TCL_ERROR if the length of `index_list` does not match the length of
      `replacement_list`.
    • TCL_ERROR if any index in `index_list` is out of bounds for the
      `source_list`.

Example
    set source [dl_slist a b c d e]

    # Replace the elements at index 1 ('b') and 4 ('e')
    set indices [dl_ilist 1 4]
    set new_vals [dl_slist X Y]

    dl_tcllist [dl_replaceByIndex $source $indices $new_vals]
    # → a X c d Y

See also
    dl_replace, dl_put, dl_insert

=== dl_reset (Manipulation) ===
Synopsis
    dl_reset <list_name1> [<list_name2> ...]

Brief
    Resets one or more dynamic lists, clearing all their elements and
    setting their length to 0. This command modifies the list(s) in-place.

Inputs
    • list_name(s) … One or more names of existing DynLists to clear.

Side Effects
    • The specified list(s) are emptied.

Errors
    • TCL_ERROR if any list name is not found.

Example
    set my_list [dl_ilist 1 2 3 4 5]
    dl_length $my_list
    # → 5

    dl_reset $my_list
    dl_length $my_list
    # → 0

See also
    dl_delete, dl_clean

=== dl_return (Scoping / Procedures) ===
Synopsis
    dl_return <list_name>

Brief
    Used inside a Tcl `proc` to correctly return a dynamic list. It
    converts a temporary or local list into a special "return list" that
    persists after the procedure has finished executing.

Inputs
    • list_name … The name of a DynList to be returned from a `proc`.

Returns
    • The name of a new "return list" (e.g., `>0<`). This new list is a
      copy of the input list.

Side Effects
    • Using `return [dl_return $myList]` is the only reliable way to get a
      list out of a Tcl `proc`. Simply using `return $myList` will result
      in a dangling pointer, as the temporary list will be deleted when the
      `proc` exits.

Example
    # A proc that creates a list and correctly returns it.
    proc create_and_return {} {
        # Create a temporary list inside the proc
        set temp [dl_ilist 1 2 3]

        # Use dl_return to ensure it survives after the proc exits
        return [dl_return $temp]
    }

    # Call the proc and capture the name of the returned list
    set my_list [create_and_return]

    # The list is valid and can be used
    dl_tcllist $my_list
    # → 1 2 3

See also
    dl_local, dl_cleanReturns

=== dl_round (Arithmetic) ===
Synopsis
    dl_round <list>

Brief
    Rounds the elements of a numeric list to the nearest integer. It uses a
    "round half away from zero" rule (e.g., 2.5 becomes 3, and -2.5 becomes -3).

Inputs
    • list … A numeric DynList.

Returns
    • A new integer (`long`) DynList containing the rounded values.

Errors
    • TCL_ERROR if the list is not found or is not numeric.

Example
    set values [dl_flist 1.2 1.8 2.5 3.5 -2.5 -2.8]
    dl_tcllist [dl_round $values]
    # → 1 2 3 4 -3 -3

See also
    dl_floor, dl_ceil, dl_int

=== dl_scanInt (Conversion / Parsing) ===
Synopsis
    dl_scanInt <string_list>

Brief
    Parses each string in a source list, attempting to convert it into a
    decimal integer. The parsing is lenient and stops at the first
    non-numeric character.

Inputs
    • string_list … A DynList of strings.

Returns
    • A new integer (`long`) DynList.
    • Parsing stops at the first invalid character (e.g., "25px" becomes 25).
    • A string with no valid leading integer (e.g., "a25") becomes 0.

Example
    set strings [dl_slist "10" "20px" "-5.5" "bad_string"]
    dl_tcllist [dl_scanInt $strings]
    # → 10 20 -5 0

See also
    dl_scanFloat, dl_scanHex, dl_int

=== dl_scanFloat (Conversion / Parsing) ===
Synopsis
    dl_scanFloat <string_list>

Brief
    Parses each string in a source list, attempting to convert it into a
    floating-point number. The parsing is lenient and stops at the first
    invalid character.

Inputs
    • string_list … A DynList of strings.

Returns
    • A new `float` DynList.
    • Parsing stops at the first invalid character (e.g., "3.14p" becomes 3.14).
    • A string with no valid leading float (e.g., "a3.14") becomes 0.0.

Example
    set strings [dl_slist "10" "2.5e-2xyz" "-5.5" "bad_string"]
    dl_tcllist [dl_scanFloat $strings]
    # → 10.0 0.025 -5.5 0.0

See also
    dl_scanInt, dl_scanHex, dl_float

=== dl_scanHex (Conversion / Parsing) ===
Synopsis
    dl_scanHex <string_list>

Brief
    Parses each string in a source list, attempting to convert it from a
    hexadecimal representation into an integer. The parsing is lenient.

Inputs
    • string_list … A DynList of strings.

Returns
    • A new integer (`long`) DynList.
    • The `0x` prefix is optional.
    • Parsing stops at the first invalid hex character (0-9, a-f).

Example
    set strings [dl_slist "10" "0xFF" "A5g" "z" "cafe"]
    dl_tcllist [dl_scanHex $strings]
    # → 16 255 165 0 51966

See also
    dl_scanInt, dl_scanOctal, dl_scanBinary

=== dl_scanOctal (Conversion / Parsing) ===
Synopsis
    dl_scanOctal <string_list>

Brief
    Parses each string in a source list, attempting to convert it from an
    octal representation into an integer. The parsing is lenient.

Inputs
    • string_list … A DynList of strings.

Returns
    • A new integer (`long`) DynList.
    • Parsing stops at the first invalid octal character (0-7).

Example
    set strings [dl_slist "10" "077" "101q" "0" "8"]
    dl_tcllist [dl_scanOctal $strings]
    # → 8 63 65 0 0

See also
    dl_scanInt, dl_scanHex, dl_scanBinary

=== dl_scanBinary (Conversion / Parsing) ===
Synopsis
    dl_scanBinary <string_list>

Brief
    Parses each string in a source list, attempting to convert it from a
    binary representation into an integer. The parsing is lenient.

Inputs
    • string_list … A DynList of strings.

Returns
    • A new integer (`long`) DynList.
    • Parsing stops at the first invalid binary character (0-1).

Example
    set strings [dl_slist "10" "1111" "1012" "0" "2"]
    dl_tcllist [dl_scanBinary $strings]
    # → 2 15 5 0 0

See also
    dl_scanInt, dl_scanHex, dl_scanOctal

=== dl_sdf (Statistics / Broken) ===
Synopsis
    dl_sdf <spike_times> <start> <stop> <kernel_sd> <kernel_nsds> <resolution>

Brief
    [WARNING: This command is broken.]
    It is intended to calculate a Spike Density Function from a list of
    event timestamps. However, like `dl_parzen`, instead of returning a
    density curve (a list of numbers), it returns only a single, seemingly
    meaningless, floating-point value.

Inputs
    • spike_times … A numeric DynList of event timestamps.
    • start ……… Start of the estimation range.
    • stop ……… End of the estimation range.
    • kernel_sd … Standard deviation of the Gaussian kernel.
    • kernel_nsds … Width of the kernel in number of standard deviations.
    • resolution … The desired number of points in the output curve.

Returns
    • A single-element list containing a single float value, regardless of
      the `resolution` parameter.

Example
    # This should return a list of 16 numbers, but returns only one.
    set spikes [dl_flist 1.1 2.3 2.8 5.4]
    set result [dl_sdf $spikes 0 10 0.5 3 16]
    dl_length $result
    # → 1

See also
    dl_parzen (also broken), dl_hist

=== dl_sdf, dl_sdfLists, dl_sdfListsRecursive (Broken) ===
Synopsis
    dl_sdf <spike_times> <start> <end> <width> [<npts>]
    dl_sdfLists <list_of_spike_times_lists> ...
    dl_sdfListsRecursive <list_of_spike_times_lists> ...

Brief
    **Non-functional.** These commands are intended to compute a Spike Density Function (SDF) but appear to be broken.

Details
    When executed, these commands return a single, seemingly meaningless floating-point number instead of the expected list of numbers representing a density curve. The issue persists even with a large and dense list of input spike times. They do not compute a density function as documented or expected.

Example
    # Returns a single float, not a density curve.
    essctrl -c "dl_tcllist [dl_sdf [dl_flist 0.1 0.5 1.2 2.5 3.8 4.2] 0 5 0.2 100]"

=== dl_gte (Logical) ===
Synopsis
    dl_gte <list1> <list2>

Brief
    Performs an element-wise "greater than or equal to" comparison between two lists.

Details
    Returns a new binary list where each element is 1 if the corresponding element in `list1` is greater than or equal to the element in `list2`, and 0 otherwise. The input lists must be of the same length.

Inputs
    • Type …… DynList
    • Length … 2
    • Element types … numeric

Returns
    • Type …………… DynList (new object)
    • Element type … long

Errors
    • TCL_ERROR if the lists have different lengths.

Example
    essctrl -c "dl_tcllist [dl_gte [dl_ilist 1 2 3] [dl_ilist 0 2 4]]"
    # → 1 1 0

See also
    dl_gt, dl_lt, dl_lte, dl_eq, dl_noteq
=== dl_gteIndex (Logical) ===
Synopsis
    dl_gteIndex <list1> <list2>

Brief
    Returns the indices of elements where the condition `list1 >= list2` is true.

Details
    Compares two lists element-wise and returns a new list containing the indices where the elements of `list1` are greater than or equal to the corresponding elements in `list2`. The input lists must be of the same length.

Inputs
    • Type …… DynList
    • Length … 2
    • Element types … numeric

Returns
    • Type …………… DynList (new object)
    • Element type … long

Errors
    • TCL_ERROR if the lists have different lengths.

Example
    essctrl -c "dl_tcllist [dl_gteIndex [dl_ilist 1 2 3 4] [dl_ilist 0 2 2 5]]"
    # → 0 1 2

See also
    dl_gt, dl_lt, dl_lte, dl_eq, dl_noteq, dl_gte 

=== dl_pow (Arithmetic) ===
Synopsis
    dl_pow <base_list> <exponent_list>

Brief
    Computes element-wise exponentiation.

Details
    Raises each element of the `base_list` to the power of the corresponding element in the `exponent_list`. If either argument is a single number instead of a list, it is broadcast across the other list. The two lists must be of the same length if they are both lists. While the command usage suggests more than two lists are possible, testing reveals that any list beyond the second is ignored.

Inputs
    • Type …… DynList_or_Number
    • Length … 2
    • Element types … numeric

Returns
    • Type …………… DynList (new object)
    • Element type … float

Errors
    • TCL_ERROR "unable to combine" if lists have mismatched lengths or contain non-numeric data.

Example (List ^ Scalar)
    essctrl -c "dl_tcllist [dl_pow [dl_flist 2 3 4] 2]"
    # → 4.0 9.0 16.0

Example (List ^ List)
    essctrl -c "dl_tcllist [dl_pow [dl_ilist 2 3 4] [dl_ilist 3 2 1]]"
    # → 8 9 4

Example (Scalar ^ List)
    essctrl -c "dl_tcllist [dl_pow 2 [dl_ilist 1 2 3]]"
    # → 2 4 8

See also
    dl_log, dl_sqrt

=== dl_prod (Arithmetic) ===
Synopsis
    dl_prod <list>

Brief
    Computes the product of all elements in a numeric list.

Details
    Multiplies all elements of the input list together and returns a single numeric value.

Inputs
    • Type …… DynList
    • Length … 1
    • Element types … numeric

Returns
    • Type …………… Number (single value)
    • Element type … Matches the most precise element type in the list (e.g., float if any floats are present).

Special Cases
    • If the input list is empty, the command returns nothing.
    • If the input list contains non-numeric values, the command returns nothing. This is a silent failure.

Example (Integers)
    essctrl -c "dl_prod [dl_ilist 1 2 3 4]"
    # → 24

Example (Floats)
    essctrl -c "dl_prod [dl_flist 1.5 2.0 -3.0]"
    # → -9.0

Example (With Zero)
    essctrl -c "dl_prod [dl_ilist 5 10 0 20]"
    # → 0

See also
    dl_cumprod, dl_sum

=== dl_put (List Modification) ===
Synopsis
    dl_put <list> <index> <new_value>

Brief
    Modifies a list in-place by replacing an element at a specific index.

Details
    Replaces the value at the given zero-based `index` in the input `list` with `new_value`. This is an in-place operation, meaning the original list variable is modified directly. The command also returns a reference to the modified list.

    The data type of `new_value` must match the data type of the list. For example, you cannot `put` a float into an integer list.

Inputs
    • `list` …… The DynList to modify.
    • `index` … The zero-based integer index of the element to replace.
    • `new_value` … The new value to place at the index. Must match the list's type.

Returns
    • Type …… DynList (the same list that was input, now modified)

Errors
    • `TCL_ERROR: index out of range` if the index is negative or greater than or equal to the list length.
    • `TCL_ERROR: expected <type> but got "<value>"` if `new_value` cannot be converted to the list's data type.

Example (Successful Replacement)
    # Note: We set a variable and check its value after the operation.
    essctrl -c "set mylist [dl_ilist 10 20 30]; dl_put \$mylist 1 99; dl_tcllist \$mylist"
    # → 10 99 30

Example (Type Mismatch Error)
    essctrl -c "set mylist [dl_ilist 10 20 30]; dl_put \$mylist 0 99.5"
    # → expected integer but got "99.5"

See also
    dl_replace, dl_insert, dl_set

=== dl_scan (Conversion / Parsing) ===
Synopsis
    dl_scan [-flag] <string_list>

Brief
    Parses a list of strings into a numeric list of a specified type.

Details
    Converts each string in the input `string_list` into a number. The conversion format is determined by an optional flag. If no flag is provided, it defaults to decimal (integer) conversion. This command is the general form of the more specific `dl_scanInt`, `dl_scanFloat`, etc., commands.

Inputs
    • `string_list` … A DynList with the `string` datatype.

Flags
    • `-d` … Decimal (integer). This is the default.
    • `-f` … Float.
    • `-x` … Hexadecimal.
    • `-o` … Octal.
    • `-b` … Binary.

Returns
    • Type …………… DynList (new object)
    • Element type … long or float, depending on the flag used.

Errors
    • `TCL_ERROR: error scanning strings` if any string in the list cannot be parsed according to the specified format.

Example (Default to Integer)
    essctrl -c "dl_tcllist [dl_scan [dl_slist 123 -45 6.7]]"
    # → 123 -45 6  (Note the truncation of 6.7)

Example (Using -f for Floats)
    essctrl -c "dl_tcllist [dl_scan -f [dl_slist 1.2 -3.4 0]]"
    # → 1.2 -3.4 0.0

Example (Using -x for Hex)
    essctrl -c "dl_tcllist [dl_scan -x [dl_slist 10 ff a0]]"
    # → 16 255 160

See also
    dl_scanInt, dl_scanFloat, dl_scanHex

=== dl_select (Manipulation / Selection) ===
Synopsis
    dl_select <source_list> <binary_mask_list>

Brief
    Selects elements from a source list using a binary (0/1) mask.

Details
    Creates a new list containing elements from the `source_list` at positions where the `binary_mask_list` contains a `1`. Both lists must have the same length for predictable behavior.

Inputs
    • `source_list` …… The DynList to select elements from.
    • `binary_mask_list` … An integer DynList of the same length, containing only 0s and 1s.

Returns
    • A new DynList containing only the selected elements.

Errors
    • `TCL_ERROR: unable to select elements...` if the mask list contains values other than 0 or 1.
    • **WARNING:** The command exhibits buggy and unpredictable behavior if the source list and mask list do not have the same length. It may not produce a TCL_ERROR, but the results will be incorrect. Always ensure lists are the same size.

Example (Correct Usage)
    set items [dl_slist a b c d e]
    set mask [dl_ilist 1 0 1 0 1]
    dl_tcllist [dl_select $items $mask]
    # → a c e

Example (Incorrect Usage - Mismatched Length)
    # The mask is shorter than the source list.
    # The output is unpredictable and should not be relied upon.
    dl_tcllist [dl_select [dl_slist a b c] [dl_ilist 1 0]]
    # → a c (Incorrect and buggy output)

See also
    dl_choose, dl_get, dl_eqIndex

=== dl_series (Creation / Sequence Generation) ===
Synopsis
    dl_series <start> <stop> [step]

Brief
    Creates a new list of numbers from `start` to `stop` (inclusive).

Details
    Generates a sequence of numbers starting from `start`, incrementing by
    `step`, up to and including `stop`. This differs from `dl_fromto`, which has an exclusive stop value.

Inputs
    • start … The starting numeric value.
    • stop …… The inclusive ending numeric value.
    • step …… Optional numeric increment (default is 1 or -1 depending
              on the direction from start to stop).

Returns
    • A new numeric DynList. The type is float if any argument is a float.

Errors
    • `TCL_ERROR: unable to create series` if the `step` is 0 or has the wrong sign.

Example
    # Integer sequence, stop is included
    essctrl -c "dl_tcllist [dl_series 1 5]"
    # → 1 2 3 4 5

    # Float sequence with a step
    essctrl -c "dl_tcllist [dl_series 0 10 2.5]"
    # → 0.0 2.5 5.0 7.5 10.0

    # Negative step
    essctrl -c "dl_tcllist [dl_series 5 1]"
    # → 5 4 3 2 1

See also
    dl_fromto, dl_ilist, dl_flist

=== dl_set (Memory Management / Naming) ===
Synopsis
    dl_set <new_name> <list_to_rename>

Brief
    Assigns a permanent name to a dynamic list, effectively renaming it.

Details
    This command renames the `<list_to_rename>` to the string provided as `<new_name>`. The original name of the list (e.g., a temporary name like `%list0`) is deleted.

    **WARNING:** If a list with `<new_name>` already exists, it will be silently deleted and replaced. This can lead to data loss if not used carefully.

Inputs
    • `new_name` ………… A string for the list's new, permanent name.
    • `list_to_rename` … The name of an existing DynList.

Returns
    • The new name of the list.

Side Effects
    • The source list is renamed. Its old name becomes invalid.
    • If the target name already exists, the list at that name is deleted.

Example (Renaming a temporary list)
    # Create a temporary list, give it a permanent name, and check that it exists
    essctrl -c "dl_set my_list [dl_ilist 1 2 3]; dl_tcllist my_list"
    # → 1 2 3

Example (Overwriting an existing list)
    essctrl -c "set first [dl_ilist 10 20]; set second [dl_slist a b c]; dl_set \$first \$second; dl_tcllist \$first"
    # The original integer list at 'first' is destroyed.
    # → a b c

See also
    dl_create, dl_delete, dl_local

=== dl_setFormat (Introspection / Formatting) ===
Synopsis
    dl_setFormat <datatype> <format_string>

Brief
    Sets the default print format for a specified data type.

Details
    This command globally changes how DynLists of a given `datatype` are displayed by commands like `dl_dump` or when returned from the `essctrl` prompt. The format is specified using a C-style format string (e.g., `%d`, `%f`, `%s`). This setting persists for the lifetime of the `essctrl` session or until it is changed again.

Inputs
    • `datatype` …… A string for the data type to affect. Common types are `long`, `float`, `string`.
    • `format_string` … A string that specifies the print format, such as `"%d"` for integers or `"%8.3f"` for floats.

Returns
    • This command does not return a value.

Side Effects
    • The default print format for the specified data type is changed globally.

Example (Formatting Integers)
    # Set integer format to be zero-padded to 5 digits
    essctrl -c "dl_setFormat long %05d; dl_dump [dl_ilist 1 23 456]"
    # Output:
    # 00001
    # 00023
    # 00456

Example (Formatting Floats)
    # Set float format to show 2 decimal places
    essctrl -c "dl_setFormat float %.2f; dl_dump [dl_flist 1.2345 6.7 8]"
    # Output:
    # 1.23
    # 6.70
    # 8.00

Example (Resetting a Format)
    # Reset float format back to the default
    essctrl -c "dl_setFormat float %f"

See also
    dl_dump, dl_datatype

=== dl_shift (Manipulation / Permutation) ===
Synopsis
    dl_shift <list> [shift_amount]

Brief
    Shifts elements of a list to the left or right. A positive shift moves
    elements to the right, and a negative shift moves them to the left.
    Elements shifted off the ends are lost.

Inputs
    • list …… The DynList to shift.
    • shift_amount … An optional integer specifying the number of positions
                     to shift. Default is 1.

Returns
    • A new DynList with the shifted elements.

Errors
    • TCL_ERROR if the list is not found or if `shift_amount` is not an integer.

Example
    set letters [dl_slist a b c d e]

    # Default shift of 1
    dl_tcllist [dl_shift $letters]
    # → e a b c d

    # Shift of 2
    dl_tcllist [dl_shift $letters 2]
    # → d e a b c

    # Shift of -1
    dl_tcllist [dl_shift $letters -1]
    # → b c d e a

See also
    dl_cycle, dl_permute, dl_reverse

    # Shift of -1
    dl_tcllist [dl_shift $letters -1]
    # → b c d e a

See also
    dl_cycle, dl_permute, dl_reverse

=== dl_shiftcycle (Manipulation / Permutation) ===
Synopsis
    dl_shiftcycle <list> [shift_amount]

Brief
    [WARNING: This command does not exist.]
    This command is listed in some documentation but is not a valid command
    in the `essctrl` environment. Its name suggests it would be an alias for
    either `dl_shift` or `dl_cycle`.

See also
    dl_shift, dl_cycle

=== dl_short (Conversion) ===
Synopsis
    dl_short <source_list>

Brief
    Converts the elements of a source list to `short` (16-bit signed integer)
    type, returning a new `short` DynList.

Details
    This command truncates floats and wraps values that are outside the
    range of a 16-bit signed integer ([-32768, 32767]).

    [WARNING: This command has a critical bug.] If the source list contains
    non-numeric strings, the command does not fail. Instead, it silently
    converts these strings to `0`, which can lead to data corruption.

Inputs
    • `source_list` … A numeric DynList or a Tcl list of numeric values.

Returns
    • A new DynList with the `short` datatype.

Errors & Bugs
    • The optional `[tclvar]` argument is non-functional and is ignored.
    • BUG: Non-numeric strings are silently converted to `0`.

Example (Correct Usage with Wrapping)
    # 32768 wraps around to -32768
    essctrl -c "dl_tcllist [dl_short [dl_ilist 1 32767 32768]]"
    # → 1 32767 -32768

Example (Buggy Behavior)
    # Non-numeric strings are silently converted to 0
    essctrl -c "dl_tcllist [dl_short [dl_slist "hello" "123" "world"]]"
    # → 0 123 0

See also
    dl_int, dl_float, dl_char

=== dl_sign (Arithmetic) ===
Synopsis
    dl_sign <list> | dl_sign <number>

Brief
    Computes the element-wise sign of a numeric list or a single number.
    Returns a new list of 1s, 0s, and -1s.

Inputs
    • Type …………… DynList_or_Number
    • Length ……… 1
    • Element types … numeric

Returns
    • Type …………… DynList (new object)
    • Element type … integer

Errors
    • TCL_ERROR if the input list is not numeric.

Example
    # Get the sign for each element in a list
    set values [dl_flist 10 -5.5 0 -0.0]
    dl_tcllist [dl_sign $values]
    # → 1.0 -1.0 0.0 0.0

    # Get the sign of a single number
    dl_tcllist [dl_sign 5]
    # → 1

    # Get the sign of a negative number
    dl_tcllist [dl_sign -7]
    # → -1

See also
    dl_abs, dl_negate

=== dl_shuffle (Manipulation / Random) ===
Synopsis
    dl_shuffle <list>

Brief
    Creates a new list by randomly shuffling the elements of a source list.

Details
    This command performs a random permutation of the elements in the input
    list and returns a new list with the shuffled elements. The original
    list is not modified.

Inputs
    • `list` … The DynList to shuffle.

Returns
    • A new DynList of the same type and length as the source list, with its
      elements in a random order.

Errors
    • `TCL_ERROR: dynlist "..." not found` if the input list does not exist.

Example
    # The output will be a random permutation of the input.
    essctrl -c "dl_tcllist [dl_shuffle [dl_slist a b c d e]]"
    # → c a e b d (for example)

See also
    dl_permute, dl_sort, dl_reverse, dl_irand

=== dl_sin (Arithmetic / Trigonometric) ===
Synopsis
    dl_sin <list_or_number>

Brief
    Computes the element-wise sine of a list or a single number.

Details
    Calculates the sine for each element in a numeric list. Input values are
    assumed to be in radians. Returns a new list of floats.

Inputs
    • `list_or_number` … A numeric DynList or a single number. The values are
                         treated as angles in radians.

Returns
    • A new float DynList containing the sine of each input value.

Errors
    • `TCL_ERROR: dl_sin: invalid list operand` if the list is not numeric.

Example
    # Angles for 0, PI/2, and PI
    set angles [dl_flist 0 1.5708 3.14159]
    dl_tcllist [dl_sin $angles]
    # → 0.0 1.0 2.5e-6 (approximately)

See also
    dl_cos, dl_tan, dl_asin

=== dl_sinh (Arithmetic / Hyperbolic) ===
Synopsis
    dl_sinh <list_or_number>

Brief
    Computes the element-wise hyperbolic sine of a list or a single number.

Details
    Calculates the hyperbolic sine for each element in a numeric list.
    Returns a new list of floats.

Inputs
    • `list_or_number` … A numeric DynList or a single number.

Returns
    • A new float DynList containing the hyperbolic sine of each input value.

Errors
    • `TCL_ERROR: dl_sinh: invalid list operand` if the list is not numeric.

Example
    set values [dl_flist 0 1 -1 2]
    dl_tcllist [dl_sinh $values]
    # → 0.0 1.1752 -1.1752 3.6268 (approximately)

See also
    dl_cosh, dl_tanh

=== dl_slist (Creation) ===
Synopsis
    dl_slist [value1 value2 ...]

Brief
    Creates a new dynamic list of type `string`.

Details
    This command creates a new string DynList, initializing it with the
    provided arguments. Each argument becomes an element in the list. To
    include spaces within a single element, enclose the argument in quotes.

Inputs
    • `valueN` … Optional. A sequence of initial values for the list.

Returns
    • The name of a new string DynList.
    • If called with no arguments, it creates an empty string list.

Example
    # Create a list of strings
    essctrl -c 'dl_tcllist [dl_slist alpha beta "gamma delta"]'
    # → alpha beta {gamma delta}

    # Create an empty string list
    essctrl -c "dl_length [dl_slist]"
    # → 0

See also
    dl_create, dl_ilist, dl_flist, dl_llist

=== dl_sort (Manipulation / Sorting) ===
Synopsis
    dl_sort <list>

Brief
    Sorts the elements of a simple list. Returns a new list with the
    elements sorted in ascending order. The original list is not modified.

Inputs
    • list … The DynList to sort. It must be a simple (1D) list.

Returns
    • A new DynList of the same type, containing the sorted elements.

Errors
    • `TCL_ERROR: dynlist "..." not found` if the input list does not exist.
    • `TCL_ERROR` if the list contains incompatible types for sorting.

Example
    # Sort a list of integers
    essctrl -c 'dl_tcllist [dl_sort [dl_ilist 3 1 4 2]]'
    # → 1 2 3 4

    # Sort a list of strings
    essctrl -c 'dl_tcllist [dl_sort [dl_slist c a d b]]'
    # → a b c d

See also
    dl_bsort, dl_sortIndices, dl_shuffle, dl_reverse


=== dl_sortIndices (Manipulation / Sorting) ===
Synopsis
    dl_sortIndices <list>

Brief
    Returns a list of indices that would sort the elements of a simple list.
    The original list is not modified.

Inputs
    • list … The DynList to get sort indices for. It must be a simple (1D) list.

Returns
    • A new integer DynList containing the indices that would sort the input list.

Errors
    • `TCL_ERROR: dynlist "..." not found` if the input list does not exist.
    • `TCL_ERROR` if the list contains incompatible types for sorting.

Example
    # Get the indices that would sort an integer list
    # For {30 10 40 20}, the sorted order is 10, 20, 30, 40
    # which are at indices 1, 3, 0, 2 respectively.
    essctrl -c 'dl_tcllist [dl_sortIndices [dl_ilist 30 10 40 20]]'
    # → 1 3 0 2

    # Get the indices that would sort a string list
    essctrl -c 'dl_tcllist [dl_sortIndices [dl_slist c a d b]]'
    # → 1 3 0 2

See also
    dl_sort, dl_bsortIndices, dl_permute, dl_shuffle


=== dl_sortByList (Manipulation / Sorting) ===
Synopsis
    dl_sortByList <list_to_sort> <sort_by_list>

Brief
    Sorts a list based on the values in a second list. Returns a new list
    with the elements of the first list reordered. This command has special
    behavior when the `sort_by_list` contains duplicate values.

Inputs
    • list_to_sort … The DynList whose elements will be reordered.
    • sort_by_list … A DynList containing the values to sort by. Can be
                     numeric or string type.

Returns
    • If `sort_by_list` has unique values, it returns a new simple list
      containing the reordered elements from `list_to_sort`.
    • If `sort_by_list` has duplicate values, it returns a new list of lists,
      where elements from `list_to_sort` are grouped into sublists based
      on the matching duplicate keys. The sort is stable within these groups.

Errors
    • `TCL_ERROR: ...cannot sort empty list...` if the input lists are empty.

Special Cases
    • If the lists have different lengths, the operation is silently
      truncated to the length of the shorter list. This can lead to
      unexpected results and should be avoided.

Example (Standard Sort)
    # Sort a list of strings based on a list of numbers
    set items [dl_slist d a c b]
    set order [dl_ilist 4 1 3 2]
    # The 'order' list sorts to {1 2 3 4}, which corresponds to
    # elements {a b c d} from the 'items' list.
    dl_tcllist [dl_sortByList $items $order]
    # → a b c d

Example (Grouping with Duplicate Keys)
    # When keys are duplicated, the command groups the corresponding items.
    set items [dl_slist a b c d]
    set keys [dl_ilist 2 1 2 1]
    # It groups items for key '1' ({b d}) and items for key '2' ({a c}).
    # The result is a list of lists.
    dl_tcllist [dl_sortByList $items $keys]
    # → {b d} {a c}

See also
    dl_sort, dl_sortIndices, dl_permute


=== dl_spliceAfter (Manipulation / Broken) ===
Synopsis
    dl_spliceAfter <list1> <list2>

Brief
    [WARNING: This command appears to be non-functional.]
    It is intended to splice list elements, but all attempts to use it with
    various list types (simple lists, lists of lists) result in an
    `unable to splice lists` error.

Inputs
    • Unknown. The command consistently fails.

Returns
    • Nothing. The command errors out.

Example
    # This command fails.
    essctrl -c "set l1 [dl_slist a b]; set l2 [dl_slist c d]; dl_spliceAfter \$l1 \$l2"
    # → dl_spliceAfter: unable to splice lists

See also
    dl_spliceBefore, dl_insert, dl_concat


=== dl_spliceBefore (Manipulation / Broken) ===
Synopsis
    dl_spliceBefore <list1> <list2>

Brief
    [WARNING: This command appears to be non-functional.]
    It is intended to splice list elements, but all attempts to use it result
    in an `unable to splice lists` error, similar to its counterpart,
    `dl_spliceAfter`.

Inputs
    • Unknown. The command consistently fails.

Returns
    • Nothing. The command errors out.

Example
    # This command fails.
    essctrl -c "set l1 [dl_slist a b]; set l2 [dl_slist c d]; dl_spliceBefore \$l1 \$l2"
    # → dl_spliceBefore: unable to splice lists

See also
    dl_spliceAfter, dl_insert, dl_concat


=== dl_sqrt (Arithmetic / Transcendental) ===
Synopsis
    dl_sqrt <list> | dl_sqrt <number>

Brief
    Computes the element-wise square root of a numeric list or a single
    number. Returns a new list of floats.

Inputs
    • Type …………… DynList_or_Number
    • Element types … numeric

Returns
    • A new float DynList.
    • For negative inputs, the result is `NaN` (Not a Number).

Errors
    • `TCL_ERROR: dl_sqrt: invalid list operand` if the list is not numeric.

Example
    # Square root of positive numbers
    dl_tcllist [dl_sqrt [dl_ilist 4 9 16]]
    # → 2.0 3.0 4.0

    # Square root of zero and a negative number
    dl_tcllist [dl_sqrt [dl_flist 25 0 -9]]
    # → 5.0 0.0 -NaN

See also
    dl_exp, dl_pow, dl_log


=== dl_std (Reduction / Statistics) ===
Synopsis
    dl_std <list>

Brief
    Calculates the sample standard deviation of the elements in a numeric list.

    [WARNING: This command has a critical bug.] It does not produce an error
    for non-numeric lists. Instead, it appears to convert non-numeric
    elements to `0` internally, leading to silent, incorrect results.

Inputs
    • list … A numeric DynList.

Returns
    • A single float value for the sample standard deviation (n-1 denominator).
    • Returns `0.0` for lists with fewer than two elements, including empty lists.
    • Returns an incorrect value for non-numeric lists instead of erroring.

Errors
    • This command fails to error on non-numeric input.

Example
    # Standard case
    dl_std [dl_ilist 1 2 3 4 5]
    # → 1.581139

    # Edge cases
    dl_std [dl_ilist 10]    ;# → 0.0
    dl_std [dl_ilist]      ;# → 0.0

    # Buggy behavior with non-numeric data
    # Instead of erroring, it treats strings as 0.
    # The std of {0, 0, 0} is 0.
    dl_std [dl_slist a b c]
    # → 0.0

See also
    dl_mean, dl_var, dl_bstds


=== dl_stringmatch (Searching / Broken) ===
Synopsis
    dl_stringmatch <list1> <list2>

Brief
    [WARNING: This command appears to be non-functional.]
    Despite its name, this command does not seem to perform string matching.
    All attempts to use it, whether with glob patterns or for simple
    equality checks, result in an `unable to compare` error.

Inputs
    • Unknown. The command consistently fails.

Returns
    • Nothing. The command errors out.

Example
    # This attempt at glob matching fails.
    essctrl -c 'dl_tcllist [dl_stringmatch [dl_slist apple banana] [dl_slist a* b*]]'
    # → dl_stringmatch: unable to compare ...

See also
    dl_regexp, dl_regmatch, dl_eq

=== dl_sub (Arithmetic) ===
Synopsis
    dl_sub <list_or_val1> <list_or_val2> [<list_or_val3>...]

Brief
    Performs element-wise subtraction. The elements of the second (and any
    subsequent) lists are subtracted from the elements of the first list.

Inputs
    • Operands … Two or more numeric DynLists or scalar values.
    • Constraints … All list inputs must have the same length. If a scalar
                    is provided, it is broadcast to all elements.

Returns
    • A new DynList containing the results of the subtraction.

Errors
    • `TCL_ERROR: dl_sub: unable to combine ...` if lists have different
      lengths or contain non-numeric data.

Example
    # List - List
    dl_tcllist [dl_sub [dl_ilist 10 20 30] [dl_ilist 1 2 3]]
    # → 9 18 27

    # List - Scalar
    dl_tcllist [dl_sub [dl_ilist 10 20 30] 5]
    # → 5 15 25

    # Chained subtraction
    dl_tcllist [dl_sub [dl_ilist 10 20 30] [dl_ilist 1 2 3] [dl_ilist 5 5 5]]
    # → 4 13 22

See also
    dl_add, dl_mult, dl_div, dl_negate

=== dl_sublist (Manipulation / Obscure) ===
Synopsis
    dl_sublist <list>

Brief
    [WARNING: This command's function is unknown.]
    It takes a single list as an argument but its behavior is not clear. For
    various valid list inputs (simple lists, lists of lists), the command
    consistently returns the single value `0` without error. Its intended
    purpose, likely related to creating a sublist or slice, could not be
    determined. It should not be used.

Inputs
    • list … A DynList.

Returns
    • The integer `0`.

Example
    # This command returns 0, not a sublist.
    essctrl -c 'dl_tcllist [dl_sublist [dl_ilist 1 2 3 4 5]]'
    # → 0

See also
    dl_get, dl_choose, dl_subshift

=== dl_subshift (Manipulation / Obscure) ===
Synopsis
    dl_subshift <list> <shift_amount>

Brief
    [WARNING: This command's function is unknown.]
    It takes a list (presumably a list of lists) and a shift amount, but its
    behavior is not clear. It does not appear to shift the top-level
    sublists, and its output is not in a standard, iterable format, making
    it impossible to inspect the results of the operation. It should not be
    used.

Inputs
    • list ……… A DynList, likely a list of lists.
    • shift_amount … An integer shift amount.

Returns
    • An unknown and seemingly unusable value.

Example
    # This command produces a result that cannot be inspected.
    essctrl -c 'set s1 [dl_slist a]; set lol [dl_llist $s1]; dl_subshift $lol 1'

See also
    dl_shift, dl_bshift, dl_cycle

=== dl_sum (Reduction) ===
Synopsis
    dl_sum <list>

Brief
    Computes the sum of all elements in a numeric list.

    [WARNING: This command fails silently.] If the list is empty or contains
    non-numeric data, the command produces no output and no error, which can
    hide problems in scripts.

Inputs
    • list … A numeric DynList.

Returns
    • A single number representing the sum of the elements.
    • Returns nothing if the list is empty or contains non-numeric values.

Example
    # Standard case
    dl_sum [dl_ilist 1 2 3 4 5]
    # → 15

    # Silent failure with an empty list
    dl_sum [dl_ilist]
    # → (no output)

    # Silent failure with non-numeric data
    dl_sum [dl_slist a b c]
    # → (no output)

See also
    dl_prod, dl_mean, dl_cumsum, dl_bsums

=== dl_sumcols_list (Reduction / List of Lists) ===

Brief
    [WARNING: This command does not exist.]
    This command is found in some documentation but is not a valid command
    in the `essctrl` environment.

=== dl_tan (Arithmetic / Trigonometric) ===
Synopsis
    dl_tan <list> | dl_tan <number_in_radians>

Brief
    Computes the element-wise tangent of a list or a single number.
    Input values are assumed to be in radians.

Inputs
    • Type …………… DynList_or_Number
    • Element types … numeric

Returns
    • A new float DynList containing the tangent of each input value.

Errors
    • `TCL_ERROR: dl_tan: invalid list operand` if the list is not numeric.

Example
    # Angles for 0, PI/4, and PI/2 (showing asymptote)
    set angles [dl_flist 0 0.7854 1.5708]
    dl_tcllist [dl_tan $angles]
    # → 0.0 1.0 -276243.8... (approximately)

See also
    dl_sin, dl_cos, dl_atan

=== dl_tanh (Arithmetic / Hyperbolic) ===
Synopsis
    dl_tanh <list_or_number>

Brief
    Computes the element-wise hyperbolic tangent of a list or a single number.

Details
    Calculates the hyperbolic tangent for each element in a numeric list.
    Returns a new list of floats. The output values are always in the
    range [-1.0, 1.0].

Inputs
    • `list_or_number` … A numeric DynList or a single number.

Returns
    • A new float DynList containing the hyperbolic tangent of each input value.

Errors
    • `TCL_ERROR: dl_tanh: invalid list operand` if the list is not numeric.

Example
    set values [dl_flist 0 1 -1 2.5]
    dl_tcllist [dl_tanh $values]
    # → 0.0 0.76159 -0.76159 0.98661

See also
    dl_cosh, dl_sinh

=== dl_tcllist (Introspection / Debugging) ===
Synopsis
    dl_tcllist <list_name>

Brief
    Converts a dynamic list into a human-readable Tcl list string. This is
    the primary command for viewing the contents of a list.

Details
    Returns a string representation of the list's contents. For simple lists,
    this is a space-delimited string of the elements. For lists of lists,
    each sublist is enclosed in curly braces `{}`. This command is essential
    for debugging and is the counterpart to Tcl's `puts` command, which does
    not work in the `essctrl` environment for viewing list values.

Inputs
    • `list_name` … The name of an existing DynList.

Returns
    • A string representing the contents of the list.

Errors
    • `TCL_ERROR: dynlist "..." not found` if the list does not exist.
    • NOTE: If more than one argument is provided, the command appears to
      process only the first argument and silently ignores the rest.

Example (Simple List)
    # View the contents of a simple float list
    essctrl -c "dl_tcllist [dl_flist 1.1 2.2 3.3]"
    # → 1.1 2.2 3.3

Example (List of Lists)
    # View the contents of a nested list
    essctrl -c "set l1 [dl_ilist 1 2]; set l2 [dl_slist a b]; set lol [dl_llist \\$l1 \\$l2]; dl_tcllist \\$lol"
    # → {1 2} {a b}

See also
    dl_dump, dl_length

=== dl_tempname (Creation) ===
Synopsis
    dl_tempname

Brief
    [WARNING: This command does not exist.]
    The documentation indicates this command should generate a unique name
    for a temporary list, but it is not a valid command in the `essctrl`
    environment.

=== dl_transpose (Manipulation / Restructuring) ===
Synopsis
    dl_transpose <list_of_lists_name>

Brief
    Transposes a 2D list (a list of lists), swapping its rows and columns.

Details
    Creates a new list of lists where the first sublist contains the first
    element of each of the original sublists, the second sublist contains
    the second element, and so on.

    If the sublists have different lengths, the command will pad the shorter
    rows with `0` (for numeric lists) or empty strings (for string lists)
    to make the matrix rectangular before transposing.

Inputs
    • `list_of_lists_name` … The name of a DynList that is a list of lists.
    • Constraints …… The list must not be empty. All sublists must be of
                     the same data type (e.g., all integer lists or all
                     string lists).

Returns
    • A new DynList containing the transposed matrix.
    • Returns an empty list if the input list contains only empty sublists.

Errors
    • `TCL_ERROR: dl_transpose: bad operand ...` if the input is not a list
      of lists, if the top-level list is empty, or if its sublists are
      not all of the same data type.

Example (Standard Transpose)
    # Transpose a 2x2 integer matrix
    set l1 [dl_ilist 1 2]
    set l2 [dl_ilist 3 4]
    set lol [dl_llist $l1 $l2]
    # lol is {{1 2} {3 4}}
    dl_tcllist [dl_transpose $lol]
    # → {1 3} {2 4}

Example (Jagged List Transpose)
    # The shorter row is padded with an empty string {}
    set l1 [dl_slist a b c]
    set l2 [dl_slist d e]
    set lol [dl_llist $l1 $l2]
    # lol is {{a b c} {d e}}
    dl_tcllist [dl_transpose $lol]
    # → {a d} {b e} {c {}}

Example (Error on Mixed Sublist Types)
    # This command fails because sublists have different types.
    # set l1 [dl_ilist 1 2]; set l2 [dl_slist a b]
    # set lol [dl_llist $l1 $l2]
    # dl_transpose $lol
    # → dl_transpose: bad operand ...

See also
    dl_pack, dl_collapse, dl_dumpMatrixInCols

=== dl_uchar (Conversion / Broken) ===
Synopsis
    dl_uchar <source_list>

Brief
    [WARNING: This command is broken and misnamed.]
    It is intended to convert a list to `unsigned char` type, but it behaves
    exactly like `dl_char` (signed char), incorrectly handling values over 127.
    It also fails to error on non-numeric input.

Details
    This command attempts to convert a source list to a character (byte)
    list. However, it implements a *signed* character conversion, causing
    values from 128-255 to wrap around to negative numbers.

    Furthermore, it has a critical bug where non-numeric strings in the
    source list do not cause an error. Instead, they are silently converted
    to `0`, which can lead to data corruption.

Inputs
    • `source_list` … A numeric DynList or a Tcl list of numeric values.

Returns
    • A new `char` DynList (not `unsigned char`).

Errors & Bugs
    • **Misnamed:** The command performs a signed, not unsigned, conversion.
    • **Bug:** Non-numeric strings are silently converted to `0`.
    • The optional `[tclvar]` argument is non-functional.

Example (Incorrect Behavior)
    # Values over 127 wrap to negative numbers.
    essctrl -c "dl_tcllist [dl_uchar [dl_ilist 127 128 255]]"
    # → 127 -128 -1

Example (Buggy Behavior)
    # Non-numeric strings are silently converted to 0.
    essctrl -c "dl_tcllist [dl_uchar [dl_slist "A" "123" "B"]]"
    # → 0 123 0

See also
    dl_char, dl_clist, dl_int

=== dl_ufloat (Conversion / Broken) ===
Synopsis
    dl_ufloat <source_list>

Brief
    [WARNING: This command is redundant and buggy.]
    It appears to be an alias for `dl_float` and does not implement any
    unsigned logic. It also fails to error on non-numeric input.

Details
    This command converts a source list to a `float` list. The "u" (for
    unsigned) in the name is misleading, as the command behaves identically
    to `dl_float` and correctly handles negative values.

    It has a critical bug where non-numeric strings in the source list do
    not cause an error. Instead, they are silently converted to `0.0`,
    which can lead to data corruption.

Inputs
    • `source_list` … A numeric DynList or a Tcl list of numeric values.

Returns
    • A new `float` DynList.

Errors & Bugs
    • **Redundant:** The command is functionally identical to `dl_float`.
    • **Bug:** Non-numeric strings are silently converted to `0.0`.
    • The optional `[tclvar]` argument is non-functional.

Example
    # It behaves just like dl_float
    essctrl -c "dl_tcllist [dl_ufloat [dl_flist 1.5 -2.5 0]]"
    # → 1.5 -2.5 0.0

Example (Buggy Behavior)
    # Non-numeric strings are silently converted to 0.0
    essctrl -c 'dl_tcllist [dl_ufloat [dl_slist "A" "123.4" "B"]]'
    # → 0.0 123.4 0.0

See also
    dl_float, dl_int

=== dl_uint (Conversion / Broken) ===
Synopsis
    dl_uint <source_list>

Brief
    [WARNING: This command is redundant, misnamed, and buggy.]
    It is an alias for `dl_int` and does not implement any unsigned logic.
    It also fails to error on non-numeric input.

Details
    This command converts a source list to an `integer` list. The "u" (for
    unsigned) in the name is misleading, as the command is implemented by
    calling the exact same internal function as `dl_int`, and it correctly
    handles negative values.

    Source code analysis confirms that `dl_uint` is a wrapper that directly
    calls the signed integer conversion function.

    It has a critical bug where non-numeric strings in the source list do
    not cause an error. Instead, they are silently converted to `0`, which
    can lead to data corruption.

Inputs
    • `source_list` … A numeric DynList or a Tcl list of numeric values.

Returns
    • A new `long` (integer) DynList.

Errors & Bugs
    • **Redundant:** The command is functionally identical to `dl_int`.
    • **Bug:** Non-numeric strings are silently converted to `0`.
    • The optional `[tclvar]` argument is non-functional.

Example
    # It behaves just like dl_int, truncating floats
    essctrl -c "dl_tcllist [dl_uint [dl_flist 1.5 -2.5 0]]"
    # → 1 -2 0

Example (Buggy Behavior)
    # Non-numeric strings are silently converted to 0
    essctrl -c 'dl_tcllist [dl_uint [dl_slist "A" "123" "B"]]'
    # → 0 123 0

See also
    dl_int, dl_float

=== dl_unique (Manipulation / Set Operations) ===
Synopsis
    dl_unique <list>

Brief
    Returns a new list containing the unique elements of the source list,
    sorted in ascending order.

Details
    This command finds all unique values in a list and returns a new list
    containing those values. A critical side effect is that the returned
    list is always sorted.

Inputs
    • `list` … A DynList to process. Can be numeric or string.

Returns
    • A new, sorted list of the same type, containing only the unique elements.

Errors
    • `TCL_ERROR: dynlist "..." not found` if the input list does not exist.

Example
    # Get the unique, sorted elements from an integer list
    essctrl -c 'dl_tcllist [dl_unique [dl_ilist 10 20 10 30 20 10]]'
    # → 10 20 30

    # Get the unique, sorted elements from a string list
    essctrl -c 'dl_tcllist [dl_unique [dl_slist c a b c a d]]'
    # → a b c d

See also
    dl_sort, dl_rank, dl_countOccurences

=== dl_unpack (Manipulation / Restructuring) ===
Synopsis
    dl_unpack <packed_list_name>

Brief
    Unpacks a list that was previously packed by `dl_pack`, restoring it to
    its original form.

Details
    This command is the counterpart to `dl_pack`. It takes a special packed
    list (which is a single-element list containing a string) and
    reconstructs the original list from that string.

Inputs
    • `packed_list_name` … The name of a DynList previously created by `dl_pack`.

Returns
    • A new DynList identical to the one that was originally packed.

Errors
    • `dl_unpack: bad operand (...)` if the input list was not created by `dl_pack`.

Example
    set mylist [dl_slist a b c]
    # pack the list into a single string
    set packed [dl_pack $mylist]
    # now unpack it to get the original list back
    dl_tcllist [dl_unpack $packed]
    # → a b c

See also
    dl_pack, dl_collapse, dl_toString

=== dl_urand (Creation / Random) ===
Synopsis
    dl_urand <size>

Brief
    Creates a new float list of a specified size containing uniformly
    distributed random numbers between 0.0 and 1.0.

Details
    Generates a list of floating-point numbers. The argument `size`
    determines the length of the new list. If a floating-point number is
    provided for the size, it will be silently truncated to an integer.

Inputs
    • `size` … A non-negative integer specifying the number of elements to create.

Returns
    • A new float DynList of the specified size.

Errors
    • `dl_urand: size must be nonneg` if the size is a negative number.

Example
    # Create a list with 4 random floats
    essctrl -c 'dl_tcllist [dl_urand 4]'
    # → 0.905792 0.126987 0.913376 0.632359 (values will vary)

    # A float size is truncated
    essctrl -c 'dl_tcllist [dl_urand 2.9]'
    # → 0.097542 0.849122 (values will vary)

See also
    dl_zrand, dl_randchoose, dl_randfill, dl_shuffle, dl_create

=== dl_ushort (Conversion / Broken) ===
Synopsis
    dl_ushort <source_list>

Brief
    [WARNING: This command is redundant, misnamed, and buggy.]
    It is intended to convert a list to `unsigned short`, but it behaves
    like `dl_short` (signed short) and fails to error on non-numeric input.

Details
    This command attempts to convert a source list to a short integer list.
    However, it implements a *signed* conversion, causing values greater
    than 32767 to wrap around into negative numbers.

    Source code analysis confirms that `dl_ushort` is mapped to the same
    buggy conversion function as the other `dl_u*` commands. It also has
    the critical bug where non-numeric string elements are silently
    converted to `0` instead of raising an error.

Inputs
    • `source_list` … A DynList to convert.

Returns
    • A new list where elements are converted (incorrectly) to signed shorts.

Errors
    • None. Fails to error on invalid input.

Example (Buggy Behavior)
    # A value that should be valid for an unsigned short wraps around
    essctrl -c 'dl_tcllist [dl_ushort [dl_ilist 40000]]'
    # → -25536

    # Non-numeric strings are silently converted to 0
    essctrl -c 'dl_tcllist [dl_ushort [dl_slist "A" "123" "B"]]'
    # → 0 123 0

See also
    dl_short, dl_int

=== dl_var (Statistics / Broken) ===
Synopsis
    dl_var <list>

Brief
    [WARNING: This command is buggy.]
    Calculates the sample variance of a numeric list. It fails to error on
    non-numeric or empty lists.

Details
    This command computes the sample variance of the elements in a list,
    which is a measure of their dispersion. It returns a single floating-point
    number. It incorrectly returns `0.0` for empty lists and non-numeric
    lists instead of raising an error.

    Note: This command calculates the *sample* variance (denominator N-1), not
    the population variance (denominator N).

Inputs
    • `list` … A numeric DynList.

Returns
    • A single float value representing the sample variance.

Errors
    • None. Fails to error on invalid input.

Example
    # Calculate the sample variance of a simple list
    essctrl -c 'dl_var [dl_ilist 1 2 3 4 5]'
    # → 2.5

Example (Buggy Behavior)
    # An empty list should error but returns 0.0
    essctrl -c 'dl_var [dl_ilist]'
    # → 0.0

    # A list of strings should error but returns 0.0
    essctrl -c 'dl_var [dl_slist a b c]'
    # → 0.0

See also
    dl_std, dl_mean

=== dl_write (I/O / Broken) ===
Synopsis
    dl_write <list> <filename>

Brief
    [WARNING: This command is buggy.]
    Writes the raw, binary contents of a list to a file. It does not work
    correctly with relative paths in the `essctrl` environment.

Details
    This command dumps the in-memory representation of a list's data directly
    to a file. It does not perform any text conversion, so the output is a
    binary file.

    Crucially, it appears to have a bug where it fails silently when using a
    relative path for the filename. An absolute path (e.g., `/tmp/output.bin`)
    must be used for the command to work.

Inputs
    • `list` … The DynList to write to a file.
    • `filename` … The path to the output file. Must be an absolute path.

Returns
    • Nothing.

Errors
    • None. Fails silently on relative paths.

Example (Buggy Behavior)
    # This command will appear to work but no file will be created
    essctrl -c 'dl_write [dl_flist 1.0 2.0] "output.bin"'

Example
    # Writing to an absolute path succeeds
    essctrl -c 'dl_write [dl_ilist 1 2 3 4] "/tmp/output.bin"'
    # (This will create a 16-byte file in /tmp)

See also
    dl_read, dl_dump

=== dl_writeAs (I/O / Broken) ===
Synopsis
    dl_writeAs ushort <list> <filename>

Brief
    [WARNING: This command is broken and misleading.]
    Writes a list to a file, converting its type to `unsigned short`. It is
    hardcoded to only this type and fails on relative paths.

Details
    This command is intended to write a list to a file with a type conversion,
    but it is not implemented correctly. The `type` parameter is hardcoded
    and the command only works if the literal string "ushort" is provided.

    Like `dl_write`, it also fails silently when using a relative path for the
    filename. An absolute path (e.g., `/tmp/output.bin`) must be used.

Inputs
    • `ushort` … The literal string "ushort". No other type is accepted.
    • `list` … The DynList to convert and write.
    • `filename` … The absolute path to the output file.

Returns
    • Nothing.

Errors
    • Returns a usage error if the type is not `ushort`.
    • Fails silently on relative paths.

Example
    # This is the only way the command works
    essctrl -c 'dl_writeAs ushort [dl_ilist 65 66 67] "/tmp/output.bin"'
    # (This will create a 6-byte file in /tmp)

Example (Buggy Behavior)
    # Using any other type fails
    essctrl -c 'dl_writeAs float [dl_ilist 1 2 3] "/tmp/output.bin"'
    # → usage: dl_writeAs ushort dynlist filename

See also
    dl_write, dl_read, dl_dump

=== dl_zeros (Creation) ===
Synopsis
    dl_zeros <size>

Brief
    Creates a new list of a specified size filled with zeros. The data type of
    the output depends on the data type of the `size` argument.

Details
    Generates a list of zeros. An integer `size` produces a list of integer
    zeros (`0`), while a float `size` produces a list of float zeros (`0.0`).
    If a float size is provided, it is silently truncated to an integer to
    determine the list's length.

Inputs
    • `size` … A non-negative number specifying the number of elements.

Returns
    • A new DynList of the specified size, filled with either integer or
      float zeros.

Errors
    • `dl_zeros: size must be nonneg` if the size is a negative number.

Example
    # An integer size creates an integer list
    essctrl -c 'dl_tcllist [dl_zeros 4]'
    # → 0 0 0 0

Example (Type-Switching Behavior)
    # A float size creates a float list and is truncated
    essctrl -c 'dl_tcllist [dl_zeros 2.9]'
    # → 0.0 0.0

See also
    dl_ones, dl_create, dl_fill

=== dl_zrand (Creation / Random) ===
Synopsis
    dl_zrand <size>

Brief
    Creates a new float list of a specified size containing random numbers
    from a standard normal distribution (mean 0, variance 1).

Details
    Generates a list of floating-point numbers sampled from a normal (or
    Gaussian) distribution. The `size` argument determines the length of
    the new list. If a floating-point number is provided for the size, it
    will be silently truncated to an integer.

Inputs
    • `size` … A non-negative integer specifying the number of elements to create.

Returns
    • A new float DynList of the specified size.

Errors
    • `dl_zrand: size must be nonneg` if the size is a negative number.

Example
    # Create a list with 4 normally-distributed random floats
    essctrl -c 'dl_tcllist [dl_zrand 4]'
    # → 0.488214 -0.153483 1.62223 -0.627346 (values will vary)

    # A float size is truncated
    essctrl -c 'dl_tcllist [dl_zrand 2.9]'
    # → -0.342731 0.124592 (values will vary)

See also
    dl_urand, dl_randchoose, dl_randfill, dl_shuffle, dl_create