## Node SQL Query Builder

This is adapter-based SQL query builder. Building queries for different DB engines 
is handled by separate adapters. MySQL support is included.  It makes this query 
builder highly extendable.

## How to use

### Selects

```js
require('colors');

var Select = require('sql-builder').select;

// you can assign some query data on construction
var query = new Select({
	group:'time',
	having:['t = 1 ASC', 'b = 2 DESC']
});

// you can assign special query options
query.option('DISTINCT');

// You can use chains
query
	.select('u.*')
	.from('users AS u');

// put array or string or object
query.where("u.name = 'john'");

// set where to be deleted later
query.select('(SELECT COUNT(*) FROM messages) AS messages)', 'selmsg');

query.select('a.avatar').join('user_attr AS a ON a.user_id = u.id');

// show 10 items per page and 5th page
query.offset(5, 10);

// we decided not to calculate messages
query.resetSelect('selmsg');

// render to string throu renderer
console.log(query.toString().bold.blue);
```

### Delete

```js
require('colors');

var Delete = require('sql-builder').delete;

// add parametres on cunstruction
var query1 = new Delete({from: 'users', where:["id = 10", "blocked = 0"]});
console.warn(query1.toString().bold.green);

var query2 = new Delete();

// use chanes
query2.from('users').where('id = 10');

// add key to value later can delet by key
query2.where('blocked = 1', 'c1');

// put array
query2.where(['a = 1', 'a = 2']);

// put object
query2.where({test:'my = 2'});

// reset key. If no key all where reseted
query2.resetWhere('c2');

console.warn(query2.toString().bold.blue);
```

### Insert

```js
require('colors');

var Insert = require('sql-builder').insert;

var query = new Insert();

query.ignore(true);

query.into('user');

query.values({
	id: 0,
	name: 'sergey'
});

query.values({
	block: 0,
	password: '123'
});

query.values({
	description: "this is user's \"descritption\"! \n Or ''not? \n Anyway #1 user to test simbols."
});

query.values([
	{ id: 0, name: 'sergey' },
	{ id: 1, name: 'andrey' },
	{ id: 2, name: 'pavel' }
]);

query.values([
	'sergey',
	'andrey',
	'pavel'
]);

console.log(query.toString().bold.blue);
```

### Update

```js
require('colors');

var Update = require('sql-builder').update;

var query = new Update();

query.table('user');

query.set({
	'name': 'sergey',
	'isAdmin': false
});

query.where('id = 1').where(['block = 0', 'date > NOW()']);

console.log(query.toString().bold.blue);
```

## Change Log

#### v.0.0.5Beta

- Added feature to create INSERT with multiple value
- Added feature to specify the names of columns in the INSERT
- Added SET keyword into UPDATE query
- Escape names of fields into UPDATE query
- Correct substitution in INSERT and UPDATE values of types Boolean and Number
- Fixed field names in case JOIN multiple table

#### v.0.0.4Beta

- Syntax changes according to jshint.
- Added colors dependency only to run examples.

#### v.0.0.3Beta

- small tunes of the code to be clearer and more readable.
