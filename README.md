# Oktane15 Platform Lab

The Okta platform makes many powerful features available through
our APIs and SDKs.  Whether you are managing authentication and 
application access for the employees of your enterprise, or
providing the identity infrastructure for other business units
of your company to launch their business initiatives, or
providing federated authentication and provisioning support in
your cloud offering for your valuable customer - this session
will provide deep insights and hands-on labs to demonstrate some
advanced usages of the platform.

# Session plan

This lab is split into two sessions, a morning session covering
internal use cases and an afternoon session covering external use
cases.

Click on the title of each session to see the detailed guide for
that session.

## Morning Session - Internal Use Cases

-   <a href="session-1-internal-use-cases/using-powershell.md">Using PowerShell with Okta</a>
-   <a href="session-1-internal-use-cases/using-python-to-run-reports-from-okta.ipynb">Using Python to run reports from Okta</a>
-   <a href="session-1-internal-use-cases/aws-cli.md">Using the AWS CLI with Okta</a>
-   <a href="session-1-internal-use-cases/splunk-app-for-okta.md">Splunk App for Okta</a>
-   <a href="session-1-internal-use-cases/servicenow.md">ServiceNow</a>

## Afternoon Session - External Use Cases

-   <a href="session-2-external-use-cases/external-use-case-setup.md">Setup</a>
    
    This step covers installing the tools necessary for writing an
    Python Flask application in Visual Studio and includes these
    steps:
    
    -   Installing Flask and PyJWT
    -   Installing "Python Tools for Visual Studio"
-   <a href="session-2-external-use-cases/build-a-simple-login-page.md">
    Building a simple login page
    </a>
    
    Starting from scratch, we’ll build a simple Login Page using the
    Okta API in Python.
-   <a href="session-2-external-use-cases/adding-sso-support.md">
    Adding SSO support - SAML into an Okta managed application
    </a>
    
    This exercise will leverage your earlier AWS IAM setup to show how
    you can SSO directly into an app in a branded end user flow
    leveraging our APIs.  It will also look into SAML specifics
    including IDP and SP initiated logins and `RelayState`.
-   <a href="session-2-external-use-cases/adding-social-login-to-the-login-page.md">
    Adding Social Login to the login page</a>
    
    Building on the previous exercise, we’ll add Social Login support
    to your page.  If you think about B2C use cases, this is almost a
    pre-requisite today to allow registration and authentication –
    using Social Identity Providers like Facebook and Linkedin.
    
    Our Social Identity Provider offering also introduces webhooks for
    the first time.  It is not part of the lab – but we will discuss
    how webhooks are used in the context of our social
    authentication/registration flow.

-   <a href="session-2-external-use-cases/introducing-the-okta-sign-in-widget.md">
    Introducing the Okta Sign-In Widget</a>
    
    You will get a sneak peek of our new Okta Sign-In Widget.  All the
    work that you’ve done today – condensed into a Java-script based
    implementation.  The widget supports credentials validation,
    password management, MFA, Social Auth – and much more.