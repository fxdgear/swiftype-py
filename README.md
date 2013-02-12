Swiftype API Client for Python
===============================

# Configuration:

Before issuing commands to the API, configure the client with your API key:

	import swiftype
	client = swiftype.Client(api_key='YOUR_API_KEY')

You can find your API key in your [Account Settings](https://swiftype.com/user/edit).

# Search

If you want to search for e.g. `action` on your engine, you can use:

	results = client.search('bookstore', 'action')

To limit the search to only the `books` DocumentType:

	results = client.search_document_type('bookstore', 'books', 'action')

Both search methods allow you to specify options as an extra parameter to e.g. filter or sort on fields. For more details on these options pelease have a look at the [Search Options](https://swiftype.com/documentation/searching). Here is an example for showing only books that are in stock:

	results = client.search_document_type('bookstore', 'books', 'action', {'filters': {'books': {'in_stock': true}}})

# Autocomplete

Autocompletes have the same functionality as searches. You can autocomplete using all documents:

	results = client.suggest('bookstore', 'acti')

or just for one DocumentType:

	results = client.suggest_document_type('bookstore', 'books', 'acti')

or add options to have more control over the results:

	results = client.suggest('bookstore', 'acti', {'sort_field': {'books': 'price'}})

# Engines

Retrieve every `Engine`:

	engines = client.engines

Create a new `Engine` with the name `bookstore`:

	engine = client.create_engine('bookstore')

Retrieve an `Engine` by `slug` or `id`:

	engine = client.engine('bookstore')

To delete an `Engine` you need the `slug` or the `id` field of an `engine`:

	client.destroy_engine('bookstore')

# Document Types

Retrieve `DocumentTypes`s of the `Engine` with the `slug` field `bookstore`:

	document_types = client.document_types('bookstore')

Show the second batch of documents:

	document_types = client.document_types('bookstore', 2)

Create a new `DocumentType` for an `Engine` with the name `books`:

	document_type = client.create_document_type('bookstore', 'books')

Retrieve an `DocumentType` by `slug` or `id`:

	document_type = client.document_type('bookstore', 'books')

Delete a `DocumentType` using the `slug` or `id` of it:

	client.destroy_document_type('bookstore', 'books')

# Documents

Retrieve all `Document`s of `Engine` `bookstore` and `DocumentType` `books`:

	documents = client.documents('bookstore', 'books')

Retrieve a specific `Document` using its `id` or `external_id`:

	document = client.document('bookstore', 'books', 'id1')

Create a new `Document` with mandatory `external_id` and user-defined fields:

	document = client.create_document('bookstore', 'books', {
		'external_id': '1',
		'fields': [
			{'name': 'title', 'value': 'Information Retrieval', 'type': 'string'},
			{'name': 'genre', 'value': 'non-fiction', 'type': 'enum'},
			{'name': 'author', 'value': 'Stefan Buttcher', 'type': 'string'},
			{'name': 'in_stock', 'value': true, 'type': 'enum'},
			{'name': 'on_sale', 'value': false, 'type': 'enum'}
		]})

Create multiple `Document`s at once and return status for each `Document` creation:

	stati = client.create_documents('bookstore', 'books', [{
		'external_id': '2',
		'fields': [
			{'name': 'title', 'value': 'Lucene in Action', 'type': 'string'},
			{'name': 'genre', 'value': 'non-fiction', 'type': 'enum'},
			{'name': 'author', 'value': 'Michael McCandless', 'type': 'string'},
			{'name': 'in_stock', 'value': true, 'type': 'enum'},
			{'name': 'on_sale', 'value': false, 'type': 'enum'}
		]},{
		'external_id': '3',
		'fields': [
			{'name': 'title', 'value': 'MongoDB in Action', 'type': 'string'},
			{'name': 'genre', 'value': 'non-fiction', 'type': 'enum'},
			{'name': 'author', 'value': 'Kyle Banker', 'type': 'string'},
			{'name': 'in_stock', 'value': true, 'type': 'enum'},
			{'name': 'on_sale', 'value': false, 'type': 'enum'}
		]}])

Update fields of an existing `Document` specified by `id` or `external_id`:

	client.update_document('bookstore','books','1', { 'in_stock': false, 'on_sale': false })

Update multiple `Document`s at once:

	stati = client.update_documents('bookstore','books', [
		{'external_id': '2', 'fields': {'in_stock': false}},
		{'external_id': '3', 'fields': {'in_stock': true}}
	])

Create or update a `Document`:

	document = client.create_or_update_document('bookstore', 'books', {
		'external_id': '1',
		'fields': [
			{'name': 'title', 'value': 'Information Retrieval', 'type': 'string'},
			{'name': 'genre', 'value': 'non-fiction', 'type': 'enum'},
			{'name': 'author', 'value': 'Stefan Buttcher', 'type': 'string'},
			{'name': 'in_stock', 'value': false, 'type': 'enum'},
			{'name': 'on_sale', 'value': true, 'type': 'enum'}
		]})

Create or update multiple `Documents` at once:

	stati = client.create_or_update_documents('bookstore', 'books', [{
		'external_id': '2',
		'fields': [
			{'name': 'title', 'value': 'Lucene in Action', 'type': 'string'},
			{'name': 'genre', 'value': 'non-fiction', 'type': 'enum'},
			{'name': 'author', 'value': 'Michael McCandless', 'type': 'string'},
			{'name': 'in_stock', 'value': false, 'type': 'enum'},
			{'name': 'on_sale', 'value': false, 'type': 'enum'}
		]},{
		'external_id' => '3',
		'fields': [
			{'name': 'title', 'value': 'MongoDB in Action', 'type': 'string'},
			{'name': 'genre', 'value': 'non-fiction', 'type': 'enum'},
			{'name': 'author', 'value': 'Kyle Banker', 'type': 'string'},
			{'name': 'in_stock', 'value': false, 'type': 'enum'},
			{'name': 'on_sale', 'value': false, 'type': 'enum'}
		]}])

Destroy a `Document`:

	client.destroy_document('bookstore','books','1')

Destroy multiple `Document`s at once:

	stati = client.destroy_documents('bookstore','books',['1','2','3'])

# Domains

Retrieve all `Domain`s of `Engine` `websites`:

	domains = client.domains('websites')

Retrieve a specific `Domain` by `id`:

	domain = client.domain('websites', 'generated_id')

Create a new `Domain` with the URL `https://swiftype.com` and start crawling:

	domain = client.create_domain('websites', 'https://swiftype.com')

Delete a `Domain` using its `id`:

	client.destroy_domain('websites', 'generated_id')

Initiate a recrawl of a specific `Domain` using its `id`:

	client.recrawl_domain('websites', 'generated_id')

Add or update a URL for a `Domain`:

	client.crawl_url('websites', 'generated_id', 'https://swiftype.com/new/path/about.html')

# Analytics

To get the amount of searches on your `Engine` in the last 14 days use:

	searches = client.analytics_searches('bookstore')

You can also use a specific start and/or end date:

	searches = client.analytics_searches('bookstore', '2013-01-01', '2013-02-01')

To get the amount of autoselects (clicks on autocomplete results) use:

	autoselects = client.analytics_autoselects('bookstore')

As with searches you can also limit by start and/or end date:

	autoselects = client.analytics_autoselects('bookstore', 2, 10)

If you are interested in the top queries for your `Engine` you can use:

	top_queries = client.analytics_top_queries('bookstore')

To see more top queries you can paginate through them using:

	top_queries = client.analytics_top_queries('bookstore', page=2)
