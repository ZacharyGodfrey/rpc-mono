# rpc-mono

A mono repo containing an RPC-style HTTP API and a statically generated client application

![commit][shield-commit]
![language][shield-lang]
![tests][shield-tests]
![coverage][shield-coverage]

## API

Unlike RESTful APIs which are structured around the *nouns* of a domain, this API is structured around the *verbs* of the domain. Instead of asking, "What data do I want to manipulate?" you should instead ask, "What do I want to do?"

The aim of the API is to use the smallest possible subset of HTTP features in order to expose application logic over the network. As you'll see in the details below, the goal is to keep the HTTP spec out of the way as much as possible so that the API is essentially a collection of functions with inputs and outputs that are executed on a remote server.

### HTTP Requests

- All requests use the HTTP method `POST`
- All requests contain exactly 1 URI segment which specifies the name of the intended action
- All input data (including auth tokens) are specified in the request body as JSON data
- Additional URI segments, query string parameters, headers, cookies, etc. will be completely ignored

```
POST /{actionName}
{
    "token": "auth-token-here",
    "data": {
        "key1": "value1",
        "key2": "value2",
        "key3": "value3"
    }
}
```

### HTTP Responses

- All responses use the HTTP status code `200`
- The `ok` key in the response JSON is the indicator for success or failure

#### Success

- `messages`: an empty array or a collection of success messages
- `data`: any JSON data returned from the requested action

```json
{
    "ok": true,
    "messages": [
        "The data was saved successfully."
    ],
    "data": {
        "key": "value"
    }
}
```

#### Failure

- `messages`: a collection of one or more error messages
- `data`: always `null` when `ok` is `false`

```json
{
    "ok": false,
    "messages": [
        "An invalid auth token was provided."
    ],
    "data": null
}
```

## Client

Will be hosted in [Netlify][hosting-netlify].

## Links

- [API Hosting: Heroku][hosting-heroku]
- [Client Hosting: Netlify][hosting-netlify]
- [VS Code Online][dev-env]

[shield-commit]: https://img.shields.io/github/last-commit/ZacharyGodfrey/rpc-mono/main?style=flat-square
[shield-lang]: https://img.shields.io/github/languages/top/ZacharyGodfrey/rpc-mono?style=flat-square
[shield-tests]: https://img.shields.io/github/workflow/status/ZacharyGodfrey/rpc-mono/CI%20Workflow/main?style=flat-square
[shield-coverage]: https://img.shields.io/badge/dynamic/json?style=flat-square&color=blue&label=coverage&query=$.total.statements.pct&suffix=%&url=https://raw.githubusercontent.com/ZacharyGodfrey/rpc-mono/main/api/coverage/coverage-summary.json
[hosting-heroku]: https://heroku.com
[hosting-netlify]: https://netlify.com
[dev-env]: https://vscode.dev/github/ZacharyGodfrey/rpc-mono