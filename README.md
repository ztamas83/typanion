# Clipanion

> Static and runtime type assertion library with no dependencies

[![](https://img.shields.io/npm/v/typanion.svg)]() [![](https://img.shields.io/npm/l/typanion.svg)]() [![](https://img.shields.io/badge/developed%20with-Yarn%202-blue)](https://github.com/yarnpkg/berry)

## Installation

```
yarn add typanion
```

## Why

  - Typanion can validate nested arbitrary data structures
  - Typanion is type-safe; it uses [type predicates](https://www.typescriptlang.org/docs/handbook/advanced-types.html#user-defined-type-guards)
  - Typanion allows you to derive types from your schemas

Note: Typanion's standard library isn't huge at the moment (mostly strings, arrays, shapes), more types are expected in future versions.

## Usage

First define a schema using the builtin operators:

```ts
import * as type from 'typanion';

const movie = type.object({
    title: type.string(),
    description: type.string(),
});
```

Then just call the schema to validate any `unknown` value:

```ts
const userData = JSON.parse(input);

if (movie(userData)) {
    console.log(userData.title);
}
```

You can derive the type from the schema for use in other functions:

```ts
import * as type from 'typanion';

const movie = type.object({
    title: type.string(),
    description: type.string(),
});

type Movie = type.InferType<typeof movie>;

// Then just use your alias:
const printMovie = (movie: Movie) => {
    // ...
};
```

Types can be kept in separate variables if needed:

```ts
import * as type from 'typanion';

const actor = type.object({
    name: type.string();
});

const movie = type.object({
    title: type.string(),
    description: type.string(),
    actors: type.array(actor),
});
```

## API

### Type predicates

- `array(spec)` will ensure that the values are arrays whose values all match the specified schema.

- `cascade(spec, [specA, specB, ...])` will ensure that the values all match `spec` and, if they do, run the followup validations as well. Since those followups will not contribute to the inference (only the lead schema will), you'll typically want to put here anything that's a logical validation, rather than a typed one (cf the [Cascading Predicates]() section).

- `literal(value)` will ensure that the values are strictly equal to the specified expected value. It's an handy tool that you can combine with `oneOf` and `object` to parse structures similar to Redux actions, etc.

- `object(props)` will ensure that the values are plain old objects whose properties match the given shape.

- `oneOf([specA, specB])` will ensure that the values all match any of the provided schema. As a result, the inferred type is the union of all candidates.

- `string()` will ensure that the values are all regular strings.

### Cascading predicates:

- `maxLength` will ensure that the values all have a `length` property at most equal to the specified value.

- `minLength` will ensure that the values all have a `length` property at least equal to the specified value.

## License (MIT)

> **Copyright © 2019 Mael Nison**
>
> Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:
>
> The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.
>
> THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
