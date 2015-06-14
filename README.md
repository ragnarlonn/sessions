
# Sessions

Package `sessions` provides minimalist Go sessions, backed by cookie or database stores.

### Features

* `Store` provides a predicatable interface for dealing with *individual* sessions.
    * `New` returns a new empty `Session`. 
    * `Get` returns the named `Session` from the `http.Request` iff it was correctly verified and decoded. Otherwise the error is non-nil.
    * `Save` encodes and signs Session.Value data into the persistent `Store`
    * `Destroy` removes the underlying session cookie.
* Each `Session` provides `Save` and `Destroy` convenience methods.
* Provides `CookieStore` for managing client-side secure cookies.
* Extensible for custom session database backends.
* No `gorilla/context` dependency. No need for contexts. No need to wrap handlers in `context.ClearHandler`.

### Differences from gorilla/sessions

* Gorilla stores a context map of Requests to Sessions to abstract multiple sessions. `dghubble/sessions` provides individual sessions, leaving multiple sessions to a `multisessions` package. No Registry is needed.
* Gorilla requires `gorilla/context` so all handlers must be wrapped in `context.ClearHandler` to avoid memory leaks.
* Gorilla's `Store` interface is surprising. `New` and `Get` can both possibly return a new session, a field check is needed. Some use cases expect developers to [ignore an error](https://github.com/gorilla/sessions/blob/master/doc.go#L32). `Destroy` isn't provided.

