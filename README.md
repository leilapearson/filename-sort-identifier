# Filename Sort Identifier

A set of directories and empty files that make it easier to identify the algorithm
used for grouping and sorting files and directories. Also makes it easier to verify
that sorting and grouping options are working as expected.

## About comparing and categorizing

A basic string comparison compares strings one character at a time looking for the first
difference. If it reaches the end of the shorter string without finding a difference,
the shorter string is considered less than the longer string - meaning it will sort
first. If both strings are the same length and no differences are found then they
compare as equal. Simple!

So why so many different test cases?

### Differences and similarities

Firstly, there are different degrees of difference and similarity when you are comparing characters:

- Two different letters are more different than the same letter in uppercase and lowercase.
- Two different letters are more different than the same letter with and without an accent.
- Two differently accented letters are more different than same letter with the same accent in lowercase and in uppercase.

A locale comparison takes these concepts into account - treating base character differences as more important than same-letter accent differences and accent differences as important than same-accented-letter case differences. The sorted result behaves accordingly. It also directly takes into account what "alphabetical order" means in each locale.

If you request a numerical locale comparison, it will also take into account what equivalent positive integers look like - treating a number with leading zeros as equal to the same number without leading zeros, and sorting 2 before 10. It doesn't attempt to account for decimal numbers or negative numbers though - treating characters that denote these concepts like separate characters and not part of the number.

You can also specify that you do NOT want to take differences in accent or case or both into account. However, keep in mind that this only affects cases where the base characters
have already all compared to be the same. The value of treating an accented character as
exactly equal to an unaccented character versus slightly different from the same unaccented
character is questionable in most cases - unless you are presorting your list in some way
where considering these characteristics would undo your work.

A simple unicode comparison on the other hand doesn't really account for these subtleties. A word that starts with an accented letter will wind up far apart from a word that starts with the same letter without an accent, for example. Languages that don't use a latin A-Z alphabetical order will also find things look quite out of order. Also numbers with leading zeros will always sort before numbers without, and 2 will sort after 10.

Okay, so that explains part of the complexity. What about the rest?

### Categorizing and grouping

Before you do your fine-grained character by character string comparison, you may want to group or categorize strings so that strings that are in the same category will all wind up together. You can also decide to compare the whole string all at once, or to break it up into component parts.

When it comes to filename sorting, you might want to:

- group files separately from directories - or mix them together
- decide whether to sort with the numeric option on or off
- compare the whole filename all at once, not distinguishing between the name part and extension part
- compare the name first and then the extension so files with the same name but different extensions will sort close together.
- compare the extension first and then the name so files with the same extension but different names will sort close together.
- compare by case first (uppercase, lowercase, or no case), so names with the same case will sort close together.
- treat files whose full filename starts with a dot (hidden files) in a special way with respect to distinguishing the filename from the extension. For example, you might want `.eslint.json` to sort near `.eslintrc` - which won't necessarily happen if you're comparing filenames before extensions and you treat `.eslint.json` as `['.eslint', '.json']`, and `.eslintrc` as `['', '.eslintrc']`.
- do combinations of the above, and do them in different orders

So you can see that with all of these options, you can get a lot of variation - much of which only shows up in very particular and subtle test cases.

Some of these test cases are even impossible to perform on a real filesystem because - for example - most filesystems won't permit you to create two files in the same directory that have exactly the same name but a different case, or two files with the same name exactly but different internal file ids. Those that do (for example, Google Drive) must rely on automatic renaming of conflicting filenames when sharing files with systems that don't allow for these cases. Definitely not what you want to happen when you're sharing source code between systems.

## Goals

This repository aims to make visible which of the above considerations are taken
into account during a filename sort.

## Non-Goals

This repository doesn't try to show all possible pairings of the kinds of differences,
focusing.
