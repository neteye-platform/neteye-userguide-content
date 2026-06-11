
Access to NetEye and Elastic
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The **SOC ADS service** monitors all authentication events in your company through the Elastic SIEM
and enriches the events with a variable `user.is_admin` when it comes to login, logout, or failed login
operations performed by system administrators.

How to access the service
`````````````````````````

Elastic has been integrated into **NetEye Cloud** and the IP of a customer has been allowed to connect.

To access and view the data contained therein, follow these instructions:

 - Connect to the cloud from the following address: `NetEye Cloud <https://neteye.cloud/neteye/authentication/login>`__
 - You will be faced with a login page similar to this (the background image changes depending on the NetEye version):

.. figure:: /neteye-cloud/soc-ads/img/login-screen.png

After logging in, Elastic will be accessible through the Log Analytics entry

.. figure:: /neteye-cloud/soc-ads/img/log-analytics.png

Each time you log in, you will be presented with the following Elastic page, called Welcome Tenant Dashboard.

.. figure:: /neteye-cloud/soc-ads/img/welcome-tenant.png

   Welcome Tenant Dashboard.

It contains three different links to as many :ref:`dashboards <soc-elastic-dashboards>` created to facilitate the viewing of the logs.
