
El Proxy Security
-----------------

The process of log management provided within NetEye grants
data integrity and inalterability due to the feature implementation
methods, in order to comply with standard NetEye policies.

El Proxy Security section should serve as a proof of El Proxy being
a reliable tool for signing your logs and safely sending them to Elasticsearch.
The implementation of El Proxy encounters risks mitigations, so that
the data corruption is preventable, or on the other hand, traceable.

The main aspects of security within mentioned software are covered in:

* Secured methods of communication and authentication
* Log signature flow, explaining the logic of how El Poxy actually works
* Error handling strategies El Proxy uses to recover after failures
* Healthchecks that are run to make sure log collecting process wasn't error-prone

Additionally, this section highlights some details of the
configuration and can be used to modify the setup if required.

.. include:: el-proxy-security/secure-communication.inc.rst
.. include:: el-proxy-security/authentication-to-elasticsearch.inc.rst
.. include:: el-proxy-security/log-signature-flow.inc.rst
.. include:: el-proxy-security/error-handling.inc.rst
.. include:: el-proxy-security/health-checks.inc.rst
.. include:: el-proxy-security/blockchain-verification.inc.rst
