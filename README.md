# code-review-tips

## Table of Contents
  1. [Introduction](#introduction)
  2. [Why Review Code?](#why-review-code)
  3. [Basics](#basics)
  4. [Readability](#readability)
  5. [Side Effects](#side-effects)
  6. [User Input](#user-input)
  7. [Security](#security)
  8. [Performance](#performance)

## Introduction
Code reviews can inspire dread in both reviewer and reviewee. Having your
code analyzed can feel like getting screened by the TSA as you go off to a
beautiful vacation. Even worse, reviewing other people's code can feel like
a painful and ambiguous exercise, searching for problems and not even knowing where to begin.

This project aims to provide some solid tips for how to review the code you and
your team write. All examples are written in JavaScript, but the advice should
be applicable to any project of any language.This is by no means an exhaustive
list but hopefully this will help you catch as many bugs as possible.

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

- Automate as much of the review as you can. That means avoid details that can
be handled by a static analysis tool. Don't argue about details such as code
formatting and whether to use `let` or `var`. Having a formatter and linter can
save your team a lot of time from reviews that your
computer can do for you.
- Avoid API discussion. These discussions should happen before the code is even
written. Don't try to argue about the floor plan once you've poured the concrete
foundation.
- Be kind. It's scary to have your code reviewed is scary and can bring about
feelings of insecurity in even the most experienced developer. Be positive
in your language and keep your teammates comfortable and secure in their work!

## Readability

### Typos should be corrected
Avoid nitpicking as much as you can and save it for your linter, compiler, and
formatter. When you can't, such as in the case of typos, leave a kind comment
suggesting a fix. It's the little things that make a big difference sometimes!

### Variable and function names should be clear
Naming is one of the hardest problems in computer science. We've all given names
to variables, functions, and files that are confusing. Help your teammate about
by suggesting a clearer name if one doesn't make sense.

```javascript
// This function could be better named as namesToUpperCase
function u(names) {
  // ...
}
```

### Functions should be short
Functions should do one thing! Long functions usually mean that they are doing
too much. Tell your teammate to split out the function into multiple different
functions.

```javascript
// This is both emailing clients and deciding which are active. Should be
// 2 different functions.
function emailClients(clients) {
  clients.forEach((client) => {
    const clientRecord = database.lookup(client);
    if (clientRecord.isActive()) {
      email(client);
    }
  });
}
```

### Files should be short
Just like functions, a file should be about one thing. A file represents a
module and a module should do one thing for your codebase.

For example, if your module is called `fake-name-generator` it should just be
responsible for creating fake names like "Keyser Söze". If the
`fake-name-generator` also includes a bunch of utility functions for querying a
database of names, that should be in a separate module.

There's no rule for how long a file should be, but if it's long like below and
includes functions that don't relate to one another ,then it should probably
be split apart.

```javascript
1: import _ from 'lodash';
2: function generateFakeNames() {
3:   // ..
4: }
...
1128: function queryRemoteDatabase() {
1129:   // ...  
1130: }
```

## Side Effects

### Functions should be pure
```javascript
// Global variable referenced by following function.
// If we had another function that used this name, now it'd be an array and it could break it.
let name = 'Ryan McDermott';

function splitIntoFirstAndLastName() {
  name = name.split(' ');
}

splitIntoFirstAndLastName();

console.log(name); // ['Ryan', 'McDermott'];
```

### I/O functions should have failure cases handled
Any function that does I/O should handle when something goes wrong

```javascript
function getIngredientsFromFile() {
  const onFulfilled = (buffer) => {
    let lines = buffer.split('\n');
    return lines.forEach(line => <Ingredient ingredient={line}/>
  };

  // What about when this rejected because of an error? What do we return?
  return readFile('./ingredients.txt').then(onFulfilled)
}
