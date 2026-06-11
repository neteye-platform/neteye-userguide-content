.. _neteye-sso:

Module Permissions and Single Sign On Within NetEye
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

NetEye modular architecture allows to integrate third-party modules to
increase its functionalities. However, accessing some module in NetEye
may require a dedicated user, proper of the module. This situation
therefore implies that to carry out some activities within NetEye
different users are required: one for NetEye itself, one for the module.

In order to ease access to all third-party modules installed on NetEye
4, the `Single Sign On <https://en.wikipedia.org/wiki/Single_sign-on>`__
(**SSO**) functionality has been implemented for a few modules and will
be soon extended to other. This chapter describes how module permissions
work in NetEye and then presents the SSO’s underlying logic.

Users, Roles, and Module Permissions
````````````````````````````````````

In this section we define and quickly outline the main properties and
interaction of Users, roles, and permissions on modules.

1. A **User** is an account that allows to login to NetEye and can have
   different *Roles*.

2. A **Role** is a set of permissions on one or multiple modules.

3. A **Permission** on a module is a rule that define the level of
   access to a module. Modules offer usually different levels of access;
   there are however two *permissions* common to every module:

   -  **Full Module Access** grants administrative access to the module.
      In other words, every functionality of the module is available.
   -  **General Module Access** permits only to see the module in the
      menu, but is a requirement to assign other permissions, if
      available for that module.

4. A **Restrictions** allow to specify one or more subsets of either
   *allowed actions*, or *accessible entities* for the Role.

   Except these two permissions *Full Module Access* and *General Module
   Access*, each module can have extra **permissions** and
   **restrictions**. Those are defined in their respective modules and
   explained how to use them.

.. note:: If a user has more than one Role, the “merging” behaviour is
   up to each module

Single Sign On Architecture
```````````````````````````

The SSO mechanism implemented in NetEye allows one to log in to NetEye
with a unique user, call it **john-acme**, and access all modules
configured for SSO, provided the role associated to *john-acme*, has the
corresponding permissions on those modules.

If *john-acme* has role with the permission to access the SSO-enabled
module, when first time he tries to access module from neteye, a new
profile for **john-acme** will be created automatically with defined
**permission** and **restriction** in the module.
