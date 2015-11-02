# Adding SSO support - SAML into an Okta managed application

In this guide, we will be updating the simple login page we just
created to enable SSO into SAML applications that are in your Okta
org. 

The two main things that we will be doing in this guide are as
follows:

1.  Changing the `PythonApplication1.py` code to use a
    `sessionCookieRedirect`, which will establish a user session on
    your Okta org.
2.  Adding an "app embed link" to the `secret.html` template, giving
    your users the ability to SSO into the Amazon Web Services Okta
    application that we set up earlier in the day.

## Update PythonApplication1.py

Replace the `main_page` method in `PythonApplication1.py` with the
code below:

    @app.route("/", methods=['GET', 'POST'])
    def main_page():
        opts = {}
        if request.method == 'GET':
            return render_template('main_page.html', opts=opts)
        session = None
        try:
            session = sessionsClient.create_session_with_cookie_token(
                request.form['username'],
                request.form['password'])
        except Exception, e:
            print e
            opts['invalid_username_or_password'] = True
            return render_template('main_page.html', opts=opts)
    
        user = UserSession(session.userId)
        login_user(user)
        logged_in_url = url_for('logged_in', _external=True)
        url = "{}/login/sessionCookieRedirect?token={}&redirectUrl={}".format(
            base_url,
            session.cookieToken,
            logged_in_url)
        return redirect(url)

## Update templates/secret.html

Replace the contents of the `templates/secret.html` file with the
HTML below.

For the contents of the `href` in the "Log in to AWS" anchor tag,
use the [app embed link](https://support.okta.com/help/articles/Knowledge_Article/27418177-Using-the-Okta-Applications-Page#Show) for the Amazon Web Services application that
you set up earlier.

    {% extends "base.html" %}
    {% block body %}
        <div class="container">
          <div>
            <h1>
              You are logged in!
            </h1>
          </div>
          <div class="row">
            <div class="col-md-6">
              <!-- Source: http://openclipart.org/detail/176289/top-secret-by-joshbressers-176289 -->
              <img src="http://joel.franusic.com/oktane15/static/top-secret.png"/>
            </div>
            <div class="col-md-6">
              <p>Logged in as User ID: {{ opts.user.user_id }}</p>
            </div>
            <div class="col-md-6">
              <a class="btn btn-default" href="https://example.okta.com/home/example/0ab1c2d3efGHIJKLMNOP/4567">
              Log in to AWS
              </a>
            </div>
          </div>
        </div>
    {% endblock %}

Extra Credit: How might you make this dynamic? Can you do that using only Javascript?