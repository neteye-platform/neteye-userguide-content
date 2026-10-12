.. _satayo-mcp-server:

SATAYO MCP Server
~~~~~~~~~~~~~~~~~

.. warning::

   This is an :ref:`experimental feature <satayo-experimental-features>`: it
   can be changed or removed at any time, without prior notice.

The **SATAYO MCP Server** lets you connect an AI assistant, such as Claude Code,
to SATAYO, so that you can ask questions about your findings in natural
language, for example *"Which exposed credentials were found for my domains
in the last month?"*. The assistant retrieves the answer directly from
SATAYO.

The server uses the `Model Context Protocol (MCP)
<https://modelcontextprotocol.io>`__, an open standard supported by many AI
assistants and tools.

Key points:

* **Read-only**: the assistant can read your SATAYO data, but it cannot
  create, modify or delete anything.
* **Your own permissions**: you sign in with your SATAYO account, and the
  assistant sees exactly what you see in the SATAYO web interface, nothing
  more.
* **No secret to manage**: you do not need an API key or a client secret.
  Signing in happens in your browser, on the usual SATAYO login page.

Requirements
============

To use the SATAYO MCP Server you need:

* a SATAYO account, the same you use to log in to the SATAYO web interface;
* an AI assistant that supports remote MCP servers over the **Streamable
  HTTP** transport, with **OAuth** authentication, and **version 2026-07-28**
  of the MCP specification. Older clients that support only earlier versions
  of the specification cannot connect, so keep your assistant up to date.

Connection Settings
===================

These are the only settings you need to configure in your AI assistant.
Replace ``<satayo-host>`` with the host name of your SATAYO instance.

.. list-table::
   :widths: 30 70
   :header-rows: 1

   * - Setting
     - Value
   * - **Server URL**
     - ``https://<satayo-host>/satayo/api/mcp/v0``
   * - **Transport**
     - Streamable HTTP (often called just *HTTP*)
   * - **Authentication**
     - OAuth
   * - **OAuth Client ID**
     - ``satayo2-mcp-client``
   * - **OAuth Client Secret**
     - None, leave it empty
   * - **OAuth Callback URL**
     - Any address on ``http://localhost``, on any port

Most assistants find all the other information, such as the address of the
SATAYO login page, automatically.

.. note::

   The OAuth Client ID is required because SATAYO does not support the
   automatic registration of new clients. If your assistant does not let you
   set a Client ID, it cannot connect to SATAYO.

Example: Claude Code
====================

This example shows how to connect `Claude Code
<https://docs.claude.com/en/docs/claude-code/overview>`__ to SATAYO. Make
sure you are using a recent version of Claude Code.

Step 1: Add the Server
----------------------

Run the following command in a terminal, replacing ``<satayo-host>`` with the
host name of your SATAYO instance:

.. code-block:: bash

    claude mcp add --transport http satayo \
      https://<satayo-host>/satayo/api/mcp/v0 \
      --client-id satayo2-mcp-client \
      --callback-port 7777

The options mean:

* ``satayo``: the name you give to the server in Claude Code. You can choose
  any name.
* ``--client-id satayo2-mcp-client``: the OAuth Client ID to use for signing
  in.
* ``--callback-port 7777``: the local port where Claude Code receives the
  result of the login. You can use any free port.

By default the server is available only to you, and only in the current
directory. To make it available in all your directories, add
``--scope user`` to the command.

Step 2: Sign In
---------------

Start Claude Code and type:

.. code-block:: text

    /mcp

Select the ``satayo`` server and choose to authenticate. Your browser opens
the SATAYO login page: log in with your SATAYO account as usual. When the
browser confirms that the authentication succeeded, go back to Claude Code.

You need to sign in only once: Claude Code keeps the connection active and
renews it automatically. If the session expires, run ``/mcp`` again and sign
in.

Step 3: Check the Connection
----------------------------

Run the following command in a terminal:

.. code-block:: bash

    claude mcp get satayo

If everything works, the output contains ``Status: ✔ Connected``. You can
also type ``/mcp`` in Claude Code and select the ``satayo`` server to see the
list of available tools.

Step 4: Ask Your Questions
--------------------------

You can now ask Claude Code about your SATAYO data in natural language. For
example:

* *"Use SATAYO to tell me which organizations and monitored domains I can
  see."*
* *"Show me the open credential findings in SATAYO, most recent first."*
* *"Are there any SATAYO findings related to the domain example.com?"*
* *"Summarize the SATAYO attack surface composition and tell me which
  findings I should look at first."*

The first time Claude Code uses a SATAYO tool, it asks for your permission.
You can allow it once or for the whole session.

Sharing the Configuration with Your Team
----------------------------------------

To share the configuration with your team, create a file named
``.mcp.json`` in the root folder of your project, with the following
content:

.. code-block:: json

    {
      "mcpServers": {
        "satayo": {
          "type": "http",
          "url": "https://<satayo-host>/satayo/api/mcp/v0",
          "oauth": {
            "clientId": "satayo2-mcp-client",
            "callbackPort": 7777
          }
        }
      }
    }

The file does not contain any credential, so you can safely commit it to your
repository. Each team member signs in with their own SATAYO account, as
described in `Step 2: Sign In`_, and sees only the data they are allowed to
see.

Other AI Assistants
===================

Any AI assistant that meets the `Requirements`_ can connect to SATAYO using
the `Connection Settings`_. The exact steps depend on the assistant: please
refer to its documentation on how to add a remote MCP server with OAuth
authentication.

To try the server without an AI assistant, you can use the `MCP Inspector
<https://github.com/modelcontextprotocol/inspector>`__, a tool that lets you
call the server manually:

.. code-block:: bash

    npx @modelcontextprotocol/inspector

In the MCP Inspector, select the **Streamable HTTP** transport, enter the
**Server URL** and the **OAuth Client ID** from the `Connection Settings`_,
and connect.

Troubleshooting
===============

.. list-table::
   :widths: 40 60
   :header-rows: 1

   * - Problem
     - Solution
   * - The assistant reports that it needs authentication, or answers
       with ``401``
     - You are not signed in, or your session expired. In Claude Code, run
       ``/mcp`` and sign in again.
   * - The SATAYO login page shows ``Invalid parameter: redirect_uri``
     - The callback URL of your assistant is not on ``localhost``.
       Configure the assistant to use a callback URL on ``http://localhost``.
   * - The assistant reports ``Unsupported protocol version``
     - Your assistant does not support version 2026-07-28 of the MCP
       specification. Update it to the latest version.
   * - The assistant cannot register a client, or asks for a Client ID
     - Set the OAuth Client ID ``satayo2-mcp-client``, as described in
       `Connection Settings`_.
   * - Every question returns no results
     - Ask the assistant to call the ``whoami`` tool to check which
       organizations and monitored domains you can see.
