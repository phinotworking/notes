## To View list of existing app registrations
https://tenant.sharepoint.com/_layouts/15/AppPrincipals.aspx

## Create a new app registration
1. Log into your SharePoint Online site collection with an account that has site collection administrator access
2. Navigate to the App Registration page ~sitecollection/_layouts/15/appregnew.aspx

3. Click Generate on Client Id, take a note of the generated value.
4. Click Generate on Client Secret, take a note of the generated value
5. Give your app a suitable Title
6. App Domain and Redirect URI will depend on the type of application you are creating. 
If we are performing updates through server side code, then it doesn’t matter what we put in the App Domain and the Redirect URI
	
7. For now, just enter in localhost for the App Domain
8. Same with Redirect URI, just enter https://localhost/default.aspx for now
9. Click Create when done

## Assign permissions for the app
Now we need to make it so our Add-in has the needed permissions to perform what we intend it to do
1. Navigate to the App Registration page ~sitecollection/_layouts/15/appinv.aspx

IMPORTANT: If you want to only give access to a subweb or list, you need to invoke app registration on the context of that subsite!! Eg: ~sitecollection/subsite/_layouts/15/appinv.aspx

2. In the App Id field, enter the Client Id noted before.
3. Press Lookup and the rest of the fields should populate
4. Enter the following in the Permission Request XML box:
<AppPermissionRequests AllowAppOnlyPolicy="true">
    <AppPermissionRequest Scope="http://sharepoint/content/sitecollection" Right="FullControl"/>
</AppPermissionRequests>

Look at this for more details:
https://medium.com/ng-sp/sharepoint-add-in-permission-xml-cheat-sheet-64b87d8d7600


##References
https://www.ryanschouten.com/2017/01/31/manually-registering-a-sharepoint-add-in/
https://docs.microsoft.com/en-us/sharepoint/dev/sp-add-ins/add-in-permissions-in-sharepoint
[How can you update an existing app registration?](https://social.msdn.microsoft.com/Forums/SqlServer/en-US/08ad2d88-42b0-48dc-ac86-3d84f7f24f35/updating-existing-sharepoint-app-registration-details?forum=appsforsharepoint)
