# Handshake.js

## The passwordless authentication system

## Alternative. Secure. Convenient.

Handshake's alternative approach to authentication is easier for users and more secure for your application.

### Safer

By eliminating the password you are eliminating the number one seucrity hole - user error and their insecure passwords. The authcode is generated uniquely every time and expires after a short period of time.

### Secure

Handshake uses the proven cryptographic standard [PBKDF2](http://en.wikipedia.org/wiki/PBKDF2) for security. Emails and authcodes are passed over SSL, and application salts are doubly encrypted in the datastore.

### Easy to implement

You can add Handshake.js to your website in very few lines of code. In addition, it eliminates all the effort you'd normally do coding signup, login, and forgot passwords forms.

### Open Source

The entire framework is open sourced. You can adjust the code to your own needs. You can self-host the API portion of it, further increasing your security.

* * *

[Get Started Button](https://github.com/handshakejs/handshakejs-js)

* * *


## Ecosystem

There are multiple parts that make up the [Handshake.js ecosystem](https://github.com/handshakejs).

The main part is [Handshake.js](https://github.com/handshakejs/handshakejs-js). This library integrates with your application's form fields and talks to the [API](https://github.com/handshakejs/handshakejs-api) to generate the authcodes for logging in. 

The API is hosted by [me](http://scottmotte.com) at no cost to you. You can self-host it if you wish. There's additional [configuration available](https://github.com/handshakejs/handshakejs-api#installation) to you if you do so. 

I recommend starting with the freely hosted version and then host your own when ready. Finally, there are the libraries which help ease integration into the language/framework of your choice.

Outline of the ecosystem:

+ [Handshake.js](https://github.com/handshakejs/handshakejs-js)
+ [API](https://github.com/handshakejs/handshakejs-api)
  + [Logic](https://github.com/handshakejs/handshakejslogic)
  + [Transport](https://github.com/handshakejs/handshakejstransport)
+ Libraries
  + [Ruby](https://github.com/handshakejs/handshakejs-ruby)
  + [Nodejs](https://github.com/handshakejs/handshakejs-nodejs)

## Made by [Scott Motte](https://github.com/scottmotte) and others

+ [Daniel Archer](https://github.com/danieljacobarcher)
+ [Kevin Chandler](https://github.com/kevinchandler)
+ [Jacob Lowe](https://github.com/jacoblwe20)
+ [Scott Motte](http://github.com/scottmotte)
