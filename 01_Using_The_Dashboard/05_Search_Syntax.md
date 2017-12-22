<!-- @@@title:Search Syntax@@@ -->

# Search Syntax

LogZilla provides standard boolean-type search syntax much like you would expect when using Google. The only difference is the ability to append a wildcard (`*`)

* All searches are case *insensitive*
* All searches must contain at least 4 characters at a minimum unless otherwise configured by your administrator.

**Correct search syntax:**

Example 1:

```
hello*
```

**Incorrect search syntax (too few characters)**

```
hel*

```

The 4 character minimum is set in a config at the OS level which administrators can opt to change at the cost of using more memory for indexing. Customers are welcome to contact us for guidance if this is desired.


# Boolean Examples

## Phrase Search

```
"hello world"
```

## Operator AND
The `AND` is automatically implied when separating search words with a space and **should not** be included in your search criteria.

For example, searching on the text `hello world` would return results for both `hello` and `world`.

## Operator NOT
The `!` or `-` operators may be used to find events `NOT` containing the specified text. For example:

```
hello -world
```

Or

```
hello !world
```

## Boolean Mode Wildcard

Many Network and Systems logs will include names such as `GigabitEthernet1/0/0`, etc. The wildcard feature allows users to specify a search term when they may not know the trailing characters. 

For example:
```
gigabitethernet1*
```
Would return results for `GigabitEthernet1/0/0`, `GigabitEthernet1/0/2`, or even `GigabitEthernet100`.


## Invalid Search Syntax
The following examples show some of the mixed-mode searches which are not supported at this time:

* Searches containing both `OR` and `NOT` operator's combined:

```
hello | -world
```

* Mixed Phrase `AND` or `NOT`

```
"hello world" !world2
```

```
"hello world" world
```

* Negative searching without a preceding positive search

```
!hello
```
>This would be analogous to searching Google for every word on the internet that does `NOT` contain the word hello. Which, of course, would not be very useful.
