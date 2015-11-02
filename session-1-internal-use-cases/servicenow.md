# ServiceNow Okta Integration

## Log Into ServiceNow instance

Please note that this is a shared environment!

- Go to [ServiceNow login page] (https://undefined.service-now.com)
- Username: `oktanedemo`
- Password: `Oktane123$`

This lab exercise will take you through the following:

- Creation of a REST message to look up Groups from Okta
- Testing the REST message
- Create a new table in ServiceNow to handle several group-related attributes
- Enhance Test script to insert values returned from Okta into table

## Assume Security_Admin Role

- Click on the lock icon next to the username in top left corner.
- Select security_admin option and hit OK.

## Create REST Message

- Type "REST" in the upper left search field
- Click "Outbound->REST Message"
- Create a new REST Message by clicking on the "New" button
- Enter a unique name under "Name".  TBD during the session.  You need to write down the name entered as this will be needed later.
- Click the "lock" icon to open up the "Endpoint" option
	- Enter https://<your_org>.okta.com/api/v1/groups
- Under HTTP Headers - insert a new row
	- Under name - Enter "Authorization"
	- Under Value - Enter "SSWS <your org key>"

- To test the REST call, on the screen where you have defined the endpoint, scroll down to see the list of HTTP operations.  Click "get"
- Midway down the page, there is a "test" button.
- You should see the raw JSON returned from the query.

## Scripting Query

We will now test the query in a scripting env within ServiceNow

- Enter "Script" in the top left and look for "System Definition->Scripts - Background"
- Click on that and you should see a script testing env with the title "Run script (JavaScript executed on server)"
- Paste the following:

        var sm = new sn_ws.RESTMessageV2("<your REST msg name>","get");
        var response = sm.execute();
        var responseBody = response.getBody();
        gs.info(responseBody);
        var obj = new global.JSON().decode(responseBody);
        JSUtil.logObject(obj);

- Replace <your REST msg name> appropriately.
- Hit "Run Script"

## Create a table for storing Group Results

- Enter "tables" in the top left search field and look for "System Definition->Tables"
- Hit "New" to create a new table
- Enter a unique name - TBD during lab.  You need to write down the name entered as this will be needed later.
- Under "Columns" - insert 2 new rows.
- 1st row
	- label = "group id"
	- Type = String
	- Max Length = 255
- 2nd row
	- label = "group description"
	- Type = String
	- Max Length = 1024

- By default - the table name will be auto-generated with "u_" and the table label separated by underscore for the spaces
- eg. label="my table1" - the name will be "u_my_table1"
- Same goes with the column names.

Now - take a note of the table name and the column names as you will need them later.

## Write queried entries into the table

Go back to scripting environment.

- Enter "Script" in the top left and look for "System Definition->Scripts - Background"

- Click on that and you should see a script testing env with the title "Run script (JavaScript executed on server)"

You want to paste back what we had tested earlier to perform the query:

    var sm = new sn_ws.RESTMessageV2("<your REST msg name>","get");
    var response = sm.execute();
    var responseBody = response.getBody();
    gs.info(responseBody);
    var obj = new global.JSON().decode(responseBody);
    JSUtil.logObject(obj);
    
Remember to replace your REST msg name.  Now add the following below it.  This loops through the "obj" which carries the response from the REST API call - and inserts them to the table.  

Make sure you replace <your table name> appropriately.
    
    for(var i=0; i < obj.length; i++){
       var curObj = obj[i];
       var yourGR = new GlideRecord("<your table name>");
       yourGR.u_group_id = curObj.id;
       yourGR.u_group_description = curObj.profile.description;

       yourGR.insert();
    }
    
- You should again see the raw results of the query on the screen.
- Now on the left pane - search for your table name and click on it.
- You should see the results inserted into the appropriate columns!!!


