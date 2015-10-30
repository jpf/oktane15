# Powershell

In this section we will learn about using Okta’s PowerShell SDK to
manage users from Windows PowerShell. Using what you learn in this
session, you will be able to create PowerShell Scripts to help
automate parts of your workflow.

## Getting started

Before you get started, you will need to create a test user in your
Okta org. Here is how to do that: 

1.  Create an Okta API token by following the Okta guide for
    "[Getting a token](http://developer.okta.com/docs/api/getting_started/getting_a_token.html)"
    -   Name this token "Oktane15 Platform Lab".
    -   Copy this token to somewhere (like Notepad). You’ll be using
        this API token throughout this lab.
2.  Create a test user to use for this lab
    -   From the "Admin" view of your Okta org, do the following:
    -   From the "Directory" menu, select "People"
    -   Click the "Add Person" button
    -   Enter "Betty" as the "First name"
    -   Enter "Whitehead" as the "Last name"
    -   Enter "bwhitehead@example.com" in the "Username" field.
    -   Enter one of your address into the  "Primary email" field. If you aren’t comfortable using your own email address you can use the Mailinator service: <http://mailinator.com>
    -   Select the "Send user activation email now" box.
    -   Check your email, open the "Welcome to Okta!" message.
    -   Click the link under the text "Click the following link to activate your Okta account"
    -   Use "Testing123" as the password.
    -   Use "Surströmming" as the "food you liked least as a child"
    -   Select a picture of a security image.
    -   Click "Create My Account"

## Using PowerShell

Now that you've created the "Betty Whitehead" test user, you are
ready to start using PowerShell! Below we will show you how to change
Betty's last name in Okta, disable her account, and re-enable it.

1.  Set up PowerShell
    
        > Import-Module Okta.Core.Automation
2.  Configure the Okta PowerShell module
    
        > Connect-Okta -Token "your-token" -FullDomain "https://your-subdomain.okta.com"
3.  Search for an existing user
    
        > $user = Get-OktaUser bwhitehead
4.  Type "`$user`" into PowerShell
    
    You should see:
    
        PS C:\Users\Administrator> $user
        
        Status                : STAGED
        Created               : 11/2/2015 5:14:52 PM
        Activated             : 1/1/0001 12:00:00 AM
        StatusChanged         : 1/1/0001 12:00:00 AM
        LastLogin             : 1/1/0001 12:00:00 AM
        LastUpdated           : 11/2/2015 5:14:52 PM
        PasswordChanged       : 1/1/0001 12:00:00 AM
        TransitioningToStatus :
        Profile               : Okta.Core.Models.UserProfile
        Credentials           : Okta.Core.Models.LoginCredentials
        Id                    : 00u01ab2cdefghi3j4k5
5.  Type "`$user.Profile`", then hit the "Enter" key
    
    You should see something like this:
    
        PS C:\Users\Administrator> $user.Profile
        
        Login          : betty.whitehead@example.com
        Email          : betty.whitehead@example.com
        FirstName      : Betty
        LastName       : Whitehead
6.  Update Betty's user account
    
    Run the commands below to change the user information for Betty:
    
        PS C:\Users\Administrator> $user.Profile.LastName = "Willis"
        PS C:\Users\Administrator> $user.Profile.MobilePhone = "702-387-6366"
    
    Then, run this command to save the changes back to your Okta org:
    
        PS C:\Users\Administrator> Set-OktaUser $user
7.  Find "bwhitehead" user and verify that her name has been updated
    
    Do the following from the Admin User Interface of your Okta org:
    
    -   From the "Directory" menu, select "People".
    -   Type "Betty" in to the search box.
    -   Verify that a user named "Betty Willis" appears, then click on
        her name.
8.  Deactivate Betty’s user account.
    
        PS C:\Users\Administrator> Disable-OktaUser bwhitehead
9.  Reload the page for the "bwhitehead" user, page that you opened above.
10. Verify that the text under "Betty Willis" reads "Deactivated"
11. Activate user Betty’s user:
    
        PS C:\Users\Administrator> Enable-OktaUser bwhitehead
12. Reload the page for the "bwhitehead" user, page that you opened above.
13. Verify that the text under "Betty Willis" reads "Password reset. The user is now in one-time password mode"