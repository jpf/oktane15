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
URL that you constructed in the "Setting up an Okta Social
Authentication provider" instructions above.

    <button type="submit" class="btn btn-default">Log In</button>
    <a class="btn btn-social-icon btn-facebook" href="https://example.okta.com/oauth2/v1/authorize?idp=0oa0bcde12fghiJkl3m4&client_id=AbcDE0fGHI1jk2LM34no&scope=openid%20email%20profile&response_type=id_token&redirect_uri=https://app.example.com/social">
      <i class="fa fa-facebook"></i>
    </a>

## Update PythonApplication1.py

Lastly, add the code below to the top of the
`PythonApplication1.py`, add these lines just above the line that
reads `sessionsClient = SessionsClient(base_url, api_key)` 

    client_id = 'AbcDE0fGHI1jk2LM34no'
    public_key = """-----BEGIN PUBLIC KEY-----
    ABCDEfGHIjklmnoP0q1RSTUVWXYZAB2CDEFGHiJKLMNO3pqRsTUvWxy4z5aBCdEF
    GhijKLMN6OP7qRstuVWx8yZAbcdefGHIJklmnOPQrsT9uVWx0Yzab1C2DEF3gh+4
    IJ5KL6mN7OP8qrsTuvwx9yzABCdEfgh0ij1KL2mn3oPQR4s5T+UVWXy67z89Abcd
    0EFG+hiJklmno1PqRS$TuvWxyZA2BC+deFghIjKLm345NopQrStU6vwXYZAb7cde
    &FgHI+8j9KlMnOpQrsTUVWXyz0AbcdeFGh1IJklmnopqrS2Tu3vWx456Yzab78c9
    dEFGHIJ0KLmnOPQRStu1vwxyZ2AB3cDeF4GHiJkl5MnOP6qRST7uVwxyZA8bc9d0
    efGHIJKL
    -----END PUBLIC KEY-----
    """

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