Lab 4 - Subrequests join
================
Combining the results of several subrequests asynchronously into a single JSON reply.

nginx.conf:

.. code-block:: nginx

  ...

  http {
      js_include example.js;

      server {
            listen 80;

            location /join {
                js_content join;
            }

            location /foo {
                proxy_pass http://localhost:8080;
            }

            location /bar {
                proxy_pass http://localhost:8090;
            }
      }
 }

example.js:

.. code-block:: js

  function join(r) {
      join_subrequests(r, ['/foo', '/bar']);
  }

  function join_subrequests(r, subs) {
      var parts = [];

      function done(reply) {
          parts.push({ uri:  reply.uri,
                       code: reply.status,
                       body: reply.responseBody });

          if (parts.length == subs.length) {
              r.return(200, JSON.stringify(parts));
          }
      }

      for (var i in subs) {
          r.subrequest(subs[i], done);
      }
  }

Checking:

.. code-block:: shell

  curl http://localhost/join
  [{"uri":"/foo","code":200,"body":"FOO"},{"uri":"/bar","code":200,"body":"BAR"}]


