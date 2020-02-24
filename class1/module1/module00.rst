===============
Getting Started
===============

Lab Components
==============

The following table lists the virtual appliances in the lab along with their networks and credentials to use.

.. list-table::
    :widths: 20 40 40
    :header-rows: 1
    :stub-columns: 1

    * - **System Type**
      - **Networks**
      - **Credentials**

    * - NGINX+
      - Management: 10.1.1.4
        Internal: 10.1.10.11
      - SSH keys
    * - MyApplication Server
      - Management: 10.1.1.8
        Internal: 10.1.10.21
      - None
    * - NGINX Controller
      - Management: 10.1.1.7
      - SSH      


Starting the Lab
================

*Insert instructions here to access UDF*

Using NGINX with Docker
============

.. code-block:: shell

  docker run -i -t nginx /usr/bin/njs

.. code-block:: none

  interactive njs 0.2.4

  v.<Tab> -> the properties and prototype methods of v.
  type console.help() for more information

  >> function hi(msg) {console.log(msg)}
  undefined
  >> hi("Hello world")
  'Hello world'
  undefined

Local Setup for Labs
=====================

.. code-block:: shell

  git clone https://github.com/xeioex/njs-examples
  cd njs-examples
  
  EXAMPLE=hello
  docker run --rm --name njs_example  -v $(pwd)/conf/$EXAMPLE.conf:/etc/nginx/nginx.conf:ro  -v $(pwd)/njs/$EXAMPLE.js:/etc/nginx/example.js:ro -p 80:80 -p 8090:8090 -d nginx

  # Stopping.
  docker stop njs_example


