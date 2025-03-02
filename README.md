# json-schema-transformer

## Overview

`json-schema-transformer` is a library for enforcing schema-based transformations on JSON data. It ensures data consistency by applying type conversions, custom formats, and default values according to the defined schema, making your JSON data compliant and well-structured.

## Getting Started

## Installation

To install json-schema-transformer:

```sh
npm install json-schema-transformer
```

### Basic Usage

```js
const schema = {
  type: 'object',
  properties: {
    invoiceNumber: { type: 'number' },
    invoiceDate: { type: 'string', format: 'datetime' },
    issuer: {
      type: 'object',
      properties: {
        name: { type: 'string', transform: ['toLowerCase'] },
        country: { type: 'string', default: 'USA' },
      },
    },
  },
};

const data = {
  invoiceNumber: '123',
  invoiceDate: '2024-08-15',
  issuer: { name: 'Tech Innovators' },
};

const jsonFormatter = new JsonFormatter();
const output = jsonFormatter.execute(schema, data);
console.log(output);

// {
//   invoiceNumber: 123,
//   invoiceDate: '2024-08-15T03:00:00.000Z',
//   issuer: { name: 'tech innovators', country: 'USA' },
// };
```

### Custom Formats

You can add and replace any default formats using addFormat method:

```js
const schema = {
  type: 'object',
  properties: {
    code: { type: 'string', format: 'customFormat' },
  },
};

const data = {
  code: 'foo',
};

const formatter = new JsonFormatter();

formatter.addFormat('customFormat', {
  type: 'string',
  format: (value) => `G-${value}`,
});

const output = jsonFormatter.execute(schema, data);
console.log(output);

// {
//   code: 'G-foo',
// };
```
