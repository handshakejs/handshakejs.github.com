# Switching to Handshake.js

Move to Handshake.js in 5 minutes.

It takes just a few minutes to switch to using Handshake.js. Make a few changes to your login form and server-side Handshakejs code to start letting users login without a password.

## Your login form

First, make a few changes to your login form. Here's an example that isn't using Handshake.js.

```html
<form action="" method="POST" id="login-form">
  <input name="email" id="email" />
  <input name="password" id="password" type="password" />
  <button type="submit">Login</button>
</form>
```

Let's convert this to using Handshake.js. Add the js library, simplify the login form, and add the authcode form.

```html
<body>
  <form action="" method="POST" id="login-form">
    <!-- Optionally, add a section to display errors -->
    <span class="login-errors"></span>
    <input name="email" id="email" type="email" />
    <button type="submit">Request Login</button>
  </form>
  <form action="/authenticate" method="POST" id="authcode-form">
    <p>Check your email. Your authcode is being emailed to you.</p>
    <!-- Optionally, add a section to display errors -->
    <span class="authcode-errors"></span>
    <input name="authcode" id="authcode" type="number" />
    <button type="submit">Confirm Login</button>
  </form>
  <!-- include handshake.js -->
  <script src="//ajax.googleapis.com/ajax/libs/jquery/2.1.1/jquery.min.js"></script>
  <script src="http://handshakejs.la/handshake.js"></script>
</body>
```

Next, add another script section. This logic will handle requesting the login.

```html
<body>
  <form action="" method="POST" id="login-form">
    <!-- Optionally, add a section to display errors -->
    <span class="login-errors"></span>
    <input name="email" id="email" type="email" />
    <button type="submit">Request Login</button>
  </form>
  <form action="/authenticate" method="POST" id="authcode-form">
    <p>Check your email. Your authcode is being emailed to you.</p>
    <!-- Optionally, add a section to display errors -->
    <span class="authcode-errors"></span>
    <input name="authcode" id="authcode" type="number" />
    <button type="submit">Confirm Login</button>
  </form>

  <!-- include handshake.js -->
  <script src="//ajax.googleapis.com/ajax/libs/jquery/2.1.1/jquery.min.js"></script>
  <script src="http://handshakejs.la/handshake.js"></script>
  <script type="text/javascript">
    // 1. Fill in your app_name and root_url
    Handshakejs.setAppName("handshake-js_test");
    Handshakejs.setRootUrl("https://handshakejs-api.herokuapp.com");

    jQuery( function( $ ) {
      // Start with the authcode form hidden
      var $loginForm = $("#login-form");
      var $authcodeForm = $("#authcode-form");
      $authcodeForm.hide();

      // 2. When the loginForm submits
      $loginForm.submit( function( e ) {
        // 2a. Disable the submit button to prevent repeated clicks
        $loginForm.find("button").prop("disabled", true);
        // 2b. Get the email
        var email = $loginForm.find("#email").val();
        // 2c. Email authcode to user
        Handshakejs.login.request( {email: email} , function( err, res ) {
          if ( err ) {
            $loginForm.find(".login-errors").text(err);
          } else {
            // 2d. We hide the loginForm on success and show the authcodeForm
            $loginForm.hide();
            $authcodeForm.show();
          }
        });

        // Prevent the form from submitting with the default action
        return false;
      });

      // 3. When the authcodeForm submits
      $authcodeForm.submit( function( e ) {
        // 3a. Get the email and authcode
        var email = $loginForm.find("#email").val();
        var authcode = $authcodeForm.find("#authcode").val();
        // 3b. Confirm if correct authcode for email
        Handshakejs.login.confirm( {email: email, authcode: authcode} , function( err, res ) {
          if ( err ) {
            $authcodeForm.find(".authcode-errors").text(err);
          } else {
            // 3c. Get the hash - represents the authenticated user in a secure way 
            var hash = res.identities[0].hash;
            // 3d. Insert the hash into the form so it gets submitted to the server
            $authcodeForm.append($('<input type="hidden" name="hash" />').val(hash));
            $authcodeForm.append($('<input type="hidden" name="email" />').val(email));
            // 3e. Submit the form
            $authcodeForm.get(0).submit();
          }
        });

        return false;
      });
    });

  </script>

</body>
```

In the example code, you first set the root_url and the app_name, which you can [generate](http://handshakejs-api.herokuapp.com/api/v1/apps/create.json?app_name=test). Then you setup the loginForm to request the authcode. Finally, you setup the authcodeForm to verify the authcode. Note that we use jQuery in our example, but it’s not required.

## On your server

Cool, now all that is left is to verify the hash on the server side and set the user session.

```ruby
def authenticate
  Handshakejs.salt = "your_long_secret_salt_when_generating_your_handshakejs_app_name"
  result = Handshakejs.validate({:email => email, :hash => hash})
  if !!result 
    session[:user] = params[:email]
    redirect "/dashboard"
  else 
    redirect "/login"
  end
end
```

This example is in [Ruby](https://www.ruby-lang.org), but the concept is the same no matter what language you're using. The [Handshakejs rubygem](http://rubygems.org/gems/handshakejs) validates that the hash is a match for the email. It uses [PBKDF2](http://en.wikipedia.org/wiki/PBKDF2) encryption under the hood for security.

You've just made it possible for your users to very securely login without a password.

