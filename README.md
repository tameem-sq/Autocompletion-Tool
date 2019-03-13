This project is the product of a collaboration between myself and the brilliant Shafin Siddique.
To run this project, you will need python 3.0 or grater installed.

# Autocompletion-Tool

An autocompletion tool that can be used in multiple domains such as returning suggestions for a string input from a text file or the autocompletion of songs from a .csv file containing codes for various melodies.

For this project we implemented a new data structure, and then applyied it to two different
problem domains, one of which is a text-based autocompletion tool similar to what online search engines
like Google use.

The Autocompleter Assignment made use of two implementations of a "prefix tree" data structure, which allows clients to autocomplete on text or music from .csv input files. 

Autocompleter operations
An Autocompleter supports the following operations:
size: Return how many values are stored by the Autocompleter.
insert: Insert a value into the Autocompleter with an associated weight and prefix sequence.
autocomplete: Return a list of values in the Autocompleter that match a given prefix. The user may
optionally specify a limit on the number of values returned (more on this later).
remove: Remove all values in the Autocompleter that match a given prefix.

It is mainly the autocomplete operation that distinguishes prefix trees from the data structures we have
studied so far. This operation accepts a list as input; we say that a value in the Autocompleter matches
the given list as a prefix if the prefix sequence used to insert the value has the input list as a prefix. (In
Python, we say that “lst1 has lst2 as a prefix” if lst1[0:len(lst2)] == lst2 .)

So unlike the regular Set ADT, which supports searching for a specific value (think __contains__), an
Autocompleter allows the user to search for multiple values at once by specifying a common prefix.

Then if we perform an autocomplete operation with the prefix [1, 2], we expect to get back the lists
[1, 2, 3] and [1, 2, 10].
Note: the empty list is a prefix of every Python list, and so we can perform an autocomplete operation
with the input prefix [] to obtain all values stored in the Autocompleter.
Limiting autocomplete results

For very large datasets, we usually don’t want to see all autocomplete results (think, for example, about
Google’s autocompletion when you start typing a few letters into its search box). Because of this, the
autocomplete operation takes an optional positive integer argument that specifies the maximum number
of values to return. Note that if the number of matches is less than or equal to the limit, all of the matches
are returned.

But when the number of matching values exceeds the given limit, how does the Autocompleter choose
which values to return? We could use a time-based approach (e.g., earliest inserted or most recently
inserted) or rely on some kind of ordering on the values themselves (e.g., alphabetical order). However,
for this assignment we’ll instead use the weight associated with each value to decide which values to
return.

But this doesn’t mean that we’ll always return the matching values with the largest weights!It
turns out that accomplishing this is tricky to do efficiently. As we’ll discuss below, our implementation of
the Autocompleter ADT will take weights into account, but in a more limited way.

The prefix tree data structure

There are many ways you could implement the Autocompleter ADT using what you’ve learned so far in
this course (and earlier). You might, for example, store each value, weight, and prefix sequence in a list
of tuples, and then iterate through the entire list in each autocomplete operation. This approach has the
downside that every single tuple would need to be checked for each autocomplete (or remove)
operation, even if only a few tuples match the given prefix.

To support the required operations in an efficient manner, we will use a tree-based data structure called
a simple prefix tree. Here is the key idea:
Every leaf of a simple prefix tree stores one value (and weight) that was inserted into the tree.
Every internal value stores a “common prefix” of each of its descendant values. That is, it stores a list
that is a prefix of every prefix sequence of each leaf value descended from it.
Each internal value’s list has length one greater less than its parent’s value (except the tree’s root,
which has no parent). The root value is always [].
As a consequence of this last point, the prefixes always grow by 1 element as you go down the tree.
This is a bit abstract, so let’s look an an example.

Example simple prefix tree
Suppose we want to store these strings:
'car', 'cat', 'care', 'dog', 'danger'
We want to autocomplete on these strings by individual letters, and so a prefix sequence for each string
is simply a list of the individual characters of each string. For example, the prefix sequence for 'car' is
['c', 'a', 'r'] .
