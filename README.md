[![CodeQL](https://github.com/EveryMundo/aws-kinesis-client/actions/workflows/codeql-analysis.yml/badge.svg)](https://github.com/EveryMundo/aws-kinesis-client/actions/workflows/codeql-analysis.yml)

# aws-kinesis-client
A helper to ease the usage of kinesis streams while saving some money

- [Instalation](#instalation)
- [Why we created this package](docs/why-we-created-this-package.md)
- [Usage](#usage)
  - [Lambda Consumer](#lambda-consumer)
  - [Data Producer](#data-producer)
  


## Instalation

```sh
npm install @everymundo/aws-kinesis-client
```

## Usage

### Lambda Consumer
```js
// index.js
const kinesisParser = require('@everymundo/aws-kinesis-client/lib/parse-kinesis-record')

const handler = (event) => {
  // flatJsonRecordsLambda expects that each element in event.Record to be
  // a valid lambda Kinesis Record input with
  // {"eventSource": "aws:kinesis","kinesis":{"data":"W3siYSI6MX0seyJhIjoyfSx7ImEiOjN9XQ=="}}
  const myRecords = []
  for (const record of kinesisParser.flatJsonRecordsLambda(event.Records)) {
    // record is a single object
    myRecords.push(record)
  }

  return myRecords
}

const assert = require('assert')
const testHandler = () => {
  const event = {
    Records: [
      { eventSource: 'aws:kinesis', kinesis:{ data: 'W3siYSI6MX0seyJhIjoyfSx7ImEiOjN9XQ==' } },
      { eventSource: 'aws:kinesis', kinesis:{ data: 'W3siYSI6NH0seyJhIjo1fSx7ImEiOjZ9XQ==' } }
    ]
  }

  const response = handler(event)

  assert(response.length === 6)
  assert(JSON.stringify(response) === '[{"a":1},{"a":2},{"a":3},{"a":4},{"a":5},{"a":6}]')
}
```

### Data Producer
TODO
