
Authentication via IdP
~~~~~~~~~~~~~~~~~~~~~~

The Cloud platform supports authentication through external **Identity Providers (IdPs)**.
An Identity Provider is a system responsible for verifying user identities and issuing
authentication tokens that grant access to NetEye.Cloud services. The platform is designed
to support multiple Identity Providers.

The NetEye.Cloud platform does not directly manage user passwords.
Authentication is delegated to your configured Identity Provider.

During login, you should access the NetEye.Cloud Login Page and provide your
email address in the authentication prompt. Based on the email domain,
the platform automatically identifies the appropriate Identity Provider
and redirects the authentication request accordingly.

The authentication itself takes place entirely within your Identity
Provider environment. Once authentication is successfully completed, the IdP
issues a token that is validated by the NetEye.Cloud platform.

Before the first login you must:

- Configure your Identity Provider to allow authentication with the NetEye.Cloud platform.
- Provide the necessary configuration parameters to the NetEye.Cloud Team.
- Allow the NetEye.Cloud Team to complete the Identity Provider setup within the Cloud environment.

This configuration activity is typically performed once during the onboarding phase.
After the integration is completed, you can authenticate using your existing corporate credentials
without any additional NetEye.Cloud-specific passwords.
