# 7. Structured data and composite types

The power of data comes from the ability to build new data from old by putting together data structures. In OCaml, three general ways to structure data are tuples, lists, and records. For each way, we need to know how to construct structures from their parts using value constructors; what the associated type of the structures is and how to construct a type expression for them using type constructors; and how to decompose the structures into their component parts using pattern-matching.

## Tuples

- **Tuple:** fixed length sequence of elements
    - Tuples can be pairs, triples, quadruples, quintuples, sextuples, septuples...
    - A tuple value is formed using the **value constructor** for tuples
        - A pair containing the integer '3' and the truth value 'true' is given by '3, true' (different from 'true, 3')
    - The type of a tuple is determined by the types of its parts
        - i.e. the tuple '3, true' is of type 'int * bool' (read "int cross bool")
        - i.e. the tuple '1, (2, "three"), is of type 'int * (int * string)'
    
## Pattern matching for decomposing data structures

The 'match' construction allows us to extract parts from the composite structure by performing matching and decomposition.

```
match <expr> with
| <pattern_1> -> <expr_1>
| <pattern_2> -> <expr_2>
...

```
For example, if we want to add two integers within a tuple:

```
# let addpair (pair: int * int) : int = 
#   match pair with
#   | x, y -> x + y ;;
val addpair : int * int -> int = <fun>
# addpair (3, 4) ;;
- : int = 7
```
- In the pattern 'x, y,' the variables 'x' and 'y' are names that can be used for the two components of the pair, as they have been in the expression 'x + y'
    - Any variable names could be used
- Because there is only one value constructor for pairs, only one 'match' is used here
    - 'match' construction allows us to deconstruct a structured datum into component parts by matching against a template that uses the _very same_ value constructor that is used to build such data in the first place

We may want to exact just one value from a pair. To do so, we need to eliminate one of the variables by incorporating an **anonymous variable** using an underscore:
```
# let fst (pair : int * int) : int =
#   match pair with
#   | x, _y -> x;;
val fst : int * int -> int = <fun> 
# fst (3, 4) ;;
- : int = 3
```

We can also perform two matches at once against a pair of points. For example when calculating the distance between two points 'p1, p2':

```
# let distance p1 p2 = 
#   match p1, p2 with
#   | (x1, y1), (x2, y2) -> 
#       sqrt ((x2 -. x1) ** 2. +. (y2 -. y1) ** 2.) ;;
val distance: float * float -> float * float -> float = <fun>
```

There is some syntactic sugar. For example:

```
let <var> = <expr> in
match <var> with
| <pattern_1> -> <expr_1>
```
can be simplified to:

```
let <pattern_1> = <expr> in
<expr_1>
```

With respect to the distance function, this looks like:

```
# let distance p1 p2 = 
#   let (x1, y1), (x2, y2) = p1, p2 in
#   sqrt ((x2 -. x1) ** 2. +. (y2 -. y1) ** 2.) ;;
```

This can be further symplified by pattern matching in the global let construct:

```
# let distance (x1, y1), (x2, y2) = 
#   sqrt ((x2 -. x1) ** 2. +. (y2 -. y1) ** 2.) ;;
val distance : float * float -> float * float -> float = <fun>
```

After adding typings:

```
# let distance (x1, y1 : float * float)
#              (x2, y2: float * float)
#            : float = 
#   sqrt ((x2 -. x1) ** 2. +. (y2 -. y1) ** 2.) ;;
val distance : float * float -> float * float -> float = <fun>
```

## Advanced pattern matching

Patterns can also match atomic types also like 'int' or 'bool':

```
# let int_of_bool (cond : bool) : int = 
#   match cond with
#   | true -> 1
#   | false -> 0 ;;
val int_of_bool : bool -> int = <fun>
```

```
# int_of_bool true ;;
- : = 1
# int_of_bool false ;;
- : = 0
```

For booleans, using a conditional is better, however:

```
# let int_of_bool (cond : bool) : int = 
#   if cond then 1 else 0 ;;
val int_of_bool : bool -> int = <fun>
```

We can also match against integers:

```
# let is_small_int (x : int) : bool =
#   match abs x with
#   | 0 -> true
#   | 1 -> true
#   | 2 -> true
#   | _ -> false ;;
val is_small_int : int -> bool = <fun>
```

In this function, the anonymouse variable _ is used as a **wild-card** pattern that matches any value.

```
# is_small_int ~-1 ;;
- : bool = true
# is_small_int 2 ;;
- : bool = true
# is_small_int 7 ;;
- : bool = false
```

This can be further simplified by using a list of patterns interspersed with vertical bars (|):

```
# let is_small_int (x : int) : bool =
#   match abs x with
#   | 0 | 1 | 2 -> true
#   | _ -> false ;;
val is_small_int : int -> bool = <fun>
```

## Lists







This material is under construction! Although we have written up most of it, you will probably find several typos. If you do, please let us know, or submit a pull request with your fixes via GitHub.

The notes are written in Markdown and are compiled into HTML using Jekyll. Please add your changes directly to the Markdown source code. This repo is configured without any extra Jekyll plugins so it can be compiled directly by GitHub Pages. Thus, any changes to the Markdown files will be automatically reflected in the live website.

To make any changes to this repo, first fork this repo. (Otherwise, if you cloned the `ermongroup/cs228-notes` repo directly onto your local machine instead of forking it first, then you may see an error like `remote: Permission to ermongroup/cs228-notes.git denied to userjanedoe`.) Make the changes you want and push them to your own forked copy of this repo. Finally, go back to the GitHub website to create a pull request to bring your changes into the `ermongroup/cs228-notes` repo.

If you want to test your changes locally before pushing your changes to the `master` branch, you can run Jekyll locally on your own machine. In order to install Jekyll, you can follow the instructions posted on their website (https://jekyllrb.com/docs/installation/). Then, do the following from the root of your cloned version of this repo:
1) Make whatever changes you want to the Markdown `.md` files.
2) `rm -r _site/ \n`  # remove the existing compiled site
3) `jekyll serve`  # this creates a running server
4) Open your web browser to where the server is running and check the changes you made.

### Notes about writing math equations

- Start and end math equations with `$$` **for both inline and display equations**! To make a display equation, put one newline before the starting `$$` a newline after the ending `$$`.

- Avoid vertical bars `|` in any inline math equations (i.e., within a paragraph of text). Otherwise, the GitHub Markdown compiler interprets it as a table cell element (see GitHub Markdown spec [here](https://github.github.com/gfm/)). Instead, use one of `\mid`, `\vert`, `\lvert`, or `\rvert` instead. For double bar lines, write `\|` instead of `||`.
