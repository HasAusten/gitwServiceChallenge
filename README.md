TMC API Service Challenge
===

Introduction
--------------------
Your company has gone SOA, all the way. It started off with so much promise: microservices as far as the eye could see, fully decoupled teams, and reusability out the wazoo.

Slowly, chaos begin to creep in. These services were not quite similar enough to make sense. Service A was written in Go and UTF-8 friendly, while Service B was written in Node and ASCII only. You had n services and 2n + 1 databases. It's time to add some sanity!

Your job is to bring up a simple caching service. Your service will exist solely to get, put, and delete data into and out of a cache. All these methods will support a clean and transparent contract to make cache interactions as simple as possible.

Your clients will push you different types of data (all valid JSON), they'll do it concurrently, and they expect a fast response, so choose an appropriate language with which you will implement this new caching service.

Endpoints
--------------------
Your caching service will operate at http://localhost:8088/. Please provide the following endpoints which handle the following HTTP verbs:
* /cache/
 * POST - creates a new item in the cache. The body of the POST should match the contract specified in the Contract section. 
 * GET - gets the entire cache.
 * DELETE - deletes the cache.

* /cache/{key}
 * GET - gets the fully-specified key / value pair.
 * PUT - updates the cache item with matching key. The body of the PUT should match the contract specified in the Contract section.
 * DELETE - deletes the cache item with matching key.

Contract
--------------------
Your cache service will expect input in valid JSON and emit output in valid JSON. A single cache item will always be described thusly:
```{
    "key": "{key}",
    "value": "{value}"
}```

An example payload:
```{
    "key": "problem_free_philosophy",
    "value": "Hakuna Matata"
}```

Keys and values may be any string, boolean, integer, or decimal value.

Any service call that returns more than one cache item will return a "cache" key with an array of the JSON object above:
```{
    "cache": [
        {
            "key": "foo",
            "value": 3.9999
        },
        {
            "key": "bar",
            "value": true
        }
    ]
}```

Additional Specifications
===
Implement at least 2 of the following 4 requirements:

Requirement #1
------------------
A successful POST should return status code 201 with a link to the new cache item's key in the Location header. It should return 409 if a resource with that key already exists.

GET, PUT, and DELETE should return 404 if passed in a cache item key that does not exist in the cache.

If invalid JSON is passed in to a PUT or POST, return a status code 406.

A successful PUT or DELETE should result in status code 204.

Requirement #2
------------------
The contents of your cache must survive a restart of the machine hosting the cache service.

Requirement #3
------------------
A cache item should be purged from the cache after it's been returned in any GET call 100 times.

Requirement #4
------------------
Your service must adhere to an SLA of 100ms at a request load of 100 requests per second for a period of 5 minutes.
