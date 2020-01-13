Lab 5 - Creating Multiple HTTP Applications per tenant using AS3 Declarative Interface 
--------------------------------------------------------------------------------------------------
In this lab, we will create two simple HTTP application using AS3 within the same tenant. After creating these applications, we will modify one of them. 

**Exercise 1 - AS3 Declaration**

#. Open Postman and locate the module 5 folder under collections on the left side of postman. Double-click the 'HTTP Multiple-Application' file.

#. Examine the body of the AS3 declaration. Note the schema and how we are declaring two HTTP applications within the same tenant. 

#. After examining the declaration, send the declaration and confirm a 200 OK response. 

#. Navigate to the BIG-IP and visually confirm the changes have been made. 

    .. image:: /_static/two_apps_confirm.jpg



**Exercise 2 - Modify an HTTP Application within a Multi-App Single Tenant AS3 Declaration**

#. In Postman, locate the declaration we previously sent.
#. We want to modify 1 of our HTTP Applications. We will add another pool member to our 'http_app_2' and the pool member we want to add is '10.1.10.34'. 
#. Modify the AS3 declaration so that our 'http_app_2' has the appropriate pool members. Once modified, send the declaration.

    .. image:: /_static/3_pool_members.jpg

#. Confirm the changes on the BIG-IP. Navigate to the pool members of our 'http_pool_2' and confirm there are now 3 pool members in this pool.

    .. image:: /_static/confirm_3.jpg

    .. image:: /_static/confirm_3_nodes.jpg



**Exercise 3 - Delete HTTP Applications and Tenant**

#. In order to delete our virtual server, pool and pool members, we can simply send a POST with an empty tenant body. Since AS3 is declarative, it will notice that we are sending a POST with an empty tenant body, and will by default delete the existing virtual server, pool and pool members.
#. In Postman, find the 'DELETE application' file. Examine the uri and body declaration. Notice we are sending a POST to the same API endpoint, but take a close look at the JSON body.
#. The body declares a AS3 tenant called http_tenant, but the body describing the state of the tenant is empty. By default, AS3 will remove the virtual server, pool and pool members. Since this would cause the entire tenant to be empty, AS3 will also remove the tenant for us.
#. Click 'SEND' and ensure a 200 OK response. Navigate back to the BIG-IP, refresh the page and confirm the changes that the tenant has been deleted.

    .. image:: /_static/delete_tenant.jpg

