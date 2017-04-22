# TypeScript

* [TypeScript loves React](https://medium.com/@basarat/typescript-developers-love-react-9871b494bc1a)

https://github.com/paulcbetts/spawn-rx/pull/12/files

## Structural vs Nominal

TypeScript is structural rather than nominal, meaning that it works basically like automatic duck typing rather than name or identity based type checking. That is a good fit for dynamic languages like JavaScript.

Go uses interfaces and structural typing. Structural typing is like compile-time duck typing.

Structural typing is an alternative to classical inheritance in statically typed languages. It allows you to write generic algorithms without obscuring the definition of a type in a sea of interfaces. It helps statically type languages capture the feel and productivity of dynamically typed languages.

Flow uses structural typing for objects and functions, but nominal typing for classes.

## Interfaces

In addition to classes, we can use interfaces to drive code consistency between domain concepts.