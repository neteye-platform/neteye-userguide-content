Run the following cluster command::

    # pcs status

and please ensure that:

#. Only **the last (N) node MUST** be active
#. All cluster resources are marked “Started” on the last (N) node
#. All cluster services under “Daemon Status” are marked active/enabled
   on the last (N) node
