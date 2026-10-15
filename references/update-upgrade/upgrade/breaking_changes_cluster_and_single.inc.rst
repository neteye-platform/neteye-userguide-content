|ne| Kubernetes Operator
~~~~~~~~~~~~~~~~~~~~~~~~

This release will introduce a brand new |ne| Operator which is the component responsible for managing the |ne|
deployment in Kubernetes. All configuration and management is made by the standard |ne| CLI procedures such as
:command:`neteye install` and :command:`neteye upgrade`, but you can configure and override the default configuration by
editing the :file:`/etc/neteye-environment.yaml` file. |ne| CLI procedures will automatically source this configuration
file and apply desired changes to the Operator. This upgrade does not require users to update the configuration
beforehand since default values will be applied. For the full list of configurable parameters, please refer to the
:file:`/usr/share/neteye/setup/neteye-environment.yaml.tpl` file. To apply the configuration changes during normal
operation, you can use the :command:`neteye install --restrict-services-to neteye-operator` command. This command will
apply the configuration changes to the Operator without affecting the other services.
