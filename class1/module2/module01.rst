Lab 1 - Implementing NJS on NGINX+
==================================

Decode URI
===========

nginx.conf:

.. code-block:: nginx

  ...

  http {
      js_include example.js;

      js_set $dec_foo dec_foo;

      server {
  ...
            location /foo {
                return 200 $arg_foo;
            }

            location /dec_foo {
                return 200 $dec_foo;
            }
      }
  }

example.js:

.. code-block:: js

  function dec_foo(r) {
    return decodeURIComponent(r.args.foo);
  }

Checking:

.. code-block:: shell

  curl -G http://localhost/foo --data-urlencode "foo=привет"
  %D0%BF%D1%80%D0%B8%D0%B2%D0%B5%D1%82

  curl -G http://localhost/dec_foo --data-urlencode "foo=привет"
  привет


