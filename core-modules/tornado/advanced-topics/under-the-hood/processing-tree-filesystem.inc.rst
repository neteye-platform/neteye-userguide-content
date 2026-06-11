.. _tornado-processing-tree-conf:

Processing Tree Configuration
+++++++++++++++++++++++++++++

.. note::

  The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL
  NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED",  "MAY", and
  "OPTIONAL" in this document are to be interpreted as described in
  `RFC 2119 <https://www.rfc-editor.org/rfc/rfc2119>`_.

The location of configuration files in the file system is pre-configured in
NetEye. NetEye automatically starts Tornado as follows:

* Reads the configuration from the :file:`/neteye/shared/tornado/conf`
  directory
* Loads and validates the Matcher configuration from the directory
  :file:`/neteye/shared/tornado/conf/rules.d`
* Starts the Tornado Engine

The hierarchy of the subdirectories in the rules directory defines the structure
of the Processing Tree, where each directory is a node in the Processing Tree.
Each directory can be one of the following nodes:

* A *Filter*
* A *Iterator*
* A *Ruleset*

Each node type MUST contain a JSON file that contains the node data in a
defined format, as well as subdirectories that contain the node's children.

.. hint:: Tornado will ignore all other file types.

Any Processing Node directory MUST NOT contain any JSON files, other than the
one needed for the node definition. If another JSON file is present, the
validation will fail and the Tornado Engine will fail to start up.

The names of all child nodes of a filter or iterator, as well as the names of
all rules in a ruleset MUST be unique and MUST NOT contain any characters,
other than letters, numbers and "_" (underscore).

1. **Loading the config version:** First of all, Tornado will try to load the
   file :file:`version.json`, which should contain the field *version* that
   defines the current config version. If the version is too old, please execute
   :command:`sudo -u tornado tornado rules-upgrade`.

2. **Loading the Processing Tree Nodes:** If the version matches the latest
   config version, Tornado will load all the subdirectories as Processing Tree
   nodes.

3. **Loading a Filter:** A filter directory MUST contain a file called
   :file:`filter.json` that contains the definition of the filter. The filter
   MAY contain subdirectories defining the filter's child nodes. The definition
   MUST NOT contain any other fields as specified below. A filter file MUST
   contain the following fields:

   1. *type:* The type always needs to be :command:`"filter"`
   2. *name:* The name of the filter node. It SHOULD be the same as the
      directory name.
   3. *description:* A quick description of the filter node. It MAY be just an
      empty string.
   4. *active:* If this field is set to :command:`false`, the Processing Node
      will still be validated, but will not be part of the Processing Tree.
   5. *filter:* The filter field MAY contain an :ref:`Operator <where-conditions>`
      otherwise it MUST be an empty object.

4. **Loading an Iterator:** An Iterator directory MUST contain a file called
   :file:`iterator.json` that describes the iterator. The iterator directory
   MAY contain subdirectories defining the iterators child nodes. An Iterator
   node MUST NOT have another iterator node as its child. The definition MUST
   NOT contain any other fields as specified below. An Iterator file MUST
   contain the following fields:

   1. *type:* The type always needs to be :command:`"iterator"`
   2. *name:* The name of the iterator node. It SHOULD be the same as the
      directory name.
   3. *description:* A quick description of the iterator node. It MAY be just
      an empty string.
   4. *active:* If this field is set to :command:`false`, the Processing Node
      will still be validated, but will not be part of the Processing Tree.
   5. *target:* The target field MUST be a valid accessor expression.

5. **Loading a Ruleset:**
   A ruleset directory MUST contain a file called :file:`ruleset.json`.
   Furthermore it MUST contain a directory :file:`rules`, that contains all
   the rule definitions for the ruleset. The rules will be loaded from the
   filesystem in lexicographic order. The ruleset file MUST contain only the
   following fields:

   1. *type:* The type always needs to be :command:`"ruleset"`
   2. *name:* The name of the iterator node. It SHOULD be the same as the
      directory name.

6. **Loading a Rule:**
   A rule file MAY only exist in the :file:`rules` directory of a ruleset node.
   It MUST only contain the following fields:

   1. *name:* The name of the iterator node. It SHOULD be the same as the
      directory name.
   2. *description:* A quick description of the iterator node. It MAY be just
      an empty string.
   3. *continue:* Defines, whether the processing of the rules should continue
      if the current rule was matched.
   4. *active:* If this field is set to :command:`false`, the Processing Node
      will still be validated, but will not be part of the Processing Tree.
   5. *constraint:* The field constraint is an object and MUST only contain the
      following fields:

      1. *WHERE:* This field MAY contain an :ref:`Operator <where-conditions>` or
         else be left out.
      2. *WITH:* This field MUST contain a JSON object with value names as a
         key and an :ref:`Extractor <tornado-with-clause>` as a value. The object
         MAY be left empty but MUST be present in the constraint definition.

   6. *actions:* The action field MUST be an array, containing a number of
      actions to perform. The array MAY be empty, but MUST NOT be left out of
      the rule definition. An action MUST have the following fields:

      1. *id:* The id MUST be a string that defines the executor, which will
         process the action.
      2. *payload:* The payload for the executor. This MUST be a JSON Object.

The directory names for the nodes MUST NOT be relied upon when interacting with
the configuration on the filesystem and are considered implementation details.
Directory names MAY change during a deploy or a rule migration.


For example, consider this directory structure:

.. code-block::

  /neteye/shared/tornado/conf/rules.d/
    ├ version.json
    └ master/
      ├ filter.json
      │ └ master_iterator/
      │   ├ iterator.json
      │   ├ iterator_ruleset/
      │   │ ├ ruleset.json
      │   │ └ rules/
      │   │   ├ 0000000010_archive.json
      │   │   └ 0000000020_director.json
      │   └ splitter_child_ruleset_node1/
      │     ├ ruleset.json
      │     └ rules/
      └ master_child_filter/
        └ filter.json


When Tornado is loading this configuration, the Processing Tree will be organized as
follows:

* The root node will have one child filter that is defined in the file
  :file:`master/filter.json`

* The node :file:`rules.d/master` is a :command:`filter` node with two child
  nodes:

  * The node :file:`rules.d/master/master_iterator` is an :command:`iterator`
    with one child.

    * The node :file:`rules.d/master/master_iterator/iterator_ruleset` is a
      :command:`ruleset` containing two rules.

  * The node :file:`rules.d/master/master_child_filter` is a :command:`filter`
    node without any children.
