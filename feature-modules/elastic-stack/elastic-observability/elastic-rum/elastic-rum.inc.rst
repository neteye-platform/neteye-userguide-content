Real User Monitoring (RUM)
~~~~~~~~~~~~~~~~~~~~~~~~~~

Real User Monitoring captures user interaction with clients such as web browsers.
The `JavaScript Agent <https://www.elastic.co/guide/en/apm/agent/rum-js/5.x/index.html>`__ is Elastic's RUM Agent.

Unlike Elastic APM backend agents which monitor requests and responses, the RUM JavaScript agent
monitors the real user experience and interaction within your client-side application.
The RUM JavaScript agent is also framework-agnostic, which means it can be used with
any front-end JavaScript application.

You will be able to measure metrics such as "Time to First Byte", ``domInteractive``,
and ``domComplete`` which helps you discover performance issues within your client-side application
as well as issues that relate to the latency of your server-side application.
