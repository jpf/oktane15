# Introduction

In this guide, you will learn how to configure your Okta org with
Single Sign-On access to Amazon Web Services ("AWS"), once you've
done that, you will learn how to use an AWS CLI tool which will
allow you to authenticate the AWS CLI tools against Okta with MFA.

# Make sure that you have an Amazon Web Services account

This lab requires the use of an AWS account. We suggest that you
[create new AWS account](https://portal.aws.amazon.com/gp/aws/developer/registration/index.html?nc2=h_ct) for the purposes of this lab. You will be
configuration administrator access to your AWS account and it's best
to do this in a non-production environment.

# Setting up Amazon Web Services in Okta

Before you can use the Okta AWS client with AWS, you will need to
add the "Amazon Web Services" application to your Okta org.

Here is how to do that:

-   From the Okta Admin view, select "Applications" from the
    "Applications" menu
-   Click the "Add Application" button
-   Type "Amazon Web Services" into the search box labeled "Search for
    an application", an "Amazon Web Services" choice will appear,
    click the "Add" button to the right of the "Amazon Web Services"
    choice.
-   You will be taken to the "General Settings" section of the "Add Amazon
    Web Services" application wizard, click the green "Next" button.
-   You will be taken to the "Provisioning" section of the application
    wizard. Click the green "Next" button.
    
    (We will return to this later)
-   You will be taken to the "Assign to People" section of the application wizard,
    click the green "Next" button.
    
    (We will return to this later)
-   You should see a dialog that says "Setup is complete. No users
    will be assigned this application now." Click the green "Done" button.
-   You should now see the "Amazon Web Services" configuration page,
    click the "Sign On" tab, then click the "Edit" button in the
    "Settings" area.
-   Click on the "View Setup Instructions" button in the dialog with
    the text "SAML 2.0 is not configured until you complete the setup
    instructions."
-   A new page should have opened titled "Setup SSO".
-   Follow the instructions in the "How to Configure SAML 2.0 for
    Amazon Web Service" page that just opened.
    -   The document refers to a "Security Credentials" section, this
        section is now named "Identity & Access Management", it is in the
        "Security & Identity" section that is in the lower part of the
        middle column of services
    -   The "Create SAML Provider" button that this document refers to
        is now named "Create Provider"
    -   In the "Configure Provider" section:
        -   Set the "Provider Type" to "SAML"
        -   Set the "Provider Name" to "Okta"
        -   Copy the XML in the document into a `.xml` file and then
            upload the `.xml` document that you created using the "Choose
            File" button.
            
            (I suggest opening "Notepad.exe" pasting in the XML and then
            saving the file to your desktop named `aws-metadata.xml`)
    -   Click the blue "Next Step" button
    -   On the "Verify Provider Information" page, click the blue
        "Create" button.
    -   On the "Providers" page, click on the "Okta" name.
    -   Copy the "Provider ARN" into your clipboard. Switch back to
        "Amazon Web Services" configuration page in your Okta org, then
        paste the Provider ARN into the "Identity Provider ARN (Amazon
        Resource Name)" field on your Okta org.
    -   Switch to the AWS page, then continue following **Step 4** in the
        "How to Configure SAML 2.0 for Amazon Web Service" page.
        -   Name this role "WebSSO"
        -   When prompted, select "AdministratorAccess" as the policy to
            attach to the "WebSSO" role.
            
            **WARNING** This is *not* what you would want to do in an
            production environment. This is done here for *demonstration
            purposes only*.
    -   Continue following **Step 8** in the
        "How to Configure SAML 2.0 for Amazon Web Service" page.
        -   You *must* create an "Access Key" and "Secret Key" as part of
            **Step 9**. When you create a user, I suggest using the
            "okta-provisioning" as the "User Name"
        -   When creating the "okta-provisioning" user, make sure to copy
            the "Access Key" and "Secret Key"! You will have *only one chance* to
            get this data.
        -   After creating the "okta-provisioning" using, click on the
            user name from the "Users" section, then click on the blue
            "Attach Policy" button.
        -   Select the "AdministratorAccess" policy, then click the blue
            "Attach Policy" button.
        -   Copy the "Access Key" and "Secret Key" into the "Amazon Web
            Services" configuration page in your Okta org, then click the
            green "Save" button at the bottom.
-   Click on the "Provisioning" tab in the "Amazon Web Services"
    configuration page.
-   Click on the "Edit" button in the "Provisioning" section
-   Click the check box next to "Enable provisioning features"
-   Paste in the "Access Key" and "Secret Key" that you created for
    the "okta-provisioning" AWS user into the appropriate fields in
    the "Provisioning" section
-   Click the "Test API Credentials" button
-   You should see a message that says "Amazon Web Services was verified successfully!"
-   Scroll down and click the check boxes next to "Create Users" and
    "Update User Attributes", then click the green "Save" button.
-   Click the green "Save" button on the Okta configuration page
    when you are done.
-   Click on the "People" tab in the "Amazon Web Services"
    configuration page.
-   Click the green "Assign to People" button
-   Click the "Assign" button next to the user that you want to assign
    the "Amazon Web Services" application to
    -   Select the Role type as "WebSSO"
    -   **Important**: Click the check box next to the "WebSSO" SAML User Role
-   Click the green "Save and Go Back" button
-   Click the green "Done" button
-   In the Okta Admin view, click on the "My Applications" link at the
    top right side of the admin page
-   Click on the "Amazon Web Services" box ("chiclet"), you should be
    logged in to AWS now, as a read-only user.
    
    **NOTE:**
    If you get an error that starts with "Your request included an
    invalid SAML response.", then make sure that the user you assigned
    the "Amazon Web Services" app to is in the "WebSSO" role.

# Setting up the Okta AWS CLI client

-   Navigate to `C:\Users\Administrator\AWS` in Windows Explorer
-   Double-click on the file `oktaAWSCLI` with the Type of "XML
    Configuration"
    
    (If prompted to register by Visual Studio, click "Not now, maybe later", then click "Start")
-   From your web browser:
    -   From your Okta Admin view, select "Applications" from the "Applications" menu
    -   Click on your "Amazon Web Services" Application
    -   Click on the "General" tab
    -   Scroll down to the "App Embed Link" section
    -   Copy the "EMBED LINK" to your clipboard.
-   From Visual Studio
    -   Paste the "App Embed Link" as the value for "OKTA\_AWS\_APP\_URL"
    -   Change the value of "OKTA\_ORG" to be the same as your Okta org, make sure the domain is "oktapreview.com"!
    -   Click the "Save" icon
-   From your Command Prompt
    -   Type `cd AWS`
    -   Type `java -jar oktaAWSCLI.jar`
    -   Enter in your username and password
    -   Type `aws ec2 describe-instances`
        If everything is set up correctly, you will see output that
        looks like this:
        
            {
                "Reservations": []
            }

# Enable MFA in Okta

In this final section, you will learn how to enable MFA for your
Okta org and see how that works with the AWS CLI client.

First we will enable MFA on your Okta org and then we will log in
with the AWS CLI client again to see how MFA works with with CLI
client.

1.  Enable MFA in Okta
    
    Follow the Okta Help Center instructions for [Configuring
    Org-Level Multifactor Authentication](https://support.okta.com/help/articles/Knowledge_Article/27315047-Configuring-Multifactor-Authentication#AboutMFA).
    
    We suggest using Google Authenticator as your factor, since
    Google Authenticator compatible clients are available for [a wide
    range of devices](https://en.wikipedia.org/wiki/Time-based_One-time_Password_Algorithm#Client_implementations). 
    
    When configuring the "Okta Sign On Policy", unselect the option
    for "Only enforce MFA when users are off network", so that you
    are prompted every time you log in.
2.  Using the AWS CLI client with MFA
    
    Once you have configured MFA for your Okta org, enroll your user
    in MFA by logging out, then logging back in.
    
    After logging in to your Okta org successfully, re-run the
    `java -jar oktaAWSCLI.jar` command above.
    
    This time, when you run the command, you should see output like
    this:
    
        Username: user@example.com
        Password: 
        
        Multi-Factor authentication required. Please select a factor to use.
        Factors:
        [ 1 ] :Google Authenticator
        Selection: 1
        
        GOOGLE Token Factor Authentication
        Enter 'change factor' to use a diffrent factor
        Token: 012345
        
        Please choose the role you would like to assume: 
        [ 1 ]: arn:aws:iam::012345678901:role/WebSSO
        Selection: 1
    
    At this point, you have configured access to the AWS web
    interface and CLI via Okta. Feel free to try out the commands you
    would normally run via the AWS CLI client, if you haven't used
    the AWS CLI before, the Amazon documentation on the
    [AWS Command Line Interface](https://aws.amazon.com/cli/) is a great place to start.
    
    Ideas of things to try using the AWS CLI:
    
    -   Launch an EC2 instance and view the console from the command line
    -   Upload some files to S3