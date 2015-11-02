# Adding Social Login to the login page

Building on the previous exercise, we’ll add Social Login support
to your page.  If you think about B2C use cases, this is almost a
pre-requisite today to allow registration and authentication –
using Social Identity Providers like Facebook and Linkedin.

Our Social Identity Provider offering also introduces webhooks for
the first time.  It is not part of the lab – but we will discuss
how webhooks are used in the context of our social
authentication/registration flow.

## Set up Social Auth for your Okta org

Follow the instructions for <a href="social-auth-setup.md">setting
up an Okta Social Authentication provider</a> for your Okta org.

When you are done, with that setup guide, you will be modifying
your `PythonApplication1.py` and `main_page.html` files according
to the instructions below.

## Update main\_page.html

Update your `main_page.html` file with the HTML below. Add this
HTML just below the `<button>` tag that says "Log in", it should
look like this:

**NOTE:** The contents of the `href` in the anchor tag ("`<a>`") will be the
"Social Auth Login URL" that you constructed in the "Setting up an Okta Social
Authentication provider" instructions above.

    <button type="submit" class="btn btn-default">Log In</button>
    <a class="btn btn-social-icon btn-facebook" href="https://example.okta.com/oauth2/v1/authorize?idp=0oa0bcde12fghiJkl3m4&client_id=AbcDE0fGHI1jk2LM34no&scope=openid%20email%20profile&response_type=id_token&redirect_uri=https://app.example.com/social">
      <i class="fa fa-facebook"></i>
    </a>

## Update PythonApplication1.py

Lastly, add the code below to the `PythonApplication1.py` file, before the
line that looks like `sessionsClient = SessionsClient(base_url, api_key)`:

    import jwt
    import requests
    import base64
    from cryptography import x509
    from cryptography.hazmat.backends import default_backend
    jwks_url = "{}/oauth2/v1/keys".format(base_url)
    r = requests.get(jwks_url)
    jwks = r.json()
    x5c = jwks['keys'][0]['x5c'][0]
    pem_data = base64.b64decode(str(x5c))
    cert = x509.load_der_x509_certificate(pem_data, default_backend())
    
    
    client_id = 'AbcDE0fGHI1jk2LM34no'
    ngrok_url = 'https://abc123def.ngrok.io'
    public_key = cert.public_key()

Add this method to your `PythonApplication1.py` file, place it just
above the line that reads `@app.route("/secret")`

    @app.route("/social")
    def social_login():
        token = request.args.get('id_token', '')
        try:
            decoded = jwt.decode(
                token,
                public_key,
                algorithms='RS256',
                audience=client_id)
        except Exception, e:
            decoded = str(e)
            return decoded
        subject = decoded['sub']
        user = UserSession(subject)
        login_user(user)
        logged_in_url = url_for('logged_in', _external=True)
        return redirect(logged_in_url)