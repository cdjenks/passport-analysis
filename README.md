# Passport App Analysis

## Folders and Top-level Files
* [package.json](#package.json)
* [server.js](#server.js)
* [Config Folder](#config)
* [Models Folder](#models)
* [Routes Folder](#routes)
* [Public Folder](#public)
* [Future Development](#develop)

## <a name="package.json"></a>package.json

This file contains and tracks info related to all the node module dependencies the application has. This is the file that will be referenced when 'npm install' is run in the terminal of a new machine the app is being installed on.

## <a name="server.js"></a>server.js

The server.js file establishes an express server on your machine and lays out some of the middleware requirements for the app to run. The server file also creates a new instance Sequelize and syncs it with the database. Obviously this file also requires the html and api routes files which are needed to handle GET and POST routes related to the app's api and to serve the html files to the browser.

## <a name="config"></a>Config Folder

### *isAuthenticated.js*

This file directs the user to the correct page depending on whether the user is currently logged into their account or not. The one function in this file is used in html-routes.js for determining what html to return.

### *config.json*

This file stores information related to connecting the app to the mysql database

### *passport.js*

This file requires passport and then configures passport to require an email rather than a username. If a user is trying to login, the code here checks the entered information against existing email/password pairs in the database to determine if the users should be given access to the members page. This file also handles serializing and deserializing of the users current session.

## <a name="models"></a>Models Folder

### *index.js*

???
This file initializes the sequelize database and unifies all the models files (in this case it is actually only one but could be more) so that they can all be easily accessed within the server.

### *user.js*

The file requires 'bcriptjs' for password hashing and creates a sequelize model to correspond with an actual table in the mysql database. The model constructor checks that each new user's email entry is not null and is a valid email; the password cannot be null either. This file also use bcrypt to create a method for checking if an entered password is correct for a given user who is trying to login. Finally, the this file creates a "hook" (which is a method that runs automatically) to hash any newly created password for safe storage.

## <a name="routes"></a>Routes Folder

### *api-routes.js*

This file requires passport.js to determine if entered credentials are valid. The '/api/login' POST route compares the entered information against existing email/password pairs to determine if access to the memebers page should be granted. The 'api/signup' POST route creates a new user object with the data passed in from the request and then automatically logs the new user in and sends them to the members page. This routes file also has a GET routes for logging out a user and then sending them to the login page. The final route in the file is another GET route that checks if the user is logged to know if it should send back user information to display in the html (currently the only information being sent back is the email and id# for the user, but the database could be expanded to hold and return more useful information).

### *html-routes.js*

This file requires isAuthenticated.js to check if the user is already logged in or not. If so, the '/' GET route will automatically serve them the members page. Otherwise the user will be served the signup.html page. The same process applies for the '/login' GET route except the login.html will be served. The final '/members' GET route confirms that a user is actually signed in before sending members.html back to the brower.

## <a name="public"></a>Public Folder

### *login.html, members.html, and signup.html*

The login and signup html files for this application contain simple forms for the user to enter their email and a password. The members.html file simply confirms that the email of the user who is currently logged in (and that they are in fact logged in).

### *login.js*

The function in this file capture the email and password fields from login.html and POSTS them to the '/api/login' route to be authenticated. If the information matches a user in the database, the members page is served back to the browser.

### *members.js*

This code is used by members.html to GET the specific current user's data if they are logged in. That user's email is then displayed in an h2 element within the html.

### *signup.js*

Similar to the login.js, signup.js collects the user inputs for email and password and then sends the data to the server via a POST route. If the inputs clear the validation criteria the data is becomes another user/row in the database, and the server sends back the members.html since the user is now logged into the website.

## <a name="develop"></a>Future Development

The most straight forward avenue for further development of this app would be to allow for more user data of some sort to be stored in the database. This data could then only be accessed from the front end by the one user, however, this idea would only make sense if it was not truly sensitive data since it probably would not all be encrypted before being added to the database. Also, changing the login error message would be a good improvement to implement.