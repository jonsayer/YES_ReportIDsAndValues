# YES_ReportIDsAndValues
This simple tool allows you to look up the IDs and other field values of records based on another unique/semi-unique field such as an email address or external ID.

Created October-November 2018 by Jon Sayer of YES! Media. 
Offered to the nonprofit Salesforce community as open source software. Please make it better.

I have blatantly ripped off some code from these fine sources:
 - Importing & reading a CSV: 			https://sfdcsrini.blogspot.com/2014/09/data-import-from-csv-using-visualforce.html
 - Generating list of object fields: 	https://developer.salesforce.com/forums/?id=9060G0000005UGKQA2
 - Sorting a Select list by value: 		https://success.salesforce.com/apex/ideaView?id=08730000000BrX6AAK

## Example uses
There are many situations when a Salesforce admin would need to figure out the IDs of a large number of records. This tool is meant to meet those needs without requiring expensive software or apps like DemandTools.

For example:

- You get a large list of email addresses that need to be added to your newsletter list. Looking up contacts by email address, returning Contact ID and field values regarding their email newsletter subscription.
- After a migration, you realize you need to import some data, but you only have the old system's IDs. You have this field saved on your new records as an external ID. 
- And many more!

## How to install
If you are familiar with how to create a Visualforce page with a custom controller, and deploy that to production, this should be easy. 

If not... In a future version, I hope to include a package that will allow for easy deployment. In the meantime, you will need to install this code manually. If you've never done this, I recommend talking to someone who has done it before who might be able to walk you through the steps. 

In summary, you will need to:

1. Log in to a sandbox instance and open developer console. 
2. Create three new files:
   - Visualforce Page: YES_ReportIDsAndValues
   - Apex Class: YES_CTRL_ReportIDsAndValues
   - Apex Class: YES_TEST_ReportIDsAndValues
3. Copy/Paste the code from this repository into the appropriate new files. 
4. Create an outbound change set and add these files to it.
5. Upload that change set to your production instance. 
6. In production, select this change set and deploy it. 
7. When selecting the Apex tests to run, select "Run Specific Tests" and paste the name of our test class into the text box: YES_TEST_ReportIDsAndValues
8. Wait for the tests to run. 
9. In both Lightning and Classic, you will need to add a custom tab and add it to your app to access the Visualforce page through normal navigation.

## How to use
1. Prepare a .csv file with your input data.
   - The first row should be a header row.
   - The first column should contain your search values. For example, if you are searching for Contacts by their email address, the email address should be in this column.
   - You should deduplicate your data in your spreadsheet editor before loading it in to this application. Duplicate search queries will not return additional results.
   - You can include as many additional columns as you need. These will be included in the output file. Note: for large jobs, tons of additional fields could cause you to reach page size limits.
   
   - NOTE: If a search value matches multiple records (eg. multiple contacts with the same Email Address), these will be returned on multiple lines of the output file. 
2. In Salesforce, navigate to the Visualforce page. It should be at https://yoursalesforceaddress.com/apex/YES_ReportIDsAndValues
3. Select the query object. Your output file will include records of this object.
4. Select the query field. You will be matching records based on values in this field.
5. Select Additional Fields to show in Output File. This tool will always return the record ID. Additional fields selected here will also be returned in your output file.
6. Upload your query csv file and click "Run Query".
   - If you experience an error that says you reached page size limits, this is happening because the size of the outputted csv is too large. Visualforce pages are limited to 132kb. Consider breaking your file up in to smaller batches of ~500 records or less, or reducing the number of fields either in your input file or that you are returning from matched records. 
   - Likewise, if you experienced an Apex CPU Time Limit error, Salesforce is trying to match too many records. You will need to break your file up in to smaller batches of ~500 records or less.
7. If everything worked, you should see a link that says "Click to Download Results". Your result file will include the columns from your original file alongside the Salesforce ID of the matched record(s) and the additional fields you selected. 

## Known Limitations (as of 11/9/2018)

Some things to be aware of if you are thinking of using this tool instead of something fancier. 

- It doesn't work in Lighting.
- Visualforce pages are limited to 132kb, meaning you can't use this tool for extremely large jobs.
- No "Scenarios" or reusable settings like Data Loader or DemandTools.
- Can only look up records by one field value, and it can't do fuzzy lookups or operations. The single field limitation might be overcomeable by using formula fields in your input data and on the Salesforce records being queried.
