# martian
Build routes on your Clojure/Clojurescript/Java client to a Swagger API and avoid hard coded routes

## Example
Given a [Swagger API definition](https://pedestal-api.herokuapp.com/swagger.json)
like that provided by [pedestal-api](https://github.com/oliyh/pedestal-api):

### Clojure / ClojureScript
```clojure
(require '[martian.core :as martian]
         '[clj-http.client :as http])

(let [api-root "https://pedestal-api.herokuapp.com"
      swagger-spec (:body (http/get (str api-root "/swagger.json") {:as :json}))
      url-for (martian/bootstrap api-root swagger-spec)]

  (url-for :get-pet {:id 123}))

;; => https://pedestal-api.herokuapp.com/pets/123
```

## Java

```java
import martian.Martian;
import java.util.Map;
import java.util.HashMap;

Map<String, Object> swaggerSpec = { ... };
Martian martian = new Martian("https://pedestal-api.herokuapp.com", swaggerSpec);

martian.urlFor("get-pet", new HashMap<String, Object> {{ put("id", 123); }});

// => https://pedestal-api.herokuapp.com/pets/123
```

## Caveats
- You need `:operationId` in the Swagger spec to name routes
  - [pedestal-api](https://github.com/oliyh/pedestal-api) automatically generates these from the interceptor name

## Development
[![Circle CI](https://circleci.com/gh/oliyh/martian.svg?style=svg)](https://circleci.com/gh/oliyh/martian)

Step in to the Clojurescript REPL as follows:
```clojure
(cemerick.piggieback/cljs-repl (cljs.repl.rhino/repl-env))
```

## Acknowledgements
martian uses [tripod](https://github.com/frankiesardo/tripod) for routing.
See also [kekkonen](https://github.com/metosin/kekkonen) for ideas integrating server and client beyond Swagger.