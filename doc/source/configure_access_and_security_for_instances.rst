===========================================
Configure access and security for instances
===========================================

Before you launch an instance, you should add security group rules to
enable users to ping and use SSH to connect to the instance. Security
groups are sets of IP filter rules that define networking access and are
applied to all instances within a project. To do so, you either add
rules to the default security group :ref:`security_groups_add_rule`
or add a new security group with rules.

Key pairs are SSH credentials that are injected into an instance when it
is launched. To use key pair injection, the image that the instance is
based on must contain the ``cloud-init`` package. Each project should
have at least one key pair. For more information, see the section
:ref:`keypair_add`.

If you have generated a key pair with an external tool, you can import
it into OpenStack. The key pair can be used for multiple instances that
belong to a project. For more information, see the section
:ref:`dashboard_import_keypair`.

When an instance is created in OpenStack, it is automatically assigned a
fixed IP address in the network to which the instance is assigned. This
IP address is permanently associated with the instance until the
instance is terminated. However, in addition to the fixed IP address, a
floating IP address can also be attached to an instance. Unlike fixed IP
addresses, floating IP addresses are able to have their associations
modified at any time, regardless of the state of the instances involved.

.. _security_groups_add_rule:

Add a rule to the default security group
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This procedure enables SSH and ICMP (ping) access to instances. The
rules apply to all instances within a given project, and should be set
for every project unless there is a reason to prohibit SSH or ICMP
access to the instances.

This procedure can be adjusted as necessary to add additional security
group rules to a project, if your cloud requires them.

.. note::

   When adding a rule, you must specify the protocol used with the
   destination port or source port.

#. Log in to the dashboard, choose a project, and click :guilabel:`Access &
   Security`. The :guilabel:`Security Groups` tab shows the security groups
   that are available for this project.

#. Select the default security group and click :guilabel:`Edit Rules`.

#. To allow SSH access, click :guilabel:`Add Rule`.

#. In the :guilabel:`Add Rule` dialog box, enter the following values:

   +--------------------------------------+--------------------------------------+
   | Rule                                 | Remote                               |
   +--------------------------------------+--------------------------------------+
   | ``SSH``                              | ``CIDR``                             |
   +--------------------------------------+--------------------------------------+

   .. note::

      To accept requests from a particular range of IP addresses, specify
      the IP address block in the CIDR box.

#. Click :guilabel:`Add`.

   Instances will now have SSH port 22 open for requests from any IP
   address.

#. To add an ICMP rule, click :guilabel:`Add Rule`.

#. In the :guilabel:`Add Rule` dialog box, enter the following values:

   +--------------------------------------+--------------------------------------+
   | Rule                                 | Direction                            |
   +--------------------------------------+--------------------------------------+
   | ``All ICMP``                         | ``Ingress``                          |
   +--------------------------------------+--------------------------------------+

#. Click :guilabel:`Add`.

   Instances will now accept all incoming ICMP packets.

.. _keypair_add:

Add a key pair
~~~~~~~~~~~~~~

Create at least one key pair for each project.

#. Log in to the dashboard, choose a project, and click Access &
   Security.

#. Click the Keypairs tab, which shows the key pairs that are available
   for this project.

#. Click Create Keypair.

#. In the Create Keypair dialog box, enter a name for your key pair, and
   click Create Keypair.

#. Respond to the prompt to download the key pair.

.. _dashboard_import_keypair:

Import a key pair
~~~~~~~~~~~~~~~~~

#. Log in to the dashboard, choose a project, and click Access &
   Security.

#. Click the Keypairs tab, which shows the key pairs that are available
   for this project.

#. Click Import Keypair.

#. In the Import Keypair dialog box, enter the name of your key pair,
   copy the public key into the Public Key box, and then click Import
   Keypair.

#. Save the ``*.pem`` file locally.

#. To change its permissions so that only you can read and write to the
   file, run the following command:

   .. code:: bash

       $ chmod 0600 yourPrivateKey.pem

   .. note::

      If you are using the dashboard from a Windows computer, use PuTTYgen
      to load the ``*.pem`` file and convert and save it as ``*.ppk``. For
      more information see the `WinSCP web page for
      PuTTYgen <http://winscp.net/eng/docs/ui-puttygen>`__.

#. To make the key pair known to SSH, run the **ssh-add** command.

   .. code:: bash

       $ ssh-add yourPrivateKey.pem

The Compute database registers the public key of the key pair.

The dashboard lists the key pair on the Access & Security tab.

Allocate a floating IP address to an instance
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

When an instance is created in OpenStack, it is automatically assigned a
fixed IP address in the network to which the instance is assigned. This
IP address is permanently associated with the instance until the
instance is terminated.

However, in addition to the fixed IP address, a floating IP address can
also be attached to an instance. Unlike fixed IP addresses, floating IP
addresses can have their associations modified at any time, regardless
of the state of the instances involved. This procedure details the
reservation of a floating IP address from an existing pool of addresses
and the association of that address with a specific instance.

#. Log in to the dashboard, choose a project, and click Access &
   Security.

#. Click the Floating IPs tab, which shows the floating IP addresses
   allocated to instances.

#. Click Allocate IP to Project.

#. Choose the pool from which to pick the IP address.

#. Click Allocate IP.

#. In the Floating IPs list, click Associate.

#. In the Manage Floating IP Associations dialog box, choose the
   following options:

   #. The IP Address field is filled automatically, but you can add a
      new IP address by clicking the + button.

   #. In the Ports to be associated field, select a port from the list.

      The list shows all the instances with their fixed IP addresses.

#. Click Associate.

   .. note::

      To disassociate an IP address from an instance, click the
      :guilabel:`Disassociate` button.

To release the floating IP address back into the pool of addresses,
click the :guilabel:`More` button and select the :guilabel:`Release
Floating IP` option.
