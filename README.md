# repc

Simple yet powerful JSON-RPC client for JavaScript and Node.js.

## Installation

```shell
npm i repc
```

## Usage

```javascript
repc(url, options)
```

Call:

```javascript
import repc from 'repc';

const math = repc('https://math.juana.dev/v1');

const result = await math.call('add', [2, 2]);
```

Notification:

```javascript
import repc from 'repc';

const math = repc('https://math.juana.dev/v1');

await math.notify('ping');
```

Batch:

```javascript
import repc, { NOTIFICATION } from 'repc';

const math = repc('https://math.juana.dev/v1');

const responses = await math.batch(
    (builder) => builder
        .call('add', [2, 2])
        .call('div', [3.14, 0])
        .call('mul', [6, 6])
        .call('sub', [10, 5])
        .notify('ping')
);
```

## Options

### `id`

ID generation function.

- type: `function(method, params)`

### `headers`

Headers when using over HTTP.

- type: `object`
- example: `{ Authorization: 'Bearer ...' }`

### `transport`

Data transportation function. Must return `Promise<string>`.

- type: `function(url, data, context)`
- example:

```javascript
(url, data, context) =>
    fetch(url, {
        body: JSON.stringify(data),
        headers: context.options.headers,
    }).then((r) => r.text());
```

## Methods

### `send(request, options)`

Make request.

### `call(method, params, options)`

Call method.

### `notify(method, params, options)`

Send notification.

### `batch(builder, options)`

Make several requests at the same time.
Returns list of raw responses.
