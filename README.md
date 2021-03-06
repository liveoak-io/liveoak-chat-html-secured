Secured chat
============
Features
--------
* HTTP REST including authentication and authorization. Keycloak integration.

* Stomp over websockets used for subscription requests. Subscription requests allowed just for authorized users (members of role 'admin' or role 'user').

* Authorized subscription responses received for newly added chat messages and also for deleted or updated messages. Update of messages is not provided by this example, but is possible through LiveOak admin console.

* Users with role _admin_ can delete any chat message. So administrator is allowed to censor everything. Normal users can delete just their own messages.

Installing the application
----------------------------

There are two ways that this example may be installed.

### Admin Console:

1. Click "Install Example Application" button, or "Try example applications" link from "Applications" page if you already have applications installed.
2. Click the "Secured Chat" example and then click "Install".

### Manually:

Assumption is that:
* $LIVEOAK points to the directory with your LiveOak server
* $LIVEOAK_EXAMPLES points to the directory with LiveOak examples

So then copy the example into the _apps_ directory of your LiveOak server and start the server
```shell
$ cp -r $LIVEOAK_EXAMPLES/chat/chat-html-secured $LIVEOAK/apps
$ sh $LIVEOAK/bin/standalone.sh
````

Setup the application
---------------------

> An Application Client was created upon installation of the application. Note that the redirect and web origin
> urls are set to 'http://localhost:8080'. If LiveOak is installed at something other than 'http://localhost:8080',
> then it will need to be edited within the Admin Console.

> When installed on OpenShift, be sure to change 'http' to 'https' for the urls mentioned above.

* Create some default users for testing purposes (their names and default passwords are not important, feel free to use different names). This step is not mandatory as you can automatically register users later on the login screen of the application.
But it's useful as self-registered users always have just default roles (in our case role "user"), so you can't test all the authorization possibilities when all users have just same role "user" .
  * Go to http://localhost:8080/admin#/applications/chat-html-secured/security/users
  * New User
    * username: "bob"
    * password: "bob"
    * roles: select both "admin" and "user"
    * Click "Save"
  * Repeat the steps and create another user "john", but assign him just to role "user"
  * Repeat the steps again and create last user "mary" and don't assign her to any role

* Create new collection in MongoDB (Manual step currently required)
  * Go to [http://localhost:8080/admin#/applications/chat-html-secured/storage/storage/browse](http://localhost:8080/admin#/applications/chat-html-secured/storage/storage/browse)
  * Click "New collection" > Fill collection name "chat" (name is important as it's used by the application) > Click "Add"
  * This step is not mandatory because in case that you first login as some admin user (in our case user "bob"), collection will be automatically created during first access to application. But in case that you're using just self-registered users, it will be needed.

* Open your browser at [http://localhost:8080/chat-html-secured](http://localhost:8080/chat-html-secured)

Running the application
-----------------------

* Open your browser at [http://localhost:8080/chat-html-secured](http://localhost:8080/chat-html-secured)
* User 'bob' is admin and can do anything (subscribe, create new chat messages, view all messages received from subscription, delete any message). He is admin and so he is allowed to censor/delete any message created by any user.
* User 'mary' doesn't have any roles and she can't do anything (view existing messages, create new messages, subscribe to receive messages). She will receive authz error directly
when you login because she is not authorized to subscribe.
* User 'john' is normal user. He can view existing messages, create new messages and subscribe to receive messages. But he is not authorized
to delete chat messages, which were not created by himself. Basically members of role 'user' can delete just their own messages.
