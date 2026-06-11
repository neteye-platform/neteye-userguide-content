.. _tornado-matcher-details:

Matcher Engine Implementation Details
+++++++++++++++++++++++++++++++++++++

The Matcher crate contains the core logic of the Tornado Engine. It is
in charge of:

- Receiving events from the Collectors
- Processing incoming events
- Detecting which Filters, Iterators and Rules an event matches
- Triggering expected actions

Due to its strategic position, its performance is of utmost importance for
global throughput. A matcher is stateless and thread-safe, thus a single
instance can be used to serve the entire application load.

At a very high level view, when the matcher initializes, it follows
these steps:

- Configuration (see the code in the "config" module): The configuration phase
  loads a set of files from the file system. Each file is either a Filter, a
  Iterator or a Rule, defined in the JSON format. The loading generates an
  internal structure representing the hierarchy from the filesystem.
- Validation (see the code in the "validator" module): The Validator receives
  the Processing Tree configuration and verifies that all nodes respect a set
  of predefined constraints (e.g., the identifiers cannot contain dots).
- Match Preparation (see the code in the "matcher" module): The Matcher
  receives the Processing Tree configuration, and for each node:

  - if the node is a Filter:

    - Builds the Accessors for accessing the event properties using the
      AccessorBuilder (see the code in the "accessor" module).
    - Builds an Operator for evaluating whether an event matches the Filter
      itself (using the OperatorBuilder, code in the "operator" module).
    - Builds all the child nodes.

  - if the node is an Iterator:

    - Builds the accessor expression, defined in the target.
    - Builds all the child nodes.

  - if the node is a rule:

    - Builds the Accessors for accessing the event properties using
      AccessorBuilder (see the code in the "accessor" module).
    - Builds the Operator for evaluating whether an event matches the
      "WHERE" clause of the rule (using the OperatorBuilder, code in the
      "operator" module).
    - Builds the Extractors for generating the user-defined variables using the
      ExtractorBuilder (see the code in the "extractor" module).

-  Listening: Listen for incoming events and then match them against the
   stored Filters, Iterators and Rules.
