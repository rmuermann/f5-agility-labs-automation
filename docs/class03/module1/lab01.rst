Lab 1 - Creating a Simple HTTPS Application using FAST - (F5 Application Services Template)
===========================================================================================

F5 Devices
----------

   F5 Device connection information for reference in the following tasks

   ======= ========= ============
   Device  Mgmt. IP  user/pass
   ======= ========= ============
   bigip1  10.1.1.6  admin/admin 
   bigip2  10.1.1.7  admin/admin 
   ======= ========= ============

Task 1 - Getting connected
---------------------------

#. Open VS Code

      To connect to ``VS CODE``, select the ``ACCESS`` dropdown from the ``client`` ``system``, then select ``VS CODE``

      .. image:: ../images/VSCode-UDF-DropDown.png

      
#. Access F5 VSCode extension

      Once connected, click on the ``F5`` logo on the left-hand side of window.

      .. image:: ../images/VScode_F5Logo.jpg
         :scale: 50 %

#. Add F5 Host

      If ``BIG-IP`` host ``10.1.1.6`` is not listed under ``F5 HOSTS``, click on ``ADD HOST``.

      .. image:: ../images/VScode_F5AddHost.jpg
         :scale: 50 %

      When prompted, enter: admin@10.1.1.6

      .. image:: ../images/VScode_F5AddBIGIP01.jpg
         :scale: 50 %

#. Connect to F5 Host

      Click on the newly created host entry in the ``Hosts`` view to connect

      .. image:: ../images/lab01_vscode_selectDeviceHostsView.png
         :scale: 50 %

      Enter the password of ``admin`` when prompted
      
      
      
      
      .. NOTE:: An alternative method to connect is to select ``F5 -> Connect!`` at the bottom of the screen
      
      .. image:: ../images/VScode_F5Connect.jpg
         :scale: 50 %

      When prompted choose ``admin@10.1.1.6`` from the list of choices:
      
      .. image:: ../images/VScode_F5ConnectBIGIP01.jpg
         :scale: 50 %



#. Confirming connection

      You can confirm ``VS CODE`` is connected by looking at detail on ``F5 -> Connect!`` at bottom of screen.  In the status you can see the version of AS3 that is installed on the ``BIG-IP``.  In this case, it is version ``3.26.0``.

      .. image:: ../images/VScode_F5ConnectedBIGIP01.jpg
         :scale: 50 %

      .. NOTE:: The version of ``AS3`` in the graphic could differ from the lab guide. 


Task 2 - Access extension documentation
---------------------------------------

      This task is just to highlight the quick access to documentation and examples accessible from within the extension.  Explore the different options

#. Click on the ``vscode-f5 Documentation`` button in the ``DOCUMENTATION/EXAMPLES`` view.

      .. image:: ../images/lab01_vscode_documentation_button.jpg

#. When prompted, Select ``Open`` to continue, which will open a new browser window to the documentation. 

      .. image:: ../images/OpenVScodeExtension.jpg
   


Task 3 - Import devices
-----------------------

How to import devices for larger/automated environments

      #. Open a new tab by double clicking in the main editor area, or select ``New file`` in main ``VS CODE`` window.
      #. Paste the following into the editor

         .. code-block:: bash
            :linenos:

            [
               {
               "device": "admin@10.1.1.7",
               "password": "admin"
               }
            ]
      

#. Highlight the json object we just pasted, then right-click in editor, select ``Import Devices``

      .. image:: ../images/lab01_vscode_deviceImport.png
         :scale: 75 %

#. You will see the ``10.1.1.7`` device added to the list of ``F5 HOSTS``.

      .. image:: ../images/ImportDeviceResults.jpg


Task 4 - Install FAST extension
-------------------------------

      How to install ATC services using the extension.

      .. NOTE::  This method is considered the "offline" method so all the lab users are not trying to download the same files from github at the same time.  For additional information on alternate install methods see:  https://f5devcentral.github.io/vscode-f5/#/atc_rpm_mgmt

#. In VSCode, make sure you are connected to BIG-IP01 (10.1.1.6) by looking at the bottom ribbon:

   .. image:: ../images/VScode_F5ConnectedBIGIP01.jpg
      :scale: 60%

#. In VSCode, go to the File Explorer view by choosing ``Menu`` >> ``View`` >> ``Explorer``
      .. image:: ../images/VScodeOpenFileExplorer.jpg

#. Open the files folder

#. Right-click ``f5-appsvcs-templates-1.7.0.noarch.rpm``, then select ``Install RPM``

      .. image:: ../images/VSCode-FAST-RPMinstall.png
         :scale: 50 %

      .. Note:: This process will upload the local RPM and install it on the F5.  The install happens rather quickly, but it can take another 30-60 seconds for all the servcies to restart and present the changes.  


#. When complete, the vscode-f5 extension should reconnect and refresh all the details.

      You may need to click on the device in the HOSTS view to reconnect and refresh the discovered services

#. Notice that FAST now shows as installed (with version number) along the bottom of the editor

      .. image:: ../images/lab01_vscode_fastInstalledVersion.png
         :scale: 80 %
      
      
Task 5 - Deploy application via FAST template in TMUI
-----------------------------------------------------
   
#. Go back to UDF deployment screen, and choose the component ``bigip1``.  Then choose the ``Access Method`` of ``TMUI``.  This will allow you to login to the ``BIG-IP`` GUI.
      
      .. image:: ../images/VSCode-bigip1_tmui_access.png
         :scale: 75%

#. Login with the following credentials: username = admin , password = admin.

#. Under ``iApps`` >> ``Application Services`` : ``Applications LX``, select ``F5 Application Services Templates``

      Login if needed: ``admin/admin``

      Select the ``Deploy`` tab, expand the ``examples`` section, select ``examples/simple_udp_defaults``

      Notice that the template has default parameters

      Click each of the buttons at the bottom, next to the submit button, to see the different outputs:

      - View Template

      - View Schema

      - View Inputs

      - View Rendered

      Now click  ``Submit`` to deploy an application using the ``FAST`` template

      .. image:: ../images/lab01_tmui_fast_template01b.jpg
         :scale: 80 %
      
      This will bring you back to the ``Deploy Log`` tab and provide a status the application deployment process

#. In the vscode-f5 extension refresh the ``FAST/AS3`` views with the refresh icon in the upper right hand corner of each view window

      Explore the windows to see the deployed ``FAST`` application/task, and how it resulted in an AS3 Tenant/Task

      .. image:: ../images/lab01_vscode_fastAppFromTMUI.png
         :scale: 80 %

#. Delete application

      In the TMUI (F5 GUI), in the ``F5 Application Services Templates`` screen, select the ``Application List``.

      We should see the application we deployed with ``FAST``.

      On the right side of the application row item, select the ``trash`` icon to delete the application.

      .. image:: ../images/lab01_vscode_deleteFastAppFromTMUI.png

Task 6 - Deploy FAST application via API
----------------------------------------

#. In Coder (browser tab for ``VS CODE``), paste the following into an editor

      .. code-block:: json
         :linenos:

         {
         "name": "examples/simple_http",
         "parameters": {
            "tenant_name": "apiTenant",
            "application_name": "apiTenant",
            "virtual_port": 80,
            "virtual_address": "192.168.230.40",
            "server_port": 8080,
            "server_addresses": [
                  "192.168.100.11",
                  "192.168.100.12"
               ]
            }
         }

#. Highlight the object, right-click, then select ``Deploy Fast App``

      This should produce the following output in another tab

      .. code-block:: json
         :linenos:

         {
            "id": "4b06e4d9-01f1-497e-93e5-662d5eb75d1d",
            "code": 200,
            "message": "success",
            "name": "examples/simple_http",
            "parameters": {
               "tenant_name": "apiTenant",
               "application_name": "apiTenant",
               "virtual_port": 80,
               "virtual_address": "192.168.230.40",
               "server_port": 8080,
               "server_addresses": [
                     "192.168.100.11",
                     "192.168.100.12"
               ]
            },
            "tenant": "apiTenant",
            "application": "apiTenant",
            "operation": "create"
         }

      .. image:: ../images/lab01_vscode_deployFastAppAPI.gif

#. Check the ``BIG-IP`` TMUI to see that the application is now in the ``Application List`` tab.

#. Check out the vscode-f5 FAST/AS3 views to see what was deployed


Delete application through the FAST API
---------------------------------------

#. In the Coder UI, right-click on the ``Deployed Application`` in the FAST view, then select ``Delete Fast App``

      .. image:: ../images/lab01_vscode_deleteFastAppAPI.gif
