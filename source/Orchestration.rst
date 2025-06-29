..    include:: <isonum.txt>

Resource Orchestration
======================

CloudVeneto provides an orchestration service, implemented
through the OpenStack **Heat** component, that allows you to spin up
multiple instances, and other cloud services in an automated fashion.

In Heat parlance, a **stack** is the collection of objects, or
**resources**, that will be created by Heat. This might include
instances (VMs), volumes, networks, subnets, routers, ports, security
groups, security group rules, etc.

Heat uses the idea of a **template** to define a stack. If you want to
have a stack that creates two instances connected by a private network,
then your template would contain the definitions for two instances, a
network, a subnet, and two network ports.

Either native **HOT** templates, and **AWS CloudFormation** (CFN)
templates are supported. Templates in HOT (Heat Orchestration
Template) format are typically, but not necessarily required, to be
expressed as `YAML <http://yaml.org>`__ while CFN (AWS CloudFormation)
formatted templates are instead typically expressed in
`JSON <http://json.org>`__.

Creating a template
-------------------

This is a working example of a HOT template which:

-  Creates a virtual machine connected to a project network;

-  Creates a storage volume;

-  Attaches this volume to the previously created VM;

::

    heat_template_version: 2015-04-30

    description: Template which creates a VM and a cinder volume; volume is then attached to this VM

    parameters:
      instance_name:
        type: string
        description: VM Name
        constraints:
        - allowed_pattern: "[a-zA-Z0-9-]+"

    resources:

      my_volume:
        type: OS::Cinder::Volume
        properties:
          name: "testVolume"
          size: 3

      my_instance:
        type: OS::Nova::Server
        properties:
          name: { get_param: instance_name }
          image: Centos7x86_64
          flavor: cloudveneto.small
          security_groups: [default]
          key_name: paolomazzon
          admin_pass: heattest
          networks: [{"network": testing-lan}]

      my_volume_attachment:
        type: OS::Cinder::VolumeAttachment
        properties:
          volume_id: { get_resource: my_volume }
          instance_uuid: { get_resource: my_instance }

    outputs:
      instance_fixed_ip:
        description: fixed ip assigned to the server
        value: { get_attr: [my_instance, first_address] }

Templates have three sections:

::

    # This is required.
    heat_template_version: 2015-04-30

    parameters:
      # parameters go here

    resources:
      # resources go here (this section is required)

    outputs:
      # outputs go here

The resources section specifies what resources Heat should create:

::

    resources:
      my_resource_id:
        type: a_resource_type
        properties:
          property1: ...
          property2: ...

Hardcoded values can be replaced with parameters. The actual value to be
used is then specified when the stack is created. In our example a
parameter is used for the name of the VM to be created:

::

    parameters:
      instance_name:
        type: string
        description: VM Name
        constraints:
        - allowed_pattern: "[a-zA-Z0-9-]+"

A full description of all the resources that can be used in the stack
creation can be found accessing the **Orchestration** |rarr| **Resource Types** menu.

Sometimes we want to extract information about a stack. In our example
the output is the fixed IP of the VM created by the stack:

::

    outputs:
      server_ip:
        description: fixed ip assigned to the server
        value: { get_attr: [my_instance, first_address]:

Heat templates allow also to insert user data via cloud-init, e.g:

::

    server01:
        type: OS::Nova::Server
        properties:
          image: sl66
          flavor: cldareapd.xsmall
          user_data_format: RAW
          user_data:
            str_replace:
              template: |
                #!/bin/sh
                yum install -y httpd
                service httpd start
                iptables -I INPUT 4 -m state --state NEW -p tcp --dport 80 -j ACCEPT
                service iptables save
                service iptables restart

Resource startup order can be managed in Heat, as explained in `this
page <https://blog.zhaw.ch/icclab/manage-instance-startup-order-in-openstack-heat-templates/>`__.
For example it is possible to create a VM only when another one has been
successfully started.

The Heat Orchestration Template (HOT) specification is available
`here <http://docs.openstack.org/developer/heat/template_guide/hot_spec.html>`__.

Creating a stack
----------------

To create a stack using the browser please select the
**Orchestration** |rarr| **Stacks** left menu. From here select **+ Launch stack**.

You will be prompted to select a template.

.. image:: ./images/launchstack.png
   :align: center


You will then be asked to fill in the parameters of the template and
launch the stack.


.. image:: ./images/launch_param.png
   :align: center


Then you can follow the status of your stack on the dashboard.

