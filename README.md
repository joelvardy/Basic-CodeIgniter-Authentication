Basic CodeIgniter Authentication
================================

A very simple library allowing users to be authenticated, there is no access level checking built in.

## Install

To install the library copy:

 * application/libraries/Authentication.php
 * application/config/authentication.php

Into your own CodeIgniter installation.

*If your CodeIgniter installation uses a table prefix then append it to the table names within the included SQL files and statements.*

Then create the required tables, to do this run the included SQL file `tables.sql`

Ensure you have setup an [encryption key] [1] and have configured CodeIgniter to [use a database] [2].

You may then integrate the library into your code.

## Setup

You should have a look at the authentication configuration file, `application/config/authentication.php` and ensure all the details are correct, there should be adequate comments to help you out.

## Loading the library

Before you can use any of the methods listed below you must load the authentication library, you can do this in a number of ways, which I will discuss below.

#### Controller Loading

This is where you specify you would like to load the authentication library in each controller where you will use it, eg:

```php
// Load the required libraries
$this->load->library('authentication');
``` 

#### Auto Loading

This is where you specity the authentication library globally and you won't have to load it again, you do this by:

 * Opening the `application/config/autoload.php`
 * Adding "authentication" to the `$autoload['libraries']` array.

For example:

```php
$autoload['libraries'] = array('authentication');
```

## Using the library

Once you've loaded the library, you can use the methods listed below, like this:

```php
$this->authentication->method_name($parameter)
```

### Check Username

You can use the `check_username` method to see whether a username is unique, this method is also called before allowing a user to be created.

```php
// Read the username
$username = $this->input->post('username');

// Check whether it is available
if ($this->authentication->username_check($username))
{
	// The username IS available
} else {
	// The username IS NOT available
}
```

### Create User

This method will add a user to the users table (as defined within the configuration file) it will only add the username and password to the database, there is no method of adding other data, so all other fields in your users table must accept null values.

```php
// Read the username
$username = $this->input->post('username');

// Read the password
$password = $this->input->post('password');

// Create the new user
$user_id = $this->authentication->create_user($username, $password);

// Check for success
if ($user_id !== FALSE)
{
	// The user was SUCCESSFULLY created
	// Their ID is $user_id
} else {
	// There was an ERROR creating the user
}
```

### Login

This is a very simple library and it does not check against brute force attacks. It also does not allow for a 'remember me' option, once the user closes their browser (or a period of inactivity) the user will be logged out.

The login method takes the username and password and will return either true or false, eg:

```php
// Read the username
$username = $this->input->post('username');

// Read the password
$password = $this->input->post('password');

// Try to log the user in
if ($this->authentication->login($username, $password))
{
	// The user was SUCCESSFULLY logged in
} else {
	// The details provided were incorrect
	// No feedback is given as to why the login failed
}
```

### Check Loggedin

This method will return true or false depending on where the current user is logged in.

```php
if ($this->authentication->is_loggedin())
{
	// User is logged in
} else {
	// User is NOT logged in
}
```

### Change Password

This method allows you to change the password for a user by passing the password and their identifier (as set in the configuation file.) You can also just pass the password and the current users password will be updated.

```php
// Read the password
$new_password = $this->input->post('password');

if ($this->authentication->change_password($new_password))
{
	// The users password HAS been updated
} else {
	// There was an ERROR updating the users password
}
```

### Logout

When you call this method it will log the current user out, all other session data you set for the user will remain intact.

```php
$this->authentication->logout();
```

### Delete User

This method will delete the user with the passed identifier (as set in the configuation file.)

```php
// Define user identifier
$user_identifier = 1; // Using the ID column in the users table as the identifier

if ($this->authentication->delete_user($user_identifier))
{
	// The user HAS been deleted
} else {
	// There was an ERROR deleting the user
}
```

## Troubleshooting

* You will encounter an error if you try to access the authentication library from a controller called "authentication"

## Credits

[Joel Vardy] [3] for writing this.

  [1]: http://codeigniter.com/user_guide/libraries/encryption.html
  [2]: http://codeigniter.com/user_guide/database/configuration.html
  [3]: http://joelvardy.com