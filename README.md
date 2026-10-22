# Inputs and Outputs

Practice working with strings, formatting what you print, and taking values
from the person running your program.

**Practicing:** string methods, slicing, f-strings, `input()`, type conversion

- [AI Use on This Assignment](#ai-use-on-this-assignment)
- [Setup](#setup)
- [Before You Start](#before-you-start)
- [From Scratch](#from-scratch)
  - [Question 1: `format_name`](#question-1-format_name)
  - [Question 2: `starts_with_vowel`](#question-2-starts_with_vowel)
  - [Question 3: `extract_domain`](#question-3-extract_domain)
  - [Question 4: `rotate`](#question-4-rotate)
  - [Question 5: `clean_username`](#question-5-clean_username)
  - [Question 6: `describe_total`](#question-6-describe_total)
  - [Question 7: `censor`](#question-7-censor)
- [Modify](#modify)
  - [Question 8: `order_summary`](#question-8-order_summary)
  - [Question 9: `print_receipt`](#question-9-print_receipt)
- [Debug](#debug)
  - [Question 10: `total_with_tax`](#question-10-total_with_tax)
  - [Question 11: `first_initial`](#question-11-first_initial)
- [Submitting](#submitting)
- [Good luck!](#good-luck)

## AI Use on This Assignment

Use whichever mode matches where you are with this material. Both are fine,
and most people move between them as a concept clicks.

**Tutor mode.** The AI explains, questions, quizzes, and critiques, and you
write every line you submit. For this assignment that means asking it what a
slice does with a negative number, or having it quiz you on string methods
until you stop reaching for the documentation. Ask it a hundred questions —
that is the whole point. What you do not do is ask it for the function. Paste
this at the start of a chat and it will hold for the rest of the conversation:

> You are acting as a tutor. Your job is to explain what this coding question
> is asking, clarify confusing wording, and highlight the relevant concepts I
> need to know — but do not provide the full solution or code that directly
> answers the question. Instead, rephrase the problem in simpler terms,
> identify what is being tested, and suggest what steps or thought processes
> might help. Ask me guiding questions to make sure I am thinking critically.
> Do not write the final function, algorithm, or code implementation.

**Implementer mode.** You write a specification first, the AI writes code from
it, and then you verify that code line by line. For this assignment your spec
has to say what each function does with an empty string and with a value of
the wrong type. If what comes back does more than you asked for, reject it —
over-delivery is a defect, and catching it is part of the job.

You own every line either way, and you will be asked to explain it.

## Setup

Work in `development/mod-1`. Make a draft branch before you start.

```sh
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
git checkout -b draft
```

Run `pytest` for everything, or `pytest -k rotate` for one question. Every push
runs the tests on GitHub and reports your score in the **Actions** tab.

75% of tests passing counts as complete. Submit at that point even if it is
not perfect. Treat submitting as a checkpoint rather than a finish line, and
come back to improve it.

## Before You Start

A string is a sequence, so you can reach into it by position using **bracket
notation**. Counting starts at 0, a negative number counts from the end, and
`len()` tells you how many characters there are.

```python
word = "python"
word[0]      # "p"
word[-1]     # "n"
word[1:4]    # "yth"
word[:3]     # "pyt"
word[9]      # IndexError: string index out of range
```

A **slice** like `word[1:4]` never raises an `IndexError`. It returns whatever
characters are there and stops early if it runs out. A single position like
`word[9]` does raise one, because there is no character to return and Python
will not invent one.

**Strings are immutable**, which means no string ever changes. A **method** is
a function that belongs to a value and is called on it with **dot notation**,
like `name.upper()`. Every string method that looks like it edits a string
actually builds a new one and hands it back:

```python
name = "ada"
name.upper()   # "ADA"
name           # still "ada", because upper() returned a new string
name[0] = "A"  # TypeError: 'str' object does not support item assignment
```

So a method call you do not keep is a method call you wasted.

**`input()` always hands you a string**, even when the person typed digits.
That is why `"1" + 1` raises a `TypeError`: Python will not guess whether you
meant `2` or `"11"`. Convert one side with `int()` or `float()` first.

Finally, `print()` takes more than one value, and two keyword arguments
control the spacing:

```python
print("a", "b", "c")                 # a b c
print("a", "b", "c", sep="-")        # a-b-c
print("loading", end="")             # no newline after it
```

## From Scratch

Write your solutions in `src/from_scratch.py`.

### Question 1: `format_name`

Write a function `format_name` that takes a `first` and `last` name in any mix
of cases and returns them properly capitalized.

```python
format_name("MarY", "JAne")
# "Mary Jane"
format_name("ZO", "SMITH")
# "Zo Smith"
```

There is a [string method](https://www.w3schools.com/python/python_ref_string.asp)
that does exactly this for one word. Can you find it? Can you also write this "by hand"?

### Question 2: `starts_with_vowel`

Write a function `starts_with_vowel` that takes one parameter: a string
`word`. It should return `True` if the word starts with a vowel and `False`
otherwise. Capital letters count as vowels too.

```python
starts_with_vowel("apple")
# True
starts_with_vowel("Iguana")
# True
starts_with_vowel("banana")
# False
starts_with_vowel("")
# False
```

That last case catches people. An empty string starts with nothing, so the
answer is always `False`.

### Question 3: `extract_domain`

Write a function `extract_domain` that takes one parameter: a string `email`.
It should return everything after the `@`, which is the domain.

```python
extract_domain("user@example.com")
# "example.com"
extract_domain("a.b@mail.co.uk")
# "mail.co.uk"
```

Hint: find the index of the `@` symbol and then slice the string. "find and slice" is a common pattern!

### Question 4: `rotate`

Write a function `rotate` that takes a string and a number. It returns the
string with that many characters moved from the end to the front.

```python
rotate("hello", 1)
# "ohell" — the "o" at the end is rotated to the front
rotate("hello", 3)
# "llohe" — the "llo" at the end is rotated to the front
rotate("xyz", 3)
# "xyz" — the whole string is rotated, nothing changes!
rotate("test", 0)
# "test" — nothing is rotated
```

[Slicing](https://www.w3schools.com/python/python_strings_slicing.asp) makes
this short. Watch what happens when the amount equals the length of the
string, and watch what a slice does when the amount is 0. Don't overthink it!

### Question 5: `clean_username`

Write a function `clean_username` that takes a string `raw` and returns it
with the surrounding whitespace removed and the rest lowercased.

```python
clean_username("  Ada  ")
# "ada"
clean_username("ZO\n")
# "zo"
clean_username("Mixed Case")
# "mixed case"
```

Try writing this in one line! There are two methods that can be combined into one like this:

```py
str.method1().method2()
```

### Question 6: `describe_total`

Write a function `describe_total` that takes a string `label` and a number
`amount`, and returns a line with the amount shown to two decimal places.

```python
describe_total("Coffee", 3.5)
# "Coffee: $3.50"
describe_total("Tax", 0.07)
# "Tax: $0.07"
describe_total("Books", 12)
# "Books: $12.00"
```

An f-string can round a number for display with a **format specifier**, written after a
colon inside the braces.
[String formatting](https://www.w3schools.com/python/python_string_formatting.asp)
shows how this works.

### Question 7: `censor`

Write a function `censor` that takes a `text` and a `word`, and returns the
text with every copy of that word replaced by one star per letter.

```python
censor("I love beets", "beets")
# "I love *****"
censor("beets and beets", "beets")
# "***** and *****"
censor("no match here", "beets")
# "no match here"
```

A string method does the replacing. How can you figure out the number of stars without hard-coding it?

## Modify

Change the two functions already in `src/modify.py`.

### Question 8: `order_summary`

`order_summary` prints a summary of an order from values written into the
function. Change it so it asks the person for each one instead.

The values are **hard-coded**, meaning they are written into the source, so
the only way to summarize a different order is to edit the program. It
hard-codes the item, the quantity, the price, and whether the order is a
gift.

Do the following:

- Replace each with a call to `input()`, keeping the printed
  output exactly as it is.
- Two of those need converting, because `input()` always returns a string!
- The gift answer needs a different kind of conversion. The person types `y` or `n`, and
  `is_gift` has to end up as `True` or `False` so the `if` still works. Accept
  an uppercase or lowercase answer.

### Question 9: `print_receipt`

`print_receipt(items, total)` is called with a _list_ (not a string) called `items` and a float
called `total`.

For example:

```py
print_receipt(["Tea", "Mug"], 11.25)
```

It then prints each item on its own line, then the word `Total:` on
one line and the amount on the next. 

```text
Tea
Mug
-----
Total:
11.25
```

Change it so the items print on a single line separated by `", "`, and the total prints as one line below it.

```text
Tea, Mug
Total: $11.25
```

_Hint: Look into the string.join(list) method_

## Debug

### Question 10: `total_with_tax`

`total_with_tax` takes a price that arrived as text and should return it plus

1. It raises a `TypeError` instead.

```text
TypeError: can only concatenate str (not "int") to str
```

Read the message closely. It says what Python was asked to do and why it
refused.

### Question 11: `first_initial`

`first_initial` should return the first letter of a name, capitalized. It
returns the second letter, and raises an `IndexError` on a name one letter
long.

```python
first_initial("ada")   # want "A", gets "D"
first_initial("b")     # IndexError
```

One number is wrong, and fixing it fixes both problems at once.

## Submitting

```sh
git add -A
git commit -m "your message"
git push
```

Pushing runs the tests on GitHub. Check the **Actions** tab for your score,
then open a pull request to your instructor for feedback.

## Good luck!

Almost every program takes something in and puts something out, so this is the
part you will reach for most often. You got this!
