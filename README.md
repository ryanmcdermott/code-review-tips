# code-review-tips

## Table of Contents
  1. [Introduction](#introduction)
  2. [Why Review Code?](#why-review-code)
  3. [Basics](#basics)
  4. [Readability][#readability]
  5. [Exceptions][#exceptions]
  6. [User Input][#user-input]
  7. [Security][#security]

## Introduction
Code reviews can inspire dread in both reviewer and reviewee. Having your
code analyzed can feel like getting screened by the TSA as you go off to a
beautiful vacation. Even worse, reviewing other people's code can feel like
a painful and ambiguous exercise, searching for problems and not even knowing where to begin.

This project aims to provide some solid tips for how to review the code you and
your team write. This is by no means an exhaustive list, but it should help you
catch as many bugs as possible from a small and memorable set of ideas that can
hopefully be applied to any review.

## Why Review Code?
Code reviews are a necessary part of the software engineering process because
you alone as a developer can't catch every problem in a piece of code you
write. That's ok though! Even the best basketball players in the world miss
shots.

Having others review our work ensures that we deliver the best product to users
and with the least amount of errors. Make sure your team implements a code
review process for new code that is introduced into your code base. Find a
process that works for you and your team, there's no one size fits all. The
important point is to do code reviews as regularly as possible.

## Basics
Code reviews should:

- Avoid details that can be handled by a static analysis tool. Don't argue about
details such as code formatting and whether to use `let` or `var`. Having a
formatter and linter can save your team a lot of time from reviews that your
computer can do for you.
- Avoid API discussion. These discussions should happen before the code is even
written. Don't try to argue about the floor plan once you've poured the concrete
foundation.
- Be kind. It's scary to have your code reviewed is scary and can bring about
feelings of insecurity in even the most experienced developer. Be positive
in your language and keep your teammates comfortable and secure in their work!

## Readability

### Typos
Avoid nitpicking as much as you can and save it for your linter, compiler, and
formatter. When you can't, such as in the case of typos, leave a kind comment
suggesting a fix. It's the little things that make a big difference sometimes!

### Variable and function names
Naming is one of the hardest problems in computer science. We've all given names
to variables, functions, and files that are confusing. Help your teammate about
by suggesting a clear name.

```javascript
// This function could be better named as namesToUpperCase
function u(names) {
  // ...
}
```
