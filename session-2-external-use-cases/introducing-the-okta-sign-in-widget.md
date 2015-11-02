# Introducing the Okta Sign-In Widget

You will get a sneak peek of our new Okta Sign-In Widget.  All the
work that you’ve done today – condensed into a Java-script based
implementation.  The widget supports credentials validation,
password management, MFA, Social Auth – and much more.

<a href="setting-up-the-okta-sign-in-widget.md">Setting up the Okta Sign-In Widget</a>

## PythonApplication1.py

Add the method below:

    @app.route("/widget")
    def widget():
        return render_template(
            'sign-in-widget.html',
            client_id=client_id,
            ngrok_url=ngrok_url,
            base_url=base_url)

## templates/sign-in-widget.html

    <!doctype html>
    <html lang="en-us">
    <head>
      <meta charset="utf-8">
      <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">
      <meta name="viewport" content="width=device-width, initial-scale=1">
      <title>Example Okta Sign-In Widget</title>
    
      <!--[if lt IE 9]>
          <script src="//cdnjs.cloudflare.com/ajax/libs/html5shiv/3.7.2/html5shiv.min.js"></script>
      <![endif]-->
    
      <script src="{{ base_url }}/js/sdk/okta-sign-in-1.1.0.min.js" type="text/javascript"></script>
      <link href="{{ base_url }}/js/sdk/okta-sign-in-1.1.0.min.css" type="text/css" rel="stylesheet">
      <!-- Customizable css theme options. Link your own customized copy of this file or override styles in-line -->
      <link href="{{ base_url }}/js/sdk/okta-theme-1.1.0.css" type="text/css" rel="stylesheet">
      <link href="https://fonts.googleapis.com/css?family=Open+Sans:400" type="text/css" rel="stylesheet">
    </head>
    <body>
      <!-- Render the login widget here -->
      <div id="okta-login-container"></div>
    
      <!-- Script to init the widget -->
      <script>
        var oktaSignIn = new OktaSignIn({
            baseUrl: '{{ base_url }}',
        clientId: '{{ client_id }}',
        redirectUri: '{{ base_url }}/oauth2/v1/widget/callback?targetOrigin={{ ngrok_url }}/widget',
            authScheme: 'OAUTH2',
            authParams: {
              responseType: 'id_token',
              scope: [ 'openid', 'email', 'profile', 'address', 'phone' ]
            },
        idps: [
            ]
        });
    
        oktaSignIn.renderEl(
          { el: '#okta-login-container' },
          function (res) {
            if (res.status === 'SUCCESS') {
              var redirectUrl = "{{ ngrok_url }}/social?id_token=" + res.idToken;
              console.log('Redirecting to: %s', redirectUrl);
              window.location.href = redirectUrl;
            }
          }
        );
      </script>
    </body>
    </html>