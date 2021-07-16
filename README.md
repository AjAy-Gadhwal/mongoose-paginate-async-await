
# mongoose-paginate-async-await

I used the reference of [mongoose-paginate](https://www.npmjs.com/package/mongoose-paginate) library and this library has some node version-related issues. Because of it, I rewrited all code using "async, await", optimize code, and changed parameters.

> Pagination plugin for [Mongoose](http://mongoosejs.com)
## Installation

```sh
npm install mongoose-paginate-async-await
```

## Usage

Add plugin to a schema and then use model `paginate` method:

```js
var mongoose = require('mongoose');
var mongoosePaginate = require('mongoose-paginate-async-await');

var schema = new mongoose.Schema({ /* schema definition */ });
schema.plugin(mongoosePaginate);

var Model = mongoose.model('Model',  schema); // Model.paginate()
```

### await Model.paginate([query], [options])
#### OR
### Model.paginate([query], [options], [callback])

**Parameters**

* `[query]` {Object} - Query criteria. [Documentation](https://docs.mongodb.org/manual/tutorial/query-documents)
* `[options]` {Object}
  - `[select]` {Object | String} - Fields to return (by default returns all fields). [Documentation](http://mongoosejs.com/docs/api.html#query_Query-select) 
  - `[sort]` {Object | String} - Sort order. [Documentation](http://mongoosejs.com/docs/api.html#query_Query-sort) 
  - `[populate]` {Array | Object | String} - Paths which should be populated with other documents. [Documentation](http://mongoosejs.com/docs/api.html#query_Query-populate)
  - `[lean=false]` {Boolean} - Should return plain javascript objects instead of Mongoose documents?  [Documentation](http://mongoosejs.com/docs/api.html#query_Query-lean)
  - `[page=1]` {Number}
  - `[perPage=10]` {Number}
* `[callback(err, result)]` - If specified the callback is called once pagination results are retrieved or when an error has occurred

**Return value**

Promise fulfilled with object having properties:
* `data` {Array} - Array of documents
* `documents` {Number} - Total number of documents in collection that match a query
* `select` {Object | String} - Selected fields
* `sort` {Object | String} - Selected fields
* `page` {Number} - Page values were used 
* `pages` {Number} - Pages values were used 
* `perPage` {Number} - PerPage that was used

### Examples

#### Skip 20 documents and return 10 documents

With async/await:
```js
const result = await Model.paginate({}, { page: 3, perPage: 10 });
```

With callback function:
```js
Model.paginate({}, { page: 3, perPage: 10 }, function(err, result) {});
```

With promise:
```js
Model.paginate({}, { page: 3, perPage: 10 }).then(function(result) {});
```

#### More advanced example

```js
var query = {};
var options = {
  select: 'title date author',
  sort: { date: -1 },
  populate: 'author',
  lean: true,
  page: 1, 
  perPage: 10
};

const result = await Model.paginate({}, { page: 3, perPage: 10 });
```

#### Set custom default options for all queries

config.js:

```js
var mongoosePaginate = require('mongoose-paginate');

mongoosePaginate.paginate.options = { 
  lean:  true,
  perPage: 20
};
```
## License

[MIT](LICENSE)
