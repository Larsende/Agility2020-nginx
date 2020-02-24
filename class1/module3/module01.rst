Lab 3 - Injecting HTTP header using stream proxy
========================================

nginx.conf:

.. code-block:: nginx

  ...

  stream {
      js_include example.js;

      server {
            listen 80;

            proxy_pass 127.0.0.1:8080;
            js_filter inject_header;
      }
  }

  ...

example.js:

.. code-block:: js

    function inject_header(s) {
        inject_my_header(s, 'Foo: my_foo');
    }

    function inject_my_header(s, header) {
        var req = '';

        s.on('upload', function(data, flags) {
            req += data;
            var n = req.search('\n');
            if (n != -1) {
                var rest = req.substr(n + 1);
                req = req.substr(0, n + 1);
                s.send(req + header + '\r\n' + rest, flags);
                s.off('upload');
            }
        });
    }

Checking:

.. code-block:: shell

  curl http://localhost/
  my_foo


