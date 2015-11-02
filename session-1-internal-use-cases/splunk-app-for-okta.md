# Splunk App for Okta

As an Okta customer, your Okta instance captures valuable
information regarding user activities, and application access over
time – including details about devices, location, browser
information, etc. This app collects data through the Okta platform
APIs with the primary focus on system log events. The app can also
present the “latest snapshot” on users, groups and apps.

Out-of-the-box dashboards are provided to give immediate insights on
your Okta data based on events.

## Installing Splunk Packages

To use Splunk App for Okta, you are required to install two packages

- Splunk App for Okta
  - This contains the pre-defined dashboards and reports for Okta
- Splunk Add-on for Okta
  - This contains the connector portion of where connection to Okta API, pull interval, start date, end date, etc is defined.

The image you have has Splunk on-prem pre-installed.  We have installed the two modules - but there is an update required for this demo.

## Updating the Splunk Add-On for Okta

Download the following file to your downloads folder:

[Splunk_TA_okta-1.0.0-102.spl](https://drive.google.com/file/d/0Bwd9pF_PO-_nLTlydlJ5Y0w3c2c/view)

- Go to FireFox and open [Splunk's Login page](https://localhost:8000/en-US/account/login)
- Username: `admin`
- Password: `OktaSplunk`
- Click on the gear sign next to "App"
- Click on "Install App from file" button.
- Click "Browse" and select the file that you had just downloaded above - Splunk_TA_okta-1.0.0-102.spl
- Make sure you check the box that says - "Upgrade app" as we will be upgrading the app from the version that is on the VM currently.
- You will be prompted to restart.  Follow the instruction for restart.  In a minute or less, it should tell you that upgrade as completed and you are free to use the instance.

Once the apps are installed, we are now ready to set up the connection with Okta to start the ETL

- Go to Settings (top right)->Data Inputs
- On the page, select the Splunk Add-On for Okta and "New" to create a new connection
- Parameters:
   - Okta Server - Full URL for your Okta Service (will be provided during lab)
   - API Token - API token obtained from Okta admin console (will be provided during lab)
   - Interval - default is `3600` - turn it to `60`.  This means every 60 seconds, Splunk will do a pull from Okta
   - Start time - You should have this set to `2015`
   - Leave everything else so default
   - Now save.

To check if data is coming in, go back to Splunk instance homepage and search for `index="main"`.  You should see data coming in.

Once you've completed this step.  Wait a bit for the data to be fully synchronized.  A rough gauge is when the last event pulled is from today.
