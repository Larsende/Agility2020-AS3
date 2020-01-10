Lab 1 - Creating a Simple HTTP Application using AS3 Declarative Interface & AS3 Schema Validation
--------------------------------------------------------------------------------------------------
In this lab, we will create a simple HTTP application using AS3. Before sending the AS3 declaration, we will use Microsoft's Visual Studio Code to validate our JSON schema.

**Exercise 1 - Launch and configure Postman**

#. Right click and open Postman

    .. image:: /_static/postman.jpg

#. Once Postman has launched, we need load our environment, variables and import the collections we will be using for the lab.
#. First, lets import the collections, which are located on a github repo. On the top left, click the 'import' button.

    .. image:: /_static/import.jpg

#. Select 'import from link' and paste the following link into the text box, then click 'import':

    .. image:: /_static/import_from_link.jpg

#. You should now see a new folder under the collections tab to the left of your postman application screen.

#. Navigate to File -> Settings and confirm the 'SSL certificate verification' option is  turned OFF. If it is on, please turn it OFF. Once finished, exit the settings menu.

#. Now we will import an environment. An environment is a set of variables that allow you to switch the context of your requests. Repeat the same steps we did to import our collections. On the top left, click the 'import' button.

    .. image:: /_static/import.jpg

#. Select 'import from link' and paste the following link into the text box, then click 'import':

    .. image:: /_static/import_from_link.jpg

#. Navigate to the top right of Postman where you will see a dropdown menu to choose your environment. Click on the environment we just imported. You can then view the variables and their values by clicking the eye button.

    .. image:: /_static/environment.jpg

    .. image:: /_static/eye.jpg

**Exercise 2 - AS3 declaration**

#. Expand the AS3 collections folder that we imported by clicking on it.
#. First, we will send a GET request to our BIG-IP to see information about the AS3 RPM.
#. Locate the 'AS3 Info' request and double click. Notice that we are sending a GET request to an API endpoint with an empty body. Click the blue **send** button and ensure you get a 200 OK response.

#. Locate the 'HTTP Application' request in the same folder. Notice that we are sending a POST request to an API endpoint. To examine the body of our request, click the body tab.

#. Before we send our AS3 JSON to the BIG-IP, we want to ensure the schema is valid. In order to do this, we will use VS Code.


**Exercise 3 - Visual Studio Code Schema Validation**

#. Open VS Code.

    .. image:: /_static/vs_code.jpg

#. Once open, open a new working file by navigating to File->New File. Copy the contents of the AS3 Schema Validation JSON body and paste into VS Code. Save the file to your desktop and notice the red line notifying you that there is a typo. Hover over the red line to see more details and correct the typo by removing one of the extra 't' in 'Tenant'.

    .. image:: /_static/vscode_newfile.jpg

#. Using Schema Validation can be very useful when creating AS3 JSON declarations. It can help check the accuracy of a declaration before deployment.
#. Now that we have validated our schema, we can confidently send this declaration to the BIG-IP to create an HTTP application.


**Exercise 4 - Send AS3 declaration**

#. Go back to Postman and find the HTTP Application JSON file. Note the only difference between this and the one we used to validate is the first line with the key/value of 'schema' and the schema format URL.
#. Send the AS3 declaration by clicking the blue 'SEND' button. Ensure once it is done processing that you receive a 200 OK response.




**Exercise 5 - Confirm changes on BIG-IP**

#. Login to the BIG-IP to confirm our changes. Open up the web browser (Chrome) and navigate to 'https://10.1.1.5".
#. Login with the following credentials: username = admin , password = admin.
#. Once you are logged in, expand the Local Traffic tab by clicking on it on the left and click Virtual Servers.
#. Change your partition by choosing the tenant we created, 'HTTP_tenant' on the top right of your BIG-IP GUI.

    .. image:: /_static/change_tenant.jpg

#. You should now see your virtual server created. Navigate to 'Pools' and examine your pool that was created. Click on the 'http_pool' and click on the 'members' tab to view the pool members.

    .. image:: /_static/virtual_server.jpg

    .. image:: /_static/pool.jpg

    .. image:: /_static/pool_members.jpg

**Exercise 6 - Delete HTTP tenant**

#. In order to delete our virtual server, pool and pool members, we can simply send a POST with an empty tenant body. Since AS3 is declarative, it will notice that we are sending a POST with an empty tenant body, and will by default delete the existing virtual server, pool and pool members.
#. In Postman, find the 'DELETE application' file. Examine the uri and body declaration. Notice we are sending a POST to the same API endpoint, but take a close look at the JSON body.
#. The body declares a AS3 tenant called http_tenant, but the body describing the state of the tenant is empty. By default, AS3 will remove the virtual server, pool and pool members. Since this would cause the entire tenant to be empty, AS3 will also remove the tenant for us.
#. Click 'SEND' and ensure a 200 OK response. Navigate back to the BIG-IP, refresh the page and confirm the changes that the tenant has been deleted.

    .. image:: /_static/delete_tenant.jpg

