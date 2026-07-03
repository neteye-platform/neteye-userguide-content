.. _mitre-attack-coverage:

MITRE ATT&CK Coverage
~~~~~~~~~~~~~~~~~~~~~

.. rubric:: MITRE ATT&CK Framework

The **MITRE ATT&CK** framework is a curated knowledge base that
describes how cyber adversaries conduct attacks against information
systems. It models the different phases of an attack lifecycle and
documents the behaviors that attackers commonly use when compromising
and operating within computer networks.

ATT&CK focuses on real-world adversary behavior and organizes it into a
structured model of **tactics, techniques, and procedures (TTPs)**. At a
high level, the framework is composed of the following core elements:

- **Tactics** – the adversary’s tactical objectives during an attack
  (the *why* of an action).
- **Techniques** – the methods used by adversaries to achieve those
  objectives (the *how*).
- **Sub-techniques** – more specific variations of techniques that
  describe particular ways in which a technique may be implemented.

As mentioned in the :ref:`SATAYO Items<satayo-items>` page, every item
within SATAYO is mapped with the :command:`MITRE ATT&CK Framework`.

The ATT&CK framework documents known attacker behaviors but is not
intended to serve as a checklist of actions that must always be
detected. A single technique can be implemented through many different
procedures, and these procedures may evolve as adversaries change their
methods over time.


.. rubric:: ATT&CK Coverage

The concept of **ATT&CK coverage** refers to the extent to which an
organization’s security capabilities can detect, analyze, or respond to
the adversary behaviors described in the MITRE ATT&CK framework. By
mapping security controls, monitoring activities, and intelligence
outputs to specific ATT&CK techniques, it becomes possible to understand
which parts of the attack lifecycle are currently covered and where
visibility or detection gaps may exist. The ATT&CK Matrix provides an
overview of the ATT&CK techniques that are currently addressed within
our security operations processes.


.. rubric:: ATT&CK Matrix

The relationship between tactics, techniques, and sub-techniques is
commonly visualized through the **ATT&CK Matrix**, which organizes
attacker behaviors across the stages of an attack.

The following image shows the tactics and techniques covered by Würth IT Italy cyber security products.

.. image:: /neteye-cloud/satayo/mitre-attack/mitreCoverage.svg
   :align: center


Colors used are to be interpreted as follows:

+ BLUE: Area covered by SATAYO
+ PURPLE: Area covered by both SATAYO and Würth IT Italy's :abbr:`SOC (Security Operation Center)`
+ GREEN: Area covered by SOC detection rules

We recommend you open the image in another tab and zoom in to read it properly.
