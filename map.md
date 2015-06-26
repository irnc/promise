TODO: write rationale.

```js
var Promise = require('bluebird');
var pipeline = new require('mongoose').Aggregate();

Promise.resolve(pipeline.exec()).map(processMarketplace);
```

---

```js
var pipeline = new require('mongoose').Aggregate();

pipeline.exec().then(function (marketplaces) {
  return marketplaces.map(processMarketplace);
});
```
