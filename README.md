# root

A super lightweight web framework with routing and prototype [mixin](https://github.com/mafintosh/protein) support.

It's available through npm:

	npm install root

## Usage

Usage is simple

``` js
var root = require('root');
var app = root();

app.get('/', function(request, response) {
	response.send({hello:'world'});
});
app.post('/echo', function(request, response) {
	request.on('json', function(body) {
		response.send(body);
	});
});
app.listen(8080);
```

You can extend the request and response with your own methods

``` js
app.use('response.time', function() {
	this.send({time:this.request.time});
});
app.use('request.time', {getter:true}, function() {
	return Date.now();
});

app.get(function(req, res) {
	res.time();
});
```

## Routing

Routing is done using [murl](https://github.com/mafintosh/murl).
Use the `get`, `post`, `put`, `del`, `patch` or `options` method to specify the HTTP method you want to route

``` js
app.get('/hello/{world}', function(req, res) {
	res.send({world:req.params.world});
});
app.get('/test', function(req, res, next) {
	// call next to call the next matching route
	next();
});
app.get('/test', function(req, res) {
	res.send('ok');
});
```

## Error handling

You can specify an error handler for a specific error code by using the `error` function

``` js
app.get('/foo', function(req, res, next) {
	res.error(400, 'bad request man'); // or use next(400)
});

app.error(404, function(req, res) {
	res.send({error:'could not find route'});
});
app.error(function(req, res) {
	res.send({error:'catch all other errors'});
});
```

## Plugins

To create a plugin simply create a function that accepts an `app`

``` js
var plugin = function(app) {
	app.get('/my-plugin', function(req, res) {
		res.send('hello from plugin');
	});
};

myApp.use(plugin);
```

## License

MIT