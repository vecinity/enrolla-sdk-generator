# enrolla-cli [![Build Status][build]](https://github.com/vecinity/enrolla-sdk-generator/actions?query=branch%3Amaster+workflow%3ACI) [![npm]](https://www.npmjs.com/package/@enrolla/cli) [![mit]](https://opensource.org/licenses/MIT)

[build]: https://img.shields.io/github/workflow/status/bcherny/json-schema-to-typescript/CI/master?style=flat-square
[npm]: https://img.shields.io/npm/v/json-schema-to-typescript.svg?style=flat-square
[mit]: https://img.shields.io/npm/l/json-schema-to-typescript.svg?style=flat-square

Compile enrolla customer schemas to classes.
Forked and based on https://github.com/bcherny/json-schema-to-typescript

## Example

Input:
```json
{
  "title": "Example Schema",
  "type": "object",
  "properties": {
    "firstName": {
      "type": "string"
    },
    "lastName": {
      "type": "string"
    },
    "age": {
      "description": "Age in years",
      "type": "integer",
      "minimum": 0
    },
    "hairColor": {
      "enum": ["black", "brown", "blue"],
      "type": "string"
    }
  },
  "additionalProperties": false,
  "required": ["firstName", "lastName"]
}
```

Output:
```ts
import { ConfigClient } from '@enrolla/enrolla-ts'

export class Person {
  constructor(private objectId: string) {}

  firstName(): string {
    return ConfigClient.get(this.objectId, 'firstName')
  }

  lastName(): string {
    return ConfigClient.get(this.objectId, 'lastName')
  }

  /**
   * Age in years
   */
  age(): number | undefined {
    return ConfigClient.get(this.objectId, 'age')
  }

  hairColor(): ('black' | 'brown' | 'blue') | undefined {
    return ConfigClient.get(this.objectId, 'hairColor')
  }
}
```

## Installation

```sh
# Using Yarn:
yarn add @enrolla/cli --dev

# Or, using NPM:
npm install @enrolla/cli --save-dev
```

## Usage

```sh
enrolla-cli -i schemas/ -o configs/
```