---
title: Simple set up guide for getting Web Deploy 3.5 (msdeploy) on a remote server
date: 2016-08-03
published: true
tags: ['MSDeploy','IIS','devops']
canonical_url: false
description: "MSDeploy setup guide."
---

These steps were used to get MSDeploy 3.5 set up on Windows Server 2008 R2 two years ago. But I’ve resurrected them as they helped a colleague through the somewhat painful process yesterday!

Enjoy (with a very large cup of coffee):

1. Hit Start and search for ‘Turn Windows features on or off’
2. Select ‘Roles’ -> ‘Web Server (IIS)’ in the left menu and then right click and select ‘Add Role services’
3. Then in popup check the ‘Management Service’ box under Management Tools
4. Go to Management Service in IIS after clicking on the computer name node in the left column
5. Enable it and assign a self signed cert. I selected Windows or IIS manager credentials and left the default port
6. Install Web Platform Installer
7. Optional: Install Web Deployment Tool 2.1
8. Install Web Deploy 3.5
9. Create a user called msdeploy (or whatever you’ve set in your config) and make it an admin
10. Try going to https://SERVERIP:8172/msdeploy.axd in your browser. E.g. https://99.222.111.50:8172/msdeploy.axd
11. You may need to open up that port in the firewall depending on the current firewall rules
12. Login using user above and you should be directed to blank page if the login works — still a 404. Otherwise you will get a 401 if you cancel out of login box
13. You will need to give the msdeploy user file system access rights to the folder that you want to deploy to. This can be done by following the steps under ‘Configuring a deployment user’ heading on this post: http://blog.richardszalay.com/2013/02/02/building-a-deployment-pipeline-with-msdeploy-part-4-server-configuration/
14. You could need to make sure that the WMSvc has the privileges to use the runCommand provider. The correct way to do this give the msdeploy user “replace a process level token” rights using secpol.msc (local policies -> User Rights Management) — http://forums.iis.net/t/1182636.aspx
15. If that doesn’t work then we should override the privileges for WMSvc (Web Management Service) service to execute runCommand successfully. (See the ‘Troublehunting’ heading on this page http://tech-fellow.net/2012/12/07/deploy-windows-service-remote-machine-msdeploy/)
16. Make sure that the “Web Management Service” and “Web Deployment Agent Service” services are set to automatic start up

One useful thing to do if you are troubleshooting is running this in the console (instead of a batch file) and you’ll get a stack trace.

h3. Useful blog posts that I used as reference:

- *Best source:* http://blog.scnetstudio.com/post/2011/01/08/How-to-Configure-Windows-Server-2008-R2-to-support-Web-Deploy-(for-Web-Matrix).aspx
- http://tech-fellow.net/2012/12/07/deploy-windows-service-remote-machine-msdeploy/
- http://forums.iis.net/t/1200267.aspx
- http://stackoverflow.com/questions/4380819/msdeploy-runcommand-priviliges
- http://www.iis.net/learn/install/installing-publishing-technologies/installing-and-configuring-web-deploy
- http://stackoverflow.com/questions/13870561/getting-a-404-from-wmsvc-via-msdeploy-exe