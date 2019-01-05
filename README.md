# sharepoint

VSTS Auto Deployment for SPFx package using SharePoint App ID Secret
1. Create a SharePoint App ID and Secret using the url https://tenant.sharepoint.com/sites/sitename/_layouts/appregnew.aspx
2. Save the App ID and Secret 
3. Grant Full Control Permission to the app on the site (for site collection levels apps)/ Tenant App catalog site (for tenant level apps)		
	1. Go to https://tenant.sharepoint.com/sites/sitename/_layouts/appinv.aspx
	2. Enter App ID saved in step no. 3 and hit lookup
	3. Enter following permission xml
          <!--<AppPermissionRequests AllowAppOnlyPolicy="true">'
                <AppPermissionRequest Scope="http://sharepoint/content/sitecollection" Right="FullControl"/>
          </AppPermissionRequests>-->
  	4. Click Ok	
4. Got to VSTS Pipelines
5. Create Release Pipeline
6. Remove existing tasks and add 2 powershell tasks
7. For 1st task add commands to Install Pnp module
8. For second powershell Task add the following Inline task
    --> Connect-PnPOnline  -AppId $AppId -AppSecret $AppSecret -Url $SiteUrl
    --> Add-PnPApp -Path $packagePath -Scope Site -Publish -Overwrite  ########## for site collection apps ###############
                                  or 
    --> Add-PnPApp -Path $packagePath -Publish -Overwrite  ########## for tenant level apps ##################
9. Save the Pipeline.
