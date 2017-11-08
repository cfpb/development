# CFPB API Standards

This document captures **CFPB's view of API best practices and standards**. We
aim to incorporate as many of them as possible into our work. This document is based on [18F's API Standards](https://github.com/18F/api-standards).

This document provides a mix of:

* **High level design guidance** that individual APIs interpret to meet their
needs.
* **Low level web practices** that most modern HTTP APIs use.

### Design for common use cases

For APIs that syndicate data, consider several common client use cases:

* **Bulk data.** Clients often wish to establish their own copy of the API's
dataset in its entirety. For example, someone might like to build their own
search engine on top of the dataset, using different parameters and technology
than the "official" API allows. If the API can't easily act as a bulk data
provider, provide a separate mechanism for acquiring the backing dataset in
bulk.
* **Staying up to date.** Especially for large datasets, clients may want to
keep their dataset up to date without downloading the data set after every
change. If this is a use case for the API, prioritize it in the design.
* **Driving expensive actions.** What would happen if a client wanted to
automatically send text messages to thousands of people or light up the side of
a skyscraper every time a new record appears? Consider whether the API's
records will always be in a reliable unchanging order, and whether they tend to
appear in clumps or in a steady stream. Generally speaking, consider the
"entropy" an API client would experience.

### Using one's own API

The #1 best way to understand and address the weaknesses in an API's design and
implementation is to use it in a production system.

Whenever feasible, design an API in parallel with an accompanying integration
of that API.

### Point of contact

Have an obvious mechanism for clients to report issues and ask questions about
the API.

When using GitHub for an API's code, use the associated issue tracker. In
addition, publish an email address for direct, non-public inquiries.

### Notifications of updates

Have a simple mechanism for clients to follow changes to the API.

Common ways to do this include a mailing list, or a
[dedicated developer blog](https://developer.github.com/changes/) with an RSS feed.

### API Endpoints

An "endpoint" is a combination of two things:

* The verb (e.g. `GET` or `POST`)
* The URL path (e.g. `/articles`)

Information can be passed to an endpoint in either of two ways:

* The URL query string (e.g. `?year=2014`)
* HTTP headers (e.g. `X-Api-Key: my-key`)

When people say "RESTful" nowadays, they really mean designing simple,
intuitive endpoints that represent unique functions in the API.

Generally speaking:

* **Avoid single-endpoint APIs.** Don't jam multiple operations into the same
endpoint with the same HTTP verb.
* **Prioritize simplicity.** It should be easy to guess what an endpoint does
by looking at the URL and HTTP verb, without needing to see a query string.
* Endpoint URLs should advertise resources, and **avoid verbs**.

Some examples of these principles in action:

* [CFPB's HMDA API](https://cfpb.github.io/api/hmda/)
* [FBOpen API documentation](https://18f.github.io/fbopen/)
* [OpenFDA example query](https://open.fda.gov/api/reference/#example-query)
* [Sunlight Congress API methods](https://sunlightlabs.github.io/congress/#using-the-api)

### Media Types

#### Use JSON

[JSON](https://en.wikipedia.org/wiki/JSON) is an excellent, widely supported
transport format, suitable for many web APIs.

Supporting JSON and only JSON is a practical default for APIs, and generally
reduces complexity for both the API provider and consumer.

General JSON guidelines:

* Responses should be **a JSON object** (not an array). Using an array to
return results limits the ability to include metadata about results, and limits
the API's ability to add additional top-level keys in the future.
* **Don't use unpredictable keys**. Parsing a JSON response where keys are
unpredictable (e.g. derived from data) is difficult, and adds friction for
clients.
* **Use consistent case for keys**. Whether you use `under_score` or
`CamelCase` for your API keys, make sure you are consistent.

#### Use UTF-8

Always [use UTF-8](http://utf8everywhere.org).

Expect accented characters or "smart quotes" in API output, even if they're not
expected.

An API should tell clients to expect UTF-8 by including a charset notation in
the `Content-Type` header for responses.

An API that returns JSON should use:

```
Content-Type: application/json; charset=utf-8
```

#### Use a consistent date format

And specifically, [use ISO 8601](https://xkcd.com/1179/), in UTC.

For just dates, that looks like `2013-02-27`. For full times, that's of the
form `2013-02-27T10:00:00Z`.

This date format is used all over the web, and puts each field in consistent
order -- from least granular to most granular.


### API Keys

These standards do not take a position on whether or not to use API keys.

But _if_ keys are used to manage and authenticate API access, the API should
allow some sort of unauthenticated access, without keys.

This allows newcomers to use and experiment with the API in demo environments
and with simple `curl`/`wget`/etc. requests.

Consider whether one of your product goals is to allow a certain level of
normal production use of the API without enforcing advanced registration by
clients.


### Error handling

Errors should always respond with the appropriate  HTTP status code. Responses
with error details should use a `4XX` status code to indicate a client-side
failure (such as invalid authorization, or an invalid parameter), and a `5XX`
status code to indicate server-side failure (such as an uncaught exception).

Handle all errors (including otherwise uncaught exceptions) and return a data
structure in the same format as the rest of the API.

For example, a JSON API might provide the following when a bad request is made:

```json
{
  "code": 400,
  "message": "Bad Request"
}
```

### Pagination

If pagination is required to navigate datasets, use the method that makes the
most sense for the API's data.

Some general recommendations around pagination:

1. Allow the API consumer to specify page sizes/limits and the offset/starting
point for a page.
2. Provide a link in a paginated response to the next page with a similar
size/limit.
3. Allow API consumers to assume that a paginated response without a link to
the next page is the last page.

### Always use HTTPS

Any new API should use and require [HTTPS encryption](https://en.wikipedia.org/wiki/HTTP_Secure) (using TLS/SSL). HTTPS
provides:

* **Security**. The contents of the request are encrypted across the Internet.
* **Authenticity**. A stronger guarantee that a client is communicating with
the real API.
* **Privacy**. Enhanced privacy for apps and users using the API. HTTP headers
and query string parameters (among other things) will be encrypted.
* **Compatibility**. Broader client-side compatibility. For CORS requests to
the API to work on HTTPS websites -- to not be blocked as mixed content --
those requests must be over HTTPS.

HTTPS should be configured using modern best practices, including ciphers that
support [forward secrecy](https://en.wikipedia.org/wiki/Forward_secrecy), and [HTTP Strict Transport Security](https://en.wikipedia.org/wiki/HTTP_Strict_Transport_Security). **This is not exhaustive**:
use tools like [SSL Labs](https://www.ssllabs.com/ssltest/analyze.html) to evaluate an API's HTTPS
configuration.


### CORS

For an API to be used from a website hosted at a different domain, the API must
[enable CORS](http://enable-cors.org).

For read-only and unauthenticated use cases, where the entire API should be
accessible from inside the browser, enabling CORS can be accomplished by
including this HTTP header in all responses:

```
Access-Control-Allow-Origin: *
```

It's supported by [every modern browser](http://enable-cors.org/client.html),
and will Just Work in many JavaScript clients, like
[jQuery](https://jquery.com).

For more advanced configuration, see the [W3C spec](http://www.w3.org/TR/cors/)
or [Mozilla's guide](https://developer.mozilla.org/en-US/docs/Web/HTTP/Access_control_CORS).

**What about JSONP?**

JSONP is [not secure or performant](https://gist.github.com/tmcw/6244497). If
IE8 or IE9 must be supported, use Microsoft's
[XDomainRequest](http://blogs.msdn.com/b/ieinternals/archive/2010/05/13/xdomainrequest-restrictions-limitations-and-workarounds.aspx?Redirected=true) object
instead of JSONP. There are [libraries](https://github.com/mapbox/corslite) to
help with this.


## Public domain

This project is in the public domain within the United States, and
copyright and related rights in the work worldwide are waived through
the [CC0 1.0 Universal public domain dedication](https://creativecommons.org/publicdomain/zero/1.0/).

All contributions to this project will be released under the CC0
dedication. By submitting a pull request, you are agreeing to comply
with this waiver of copyright interest.
