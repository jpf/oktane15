# Building a simple login page

Now it's time to move beyond the "Hello World" example that we built
in the previous step.

In this section, you will need to create a folder named `templates`
inside of your `PythonApplication1` project, then you will need to
create three `.html` files inside of the `templates` folder:

-   `base.html`
-   `main_page.html`
-   `secret.html`

Then, once you've created those `.html` files you will need to
update the `PythonApplication1.py` file with the logic needed to
make a simple login page.

The contents that of each of those files should contain is
below. Create the `templates` folder, then create each of the
required `.html` files and copy the code below into each appropriate
file.

## templates/base.html

    <!DOCTYPE html>
    <html lang="en">
      <head>
        <meta charset="utf-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1">
        <!-- The above 3 meta tags *must* come first in the head; any other head content must come *after* these tags -->
        <title>Okta Authentication Example</title>
    
        <!-- Bootstrap core CSS -->
        <link href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.5/css/bootstrap.min.css" rel="stylesheet" integrity="sha256-MfvZlkHCEqatNoGiOXveE8FIwMzZg4W85qfrfIFBfYc= sha512-dTfge/zgoMYpP7QbHy4gWMEGsbsdZeCXz7irItjcC3sPUFtf0kuFbDz/ixG7ArTxmDjLXDmezHubeNikyKGVyQ==" crossorigin="anonymous">
    
        <!-- HTML5 shim and Respond.js for IE8 support of HTML5 elements and media queries -->
        <!--[if lt IE 9]>
          <script src="https://oss.maxcdn.com/html5shiv/3.7.2/html5shiv.min.js"></script>
          <script src="https://oss.maxcdn.com/respond/1.4.2/respond.min.js"></script>
          <![endif]-->
        <!-- 60px to make the container go all the way
          to the bottom of the topbar -->
        <style>
          body { padding-top: 60px; }
        </style>
        <link href="https://maxcdn.bootstrapcdn.com/font-awesome/4.4.0/css/font-awesome.min.css" rel="stylesheet" integrity="sha256-k2/8zcNbxVIh5mnQ52A0r3a6jAgMGxFJFE2707UxGCk= sha512-ZV9KawG2Legkwp3nAlxLIVFudTauWuBpC10uEafMHYL0Sarrz5A7G79kXh5+5+woxQ5HM559XX2UZjMJ36Wplg==" crossorigin="anonymous">
        <link rel="stylesheet" href="https://lipis.github.io/bootstrap-social/bootstrap-social.css">
      </head>
    
      <body>
    
        <nav class="navbar navbar-inverse navbar-fixed-top">
          <div class="container">
            <div class="navbar-header">
              <button type="button" class="navbar-toggle collapsed" data-toggle="collapse" data-target="#navbar" aria-expanded="false" aria-controls="navbar">
                <span class="sr-only">Toggle navigation</span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
              </button>
              <a class="navbar-brand" href="#">Okta Authentication Example</a>
            </div>
            <div id="navbar" class="collapse navbar-collapse">
              <ul class="nav navbar-nav">
                {% if 'user' in opts and opts['user'].is_authenticated() %}
                <li><a href="{{ opts.okta_url }}">Dashboard</a></li>
                <li><a href="/logout">Log out</a></li>
                {% endif %}
              </ul>
            </div><!--/.nav-collapse -->
          </div>
        </nav>
        <div class="container">
          {% block body %}
          {% endblock %}
        </div><!-- /.container -->
        <!-- Bootstrap core JavaScript
        ================================================== -->
        <!-- Placed at the end of the document so the pages load faster -->
        <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.11.3/jquery.min.js"></script>
        <script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.5/js/bootstrap.min.js" integrity="sha256-Sk3nkD6mLTMOF0EOpNtsIry+s1CsaqQC1rVLTAy+0yc= sha512-K1qjQ+NcF2TYO/eI3M6v8EiNYZfA95pQumfvcVrTHtwQVDG+aHRqLi/ETn2uB+1JqwYqVG3LIvdm9lj6imS/pQ==" crossorigin="anonymous"></script>
      </body>
    </html>

## templates/main\_page.html

    {% extends "base.html" %}
    {% block body %}
          <div class="row">
            <div class="col-md-6">
              <p>
              This is a demonstration of how to use Okta to handle authentication for a web application.
              </p>
              <!-- Source: -->
              <!-- http://openclipart.org/detail/34273/tango-system-lock-screen-by-warszawianka -->
              <img src="http://joel.franusic.com/oktane15/static/locked-screen.png" alt="Locked computer screen"/>
            </div>
            <div class="col-md-6">
              {% if opts['invalid_username_or_password'] %}
              <div class="alert alert-danger" role="alert">
                <button type="button" class="close" data-dismiss="alert">&times;</button>
                Incorrect Username or Password.
              </div>
              {% endif %}
              {% if opts['account_not_active'] %}
              <div class="alert alert-error">
                <button type="button" class="close" data-dismiss="alert">&times;</button>
                Account not active. Please contact your account administrator.
              </div>
              {% endif %}
              <form method="POST">
                <div class="form-group">
                  <label for="username">Username</label>
                  <input type="email" class="form-control" name="username" placeholder="user@example.com">
                </div>
                <div class="form-group">
                  <label for="password">Password</label>
                  <input type="password" class="form-control" name="password" placeholder="">
                </div>
                <button type="submit" class="btn btn-default">Log In</button>
              </form>
            </div>
          </div>
    {% endblock %}

## templates/secret.html

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
          </div>
        </div>
    {% endblock %}

## PythonApplication1.py

    import os
    
    from flask import Flask
    from flask import redirect
    from flask import render_template
    from flask import request
    from flask import url_for
    from flask.ext.login import LoginManager
    from flask.ext.login import current_user
    from flask.ext.login import login_required
    from flask.ext.login import login_user
    from flask.ext.login import logout_user
    from okta import SessionsClient
    
    app = Flask(__name__)
    
    # NOTE: Change this to something else!
    app.secret_key = 'change-me'
    
    # NOTE: If you aren't storing your Okta Base URL or API Key in environment
    # variables, then replace these like so:
    #    base_url = 'https://example.okta.com'
    #    api_key = '01Ab2cDE-fghIJ3KLmNoP-Qr4stu5VwXYZAbcDeFGH'
    # Don't forget to replace the contents of the variables with real values!
    base_url = 'https://example.okta.com'
    api_key = '01Ab2cDE-fghIJ3KLmNoP-Qr4stu5VwXYZAbcDeFGH'
    
    sessionsClient = SessionsClient(base_url, api_key)
    
    login_manager = LoginManager()
    login_manager.setup_app(app)
    
    
    class UserSession:
        def __init__(self, user_id):
            self.authenticated = True
            self.user_id = user_id
    
        def is_active(self):
            # In this example, "active" and "authenticated" are the same thing
            return self.authenticated
    
        def is_authenticated(self):
            # "Has the user authenticated?"
            # See also: http://stackoverflow.com/a/19533025
            return self.authenticated
    
        def is_anonymous(self):
            return not self.authenticated
    
        def get_id(self):
            return self.user_id
    
    
    @login_manager.user_loader
    def load_user(user_id):
        print "Loading user: " + user_id
        return UserSession(user_id)
    
    
    @app.route("/", methods=['GET', 'POST'])
    def main_page():
        opts = {}
        if request.method == 'GET':
            return render_template('main_page.html', opts=opts)
    
        print request.form
    
        session = None
        try:
            session = sessionsClient.create_session(
                request.form['username'],
                request.form['password'])
        except Exception, e:
            print e
            opts['invalid_username_or_password'] = True
            return render_template('main_page.html', opts=opts)
    
        user = UserSession(session.userId)
        login_user(user)
        logged_in_url = url_for('logged_in', _external=True)
        url = logged_in_url
        return redirect(url)
    
    
    @app.route("/secret")
    @login_required
    def logged_in():
        opts = {'user': current_user}
        return render_template('secret.html', opts=opts)
    
    
    @app.route("/logout")
    def logout():
        logout_user()
        return redirect(url_for('main_page'))
    
    if __name__ == "__main__":
        port = int(os.environ.get('PORT', 5000))
        if port == 5000:
            app.debug = True
        app.run(port=port)