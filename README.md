# Exporting-and-Importing-Sites-and-App-Pools-from-IIS-7-and-7.5Exporting and Importing Sites and App Pools from IIS 7 and 7.5
 
When using multiple IIS server in a Load Balanced Environment it will  be alot of work to create all you’re website twice with the same settings on each webserver. Therefor it is possible to export and import you’re configuration from one webserver to the other. This will also be usefull when you’re upgrading from IIS 7 (Windows Server 2008) to IIS 7.5 (Windows Server 2008 R2).

When you create a website in IIS 7 or 7.5 a unique application pool will also be created and used by this website, that’s why you need to import these application pools first on the second webserver before importing the website(s).

To Export the Application Pools on IIS 7 : %windir%\system32\inetsrv\appcmd list apppool /config /xml > c:\apppools.xml

This will export all the application pools on you’re webserver, therefor you need to edit the apppools.xml and remove the application that you do not need to import for example:

DefaultAppPool
Classic .NET AppPool
SecurityTokenServiceApplicationPool
And other apppools that already exist on the second webserver, appcmd doesn’t skip already existing apppools, it just quit’s and doesn’t import any.

To import the Application Pools: %windir%\system32\inetsrv\appcmd add apppool /in < c:\apppools.xml

All the AppPools in the xml will be created on you’re second webserver.

To Export all you’re website: %windir%\system32\inetsrv\appcmd list site /config /xml > c:\sites.xml

This will export all the websites on you’re webserver, therefor you need to edit the sites.xml and remove the websites that you do not need to import for example:

Default Website
And all other websites that already exist on the second webserver.

To Import the website: %windir%\system32\inetsrv\appcmd add site /in < c:\sites.xml

It’s also possible to export a single website or application pool all you need to do is add the name of the Application Pool or Website to the command line:

To export/import a single application pool: %windir%\system32\inetsrv\appcmd list apppool “MyAppPool” /config /xml > c:\myapppool.xml

Import: %windir%\system32\inetsrv\appcmd add apppool /in < c:\myapppool.xml

To export/import a single website: %windir%\system32\inetsrv\appcmd list site “MyWebsite” /config /xml > c:\mywebsite.xml

Import: %windir%\system32\inetsrv\appcmd add site /in < c:\mywebsite.xml
