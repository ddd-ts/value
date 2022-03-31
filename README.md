# @ddd-ts/value

A flexible domain driven design tool for encapsulating your domain values into objects

# Description

A tool helping ensure correctness of domain values, and allowing encapsulation of value domain logic

## Features

- Primitives VO
  - String
  - Number
  - Boolean
- Structured VO
- Validation
- Serialization / Deserialization

### Primitives

Most values of a domain are primitives.
We can encapsulate domain logic within those primitives to protect their meaning.

- String

```ts
class Fullname extends Value(String) {
  get firstname() {
    return this.value.split(" ")[0];
  }

  get lastname() {
    return this.value.split(" ")[1];
  }
}
```

````
class Age extends Value(Number) {}
class IsHuman extends Value(Boolean) {}

```ts
class InvalidMoney extends Error {
  constructor(value: number, reason: string) {
    super(`Money value ${value} invalid: ${reason}`);
  }
}

class MoneyTooPreciseError extends InvalidMoney {
  constructor(value: number) {
    super(value, "Precision must be greater than 0.01");
  }
}

class Money extends Value(Number) {
  ensureValidity() {
    if (Math.round(this.value * 100) !== this.value * 100) {
      throw new MoneyTooPreciseError(this.value);
    }
  }

  print() {
    return `$${this.value}`;
  }
}

const taxes = Money.deserialize(2.01);
taxes.print(); // '$2.01'
taxes.serialize(); // 2.01
````
