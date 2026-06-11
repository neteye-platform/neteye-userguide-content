The Audit Log Database Structure
--------------------------------

The Audit Log module relies on a simple but secure database structure
that ensures log integrity.

The following diagram shows the structure of the Audit Log database,
where ``id`` is the primary key.


.. csv-table:: Audit Log Database Structure (audit_log)
   :header: "Field", "Type", "Description"

   "id", "bigint(20)", "The primary, unique key that distinguishes an
   Audit Log record from the others."
   "module_name", "varchar(64)", "The name of the module (Log Manager,
   Geo Map, etc.) that originated the action."
   "obj_type", "varchar(64)", "The type of the object subjected to the action."
   "obj_name", "varchar(255)", "The name of the object."
   "action_name", "varchar(255)", "The type of action performed by the
   user (create, modify, delete or deploy)."
   "old_properties", "text", "A complete copy of the properties of the
   object before the update."
   "new_properties", "text", "The object's new properties after the
   update."
   "user", "varchar(64)", "The user name of the user who performed the
   action."
   "url", varchar(255)", "A URL pointing to the object (the old object
   for a delete action, the new object for an add action, etc.)"
   "message", "text", "An optional field containing a custom message
   (potentially including HTML markup) that replaces the action,
   object type, and URL fields shown in the table row."
   "change_time", "timestamp", "The Unix timestamp when the action was
   performed."
   "checksum", "varbinary(20)", "The chained SHA-1 value calculated
   from all the other fields in this database row, including the
   parent checksum."
   "parent_checksum", "varbinary(20)", "The SHA-1 checksum of the
   previous row in the table (the action before this one), used to
   create the chained checksum."
