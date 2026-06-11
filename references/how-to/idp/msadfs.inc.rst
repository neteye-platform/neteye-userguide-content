.. _idp-msadfs-saml:

How To setup Microsoft Active Directory Federation Services (MSADFS) in Keycloak with SAML
------------------------------------------------------------------------------------------

This guide explains how to configure Microsoft Active Directory Federation Services (MSADFS) as an Identity Provider (IdP) in Keycloak.

.. note::
  This guides assumes that you have access to the :ref:`Authentication Admin Console <auth-admin-console>` and to your MSADFS console.

.. note::
   You need the federation metadata endpoint URL from your MSADFS console. This URL is used to configure the IdP in Keycloak.
   Usually, the URL is in the format ``https://<ADFS_HOST>/FederationMetadata/2007-06/FederationMetadata.xml``.


.. _idp-msadfs-keycloak-configuration:

**Keycloak Configuration**

To configure MSADFS as an IdP in Keycloak, you need to go to the :ref:`Authentication Admin Console <auth-admin-console>` and follow these steps:

    * Go to :menuselection:`Identity Providers` > :menuselection:`Add provider` > :menuselection:`SAML v2.0`.
    * Select an alias for the IdP, for example, ``MSADFS``.
    * Insert the federation metadata endpoint URL from your MSADFS console
    * Click on :menuselection:`Save`.

.. note::
    Following configurations depends on your MSADFS setup. Please refer to your MSADFS documentation for more information.
    We recommend to set the following configuration parameters that usually work for most setups

* **NameID Policy Format**: ``Unspecified``
* **Want AuthnRequests Signed**: ``TRUE``
* **SAML Signature Key Name**: ``CERT_SUBJECT``
* **Validate Signature**: ``TRUE``
* **Use metadata descriptor URL**: ``TRUE``


Now we need to create a new mapper in :menuselection:`Mappers` with the following settings:

* **Name**: ``username``
* **Mapper Type**: ``Attribute Importer``
* **Attribute Name**: ``http://schemas.microsoft.com/2012/12/certificatecontext/field/subjectname``
* **User Attribute Name**: ``username``

.. note::
    The ``Attribute Name`` **MAY** be different for you, referer to your claim names, in this configuration we use the subject name.

At this point, you can complete the rest of the configuration with common settings, please refer to :ref:`Common IdP configuration <idp-common-config>` and :ref:`IdP After Setting up configuration <idp-after-config>` sections to find all the information you need.

.. _idp-msadfs-configuration:

**MSADFS Configuration**

Now you need to configure the Keycloak as a relying party in your MSADFS console.
You can follow this `guide <https://learn.microsoft.com/en-us/previous-versions/windows-server/it-pro/windows-server-2012/identity/ad-fs/operations/create-a-relying-party-trust#to-create-a-claims-aware-relying-party-trust-using-federation-metadata>`__ to add a new relying party in your MSADFS console:

.. note::
    You can use the **federation metadata** URL from Keycloak to automatically configure the ADFS relying party.
    You can find it in the **provider settings** in the :ref:`Identity Providers <idp-msadfs-keycloak-configuration>` section.

.. warning::
    If the metadata URL is not working, you can download the XML from keycloak at the same federation metadata URL and upload it in the MSADFS console.

After you have configured the relying party, you need to add the claim rules to map the user attributes to the SAML token.
You can follow this `guide <https://learn.microsoft.com/en-us/windows-server/identity/ad-fs/operations/create-a-rule-to-send-ldap-attributes-as-claims#to-create-a-rule-to-send-ldap-attributes-as-claims-for-a-relying-party-trust-in-windows-server-2016>`__ to map your LDAP attributes to SAML attributes.

**At the very minimum** you need to map the username to the SAML token.
In our how-to guide, we have mapped the **sAMAccountName** field in MSADFS as **Subject Name**, but it can be mapped depending on your setup.

.. csv-table::
    :header: "LDAP Attribute", "Outgoing Claim Type"

    "sAMAccountName", "Subject Name"

.. note::
    Remember to complete the rest of the configuration with common settings, please refer to :ref:`Common IdP configuration <idp-common-config>` and :ref:`IdP After Setting up configuration <idp-after-config>` sections to find all the information you need.
