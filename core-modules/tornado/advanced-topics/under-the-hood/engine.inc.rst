.. _tornado-engine-exec:


Tornado Engine (Executable)
+++++++++++++++++++++++++++

This crate contains the Tornado Engine executable code, which is a
configuration of the Engine based on `actix
<https://github.com/actix/actix>`__ and built as a portable
executable.

.. rubric:: Structure of Tornado Engine

This specific Tornado Engine executable is composed of the following
components:

-  A JSON Collector
-  The Engine
-  The Archive Executor
-  The Elasticsearch Executor
-  The Foreach Executor
-  The Icinga 2 Executor
-  The Director Executor
-  The Monitoring Executor
-  The Logger Executor
-  The Script Executor
-  The Smart Monitoring Executor

Each component is wrapped in a dedicated actix actor.

This configuration is only one of many possible configurations. Each
component has been developed as an independent library, allowing for
greater flexibility in deciding whether and how to use it.

At the same time, there are no restrictions that force the use of the
components into the same executable. While this is the simplest way to
assemble them into a working product, the Collectors and Executors could
reside in their own executables and communicate with the Tornado Engine
via a remote call. This can be achieved either through a direct TCP or
HTTP call, with an RPC technology (e.g., Protobuf, Flatbuffer, or
CAP'n'proto), or with a message queue system (e.g., Nats.io or Kafka) in
the middle for deploying it as a distributed system.
