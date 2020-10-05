# challenge-ping-tree

This challenge is to build an API for the http server to receive POST requests with information about a visitor and respond with a decision, either: rejection, or accept with a corresponding target url.

## Decisioning

1) The decisions should be based on information about the visitor that comes in POST request body
 
        {
            "geoState": "ca",
            "publisher": "abc",
            "timestamp": "2018-07-19T23:28:59.513Z"
        }

2) No `target` can receive more traffic per day than it allows. Here's an example `target` with max 10 accepts per day

        {
            "id": "1",
            "url": "http://example.com",
            "value": "0.50",
            "maxAcceptsPerDay": "10",
            "accept": {
                "geoState": {
                    "$in": ["ca", "ny"]
                },
                  "hour": {
                    "$in": [ "13", "14", "15" ]
                  }
            }
        }

4) All remaining `targets` should be filtered by criteria they are willing to accept. The above `target` example only accepts visitors from either California or New York from 13 to 16 UTC.

5) If no `targets` are left, the request is rejected (returns `{"decision":"reject"}`), otherwise it should return the url of the remaining `target` with the highest value.


## Requirements

1. New endpoints that needs to be built:
   - POST /api/targets
      - post/update a target
   - GET /api/targets
      - get all targets
   - GET /api/target/:id
      - get a target by id
   - POST /route
      - post a request with information about the visitor
      - respond with a decision

2. API functional tests. 
   - each endpoint should have its own test
   - write all tests in `test/endpoints.js`
   - be sure to understand how [`servertest`](https://github.com/rvagg/servertest) works


All persistence should use [redis](http://redis.io). 
Source code should be in `src` dir and use redis client factory from `src/redis.js`


## Instructions

How to attempt this challenge:

1. Create a new repo in your account and note the git url
2. Clone this repo
3. Solve the challenge, following our [coding guidelines](https://github.com/Interlincx/adnet-onboarding)
4. Set your new repo as the origin: `git remote set-url origin ${your repo url}`
5. Push your solution to your repo

You must follow these steps for your solution to be accepted -- forks or other methods will not be considered.
