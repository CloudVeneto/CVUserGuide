..    include:: <isonum.txt>

Deploying and using Windows VMs
==========================================




Deploying a Windows VM: step-by-step guide
------------------------------------------


Select the Windows Image
^^^^^^^^^^^^^^^^^^^^^^^^
.. _Windowsimages:



From the list of Public Images, look for the image named "Win10_22HXXXX" (or similar).
   
.. _publicimages:


.. image:: ./images/Win10-ImageWin10.png
   :align: center



From the dropdown menu next to the image, select "Create Volume".


.. image:: ./images/Win10-CreateVolumefromImage.png
   :align: center


Create the bootable volume
^^^^^^^^^^^^^^^^^^^^^^^^^^

While creating the volume, fill in all the required parameters.
Important: make sure to set the volume size to at least 35GB.
(The system might allow smaller sizes, but Windows installation or updates may fail without enough space.)

.. image:: ./images/Win10-CreateVolume.png
   :align: center

Launch an instance from the bootable volume
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Once the volume has been created, it will appear under the Volumes section.
Open the Actions dropdown next to the new volume and select "Launch as Instance".
Configure the instance as needed (specifying the flavor, network, key pair, etc.).


.. image:: ./images/Win10-LaunchInstance.png
   :align: center


Enable Remote Access via RDP - Setting Security groups(s)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
.. _Setting Security group:

To allow remote access to the Windows VM via RDP, you must configure a Security Group that includes a rule to allow inbound traffic on port 3389 (TCP).
This rule is already active in the image provided, but you must still assign the appropriate security group to the instance.



.. image:: ./images/Win10-AddRuleRDP.png
   :align: center

.. image:: ./images/Win10-CreateSecurityGroupRDP.png
   :align: center


.. NOTE ::
   Note: If additional ports need to be opened, remember that you must add the corresponding rules to the security
   group in OpenStack and allow the same ports through the Windows firewall inside the VM


Initial Windows Setup via Console
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Once the instance has been successfully created, you will have access to a running Windows VM.
Navigate to the Console tab (alongside Overview, Interfaces, Log, Console, Action Log) to interact with the VM.

.. image:: ./images/Win10-Console.png
   :align: center

Install and Configure Windows 10
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
From the console, you can begin the initial Windows 10 setup process. Follow the on-screen instructions:

* Select the appropriate region

  .. image:: ./images/Win10-StartInstallation.png
     :align: center

* Choose your keyboard layout

  .. image:: ./images/Win10-Keyboard.png
     :align: center

* Wait for any necessary operations to complete (this might take a few minutes)

  .. image:: ./images/Win10-Wait.png
     :align: center


License Agreement
^^^^^^^^^^^^^^^^^

Accept the Windows License Agreement when prompted.

.. image:: ./images/Win10-License.png
   :align: center

Installation Mode
^^^^^^^^^^^^^^^^^
Choose the installation type that suits your needs. A standard installation should be ok for most cases.

.. image:: ./images/Win10-PersonalOrganization.png
   :align: center

Set Up a User Account
^^^^^^^^^^^^^^^^^^^^^
   
When prompted add your account:

* Select “Offline account” (unless you have a Microsoft account and you prefer to use that)

  .. image:: ./images/Win10-AccountOffline.png
     :align: center

* If you select offline mode, continue by clicking “Limited experience” when asked

   .. image:: ./images/Win10-Go.png
      :align: center

Create a Local User Account
^^^^^^^^^^^^^^^^^^^^^^^^^^^
Set a username and password for the local user account. 
This will be the main credential used to log in to the VM.

.. image:: ./images/Win10-Account.png
   :align: center

.. image:: ./images/Win10-Password.png
   :align: center


Set Security Questions
^^^^^^^^^^^^^^^^^^^^^^

During the setup process, you’ll be asked to define three security questions. These are required for account recovery in case you forget your password.
Choose the questions and answers that are easy for you to remember but hard for others to guess.

.. image:: ./images/Win10-Questions.png
   :align: center

Privacy and Permissions Settings
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

You will be prompted to configure various Windows permissions and privacy settings. Review and choose according to your preferences:


* Allow Microsoft to use your location

  .. image:: ./images/Win10-Position.png
     :align: center
* Enable Find My Device

  .. image:: ./images/Win10-Dispositivo.png
     :align: center
* Choose to send diagnostic data to Microsoft (basic or full)

  .. image:: ./images/Win10-Diagnostic.png
     :align: center
* Enable pen input improvement

  .. image:: ./images/Win10-Pen.png
     :align: center
* Allow personalized experiences using diagnostic data

  .. image:: ./images/Win10-Expertice.png
     :align: center
* Permit apps to use your advertising ID

  .. image:: ./images/Win10-ID.png
     :align: center
* Customize your experience (optional settings based on usage)

  .. image:: ./images/Win10-Personal.png
     :align: center
* Enable or disable Cortana (Microsoft's virtual assistant)

  .. image:: ./images/Win10-Cortana.png
     :align: center
* Allow automatic setup and updates (do not turn off the computer during this process)

  .. image:: ./images/Win10-execution.png
     :align: center

Finalizing the Setup
^^^^^^^^^^^^^^^^^^^^

After completing these steps, Windows will finalize the setup.
You’ll see a message like “Almost there…”.

.. image:: ./images/Win10-Ready.png
   :align: center

Once finished, the system will boot into a fully operational Windows 10 environment, based on the original "Win10_22HXXXX" image.

.. image:: ./images/Win10-ReadyStart.png
   :align: center


Windows Activation
^^^^^^^^^^^^^^^^^^

You may notice a watermark in the lower-right corner of the desktop saying:
"Activate Windows. Go to Settings to activate Windows."

.. image:: ./images/Win10-WindowsKey.png
   :align: center

Open Settings > Update & Security > Activation.

A message will appear: "Windows is not activated. Activate now."

.. image:: ./images/Win10-AccountWin.png
   :align: center

Proceed to enter a valid product key when prompted.

.. image:: ./images/Win10-Key.png
   :align: center


.. NOTE ::

  For official instructions on how to activate Windows, refer to the Microsoft support page:
  https://support.microsoft.com/en-us/windows/activate-windows-10-92c27cff-d2c2-6d78-4136-6b40baecc8c3
  If you don’t have a valid product key, you will need to acquire one through Microsoft or a licensed distributor.


KMS License Activation (**INFN Staff Only**)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. NOTE ::
   
   **Only INFN staff users** are authorized to request a KMS (Key Management Service) license.
   It is the user's responsibility to carefully read and understand the terms and conditions related to activating a KMS license on virtualized clients.
   Failure to comply with the licensing terms may result in improper activation or violation of usage policies.


* Usage Terms and Conditions:

   https://web.infn.it/windows/index.php/licenze/windows-client-licenze-e-virtualizzazione

* KMS Installation Instructions:
      
   https://web.infn.it/windows/index.php/istruzioni/17-kms-categoria

   To activate Windows using the KMS server, follow these steps only if you are authorized and have a valid product key.

   * Open Command Prompt as Administrator (Right-click on Command Prompt and select “Run as Administrator”)

   * Run the following commands one by one:

   ::

      cscript \windows\system32\slmgr.vbs /ipk "ENTER-YOUR-PRODUCT-KEY-HERE"   #Replace "ENTER-YOUR-PRODUCT-KEY-HERE" with the specific product key for your Windows version.
      cscript \windows\system32\slmgr.vbs /skms kms.infn.it
      cscript \windows\system32\slmgr.vbs /ato

These commands will:

* Set the product key

* Configure the KMS server to kms.infn.it

* Attempt to activate Windows via the INFN KMS infrastructure


Connecting to the Windows VM via Remote Desktop
-----------------------------------------------

Once the setup is complete and the instance is running, you can connect to the virtual machine using Remote Desktop (RDP).


* Open the Remote Desktop Connection application on your local machine. In the Computer field, enter the IP address assigned to the VM and the username
  
  .. image:: ./images/Win10-RemoteDesktop.png
   :align: center


* Use the username and password of the local account you created during the Windows setup

  .. image:: ./images/Win10-RemoteDesktopLogin.png
   :align: center

* Once connected, you will be able to interact with your Windows VM just like a physical machine.

  .. image:: ./images/Win10-VMConnect.png
   :align: center


















