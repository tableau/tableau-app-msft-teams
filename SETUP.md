[![Tableau Supported](https://img.shields.io/badge/Support%20Level-Tableau%20Supported-53bd92.svg)](https://www.tableau.com/support-levels-it-and-developer-tools)
# Setup Guide

## Prerequisites

In order to use the Tableau App for Microsoft Teams, you need a few things:

* **Tableau Cloud** or **Tableau Server** \- You will need to be an admin on your Tableau Cloud site or Tableau Server.  We use [connected apps](https://help.tableau.com/current/online/en-us/connected\_apps\_direct.htm) to provide a single-sign on experience, so we’ll need a Tableau admin to set that up.  If using Tableau Server, there are some additional considerations:
  * Tableau Server must be running version 2023.1 or newer.  
  * The Tableau app communicates with Tableau Server via REST API calls.  If your Tableau Server is behind a firewall, you must provide access from the Tableau teams app (```tableau-ms-teams-prod-gycea7csh5hsbfh5.a02.azurefd.net```)
  * The Tableau app uses the Embedding API to embed interactive dashboards from Tableau Server.  So your end users will need to be able to access Tableau Server in the same way they access Microsoft Teams.  The specific requirements will vary depending on your implementation of Microsoft Teams, for example if users must be on the VPN to use Teams then their Tableau access needs to match that.  Or if they access Teams through the public internet, then Tableau would also need to be accessible through the public internet.  Keep in mind Teams also has mobile apps, so the users may try and access Tableau Server from their mobile devices as well.
* **Microsoft E365 Subscription** \- The Tableau app is built to work within Microsoft Teams, so you will need an [E365 subscription](https://www.microsoft.com/en-us/microsoft-365/products-apps-services).  Depending on your organization’s [policies](https://learn.microsoft.com/en-us/microsoftteams/app-policies), you may also need to be an E365 admin in order to install the app.  
* **Microsoft Teams** \- In order to use the Tableau app, you will need access to Microsoft Teams.  You can use either the [Teams client app](https://www.microsoft.com/en-us/microsoft-teams/download-app) or login to Teams using your [web browser](https://teams.microsoft.com/).  
* **User mappings** \- The end users of the Tableau app will already be licensed for Microsoft Teams, but they will also need licenses for Tableau.  Further, the user’s email or UserPrincipalName in Azure AD (used by Teams) must match the username in Tableau (for single-sign on)

# Step 1: Get the Tableau App manifest

If you are using Tableau Cloud, you can install the Tableau Cloud app for Teams in the [Microsoft Teams Marketplace](https://marketplace.microsoft.com/en-us/product/office/wa200007445?tab=overview).

If you are using Tableau Server, download the manifest (zip file) titled ‘tableau-server-app-for-teams.zip’ from [this repository](https://github.com/tableau/tableau-app-msft-teams/raw/main/appManifest/tableau-app-for-teams-server.zip).  Before we can use it though, we’ll have to make some edits.  Unzip the file, and open up manifest.json.  Search the manifest.json file for all occurrences of ```*.online.tableau.com```.  Replace this with the hostname of your Tableau Server environment.  For example, if your Tableau Server is found at ```https://analytics.company.com``` then you would use ```analytics.company.com``` as the new value.  There should be two occurrences that need to be changed:

* composeExtensions.messageHandlers.value.domains

* validDomains

Once the changes have been saved, re-create the zip file and use that as your app package file.  The zip file should not contain any folders within it.

# Step 2: Install the Tableau App

The process of installing apps for Microsoft Teams will likely vary depending on your org’s app policies.  Some customers lock down what apps users are allowed to install in Teams, and if that’s the case the installation will require a Microsoft E365 admin.  

## Admin-managed Installation

If you are managing apps for your users, we need to start in the Teams admin center.  Login to [https://admin.teams.microsoft.com](https://admin.teams.microsoft.com) and navigate to **Teams Apps \-\> Manage Apps** page.  This is where you can manage what apps are available for users to install.  Click on the Upload new App button and upload the manifest zip file from Step 2\.  

![Install app](/public/images/image20.png)
Once installed, you should get a link to manage the app.  
![Install app complete](/public/images/image5.png)

It may take up to 24 hours, but now the Tableau app will be available for users to install in Teams.  As an optional step, an E365 admin may want to auto-install the app for all (or a subset) users.  To do this, use the Teams admin center to navigate to **Teams apps** \-\> **Setup policies**. Create a new policy and click the **Add apps** button under **Installed apps**.  Search for the Tableau app and click **Add.**  You may also want to “pin” the Tableau app in your users’  left navigation.  Once you’ve made your selections, click the save button.  Again, it may take up to 24 hours for these changes to reach your end users.  

![setup app policy](/public/images/image18.png)

## User-managed Installation

If you want your users to decide if they want the app, they can install the app directly from Teams.   Goto the **Apps** page then click the **Manage your apps** button. This shows a list of all the apps installed for your user.  Click the **Upload an app** button, and pass in the manifest zip file.

![Install app](/public/images/image12.png)
You should see a purple **Add** button.  Clicking here will install the Tableau app.

![Install app](/public/images/image19.png)



# Step 3: Configure your site(s)

## First time setup

When you install the app for the first time, Teams will prompt you to open the app.  This brings you to the Tableau app’s Personal tab.  Assuming no sites have already been configured for the Teams tenant, you will see a different landing page that prompts you to enter some authentication details.  If you open the app from Microsoft Word or Powerpoint (instead of Teams) you will see a similar Initial Setup page.  The Teams app and Office addin use the same site configurations, so if you add a site in Teams it will also work in Office (and vice versa).


If you are using Tableau Server and want to use the [default site](https://help.tableau.com/current/server/en-us/sites_intro.htm#the-default-site), leave the site name input field blank.
![Ininital setup](/public/images/image8.png)
Use the below documentation to create a direct trust connected app in Tableau, and enter those details into this form.  

[Tableau Cloud: Create Direct Trust Connected App](https://help.tableau.com/current/online/en-us/connected\_apps\_direct.htm\#create-a-connected-app)

[Tableau Server: Create a Connected App](https://help.tableau.com/current/server/en-us/connected\_apps.htm\#create-a-connected-app)


When you click the **Add Site Config** button, the Tableau app will verify your connected app details actually work before saving them.  It uses the connected app details to create a JWT and tries to authenticate to the Tableau site using an attribute of your Microsoft Entra user profile.  
![Entra Profile image](/public/images/entra-user-profile.png)

There are a few options for the User Mapping Attribute.

#### Attributes from the Microsoft Teams SDK:
* [Preferred_Username](https://learn.microsoft.com/en-us/microsoftteams/platform/tabs/how-to/authentication/tab-sso-code#:~:text=user%27s%20display%20name.-,preferred_username,-%3A%20The%20app%20user%27s): The Teams user's email address, from the Teams SDK. In some cases, this value can differ from the email defined in Microsoft Entra.
* [User Principal Name](https://learn.microsoft.com/en-us/entra/identity/hybrid/connect/plan-connect-userprincipalname#what-is-userprincipalname): This is the primary way users login to Microsoft Entra

#### Attributes from the user's Microsoft Entra profile:
* [Primary Email](https://learn.microsoft.com/en-us/graph/api/resources/user?view=graph-rest-1.0#properties:~:text=%24select.-,mail,-String): This corresponds to the ```user.mail``` attribute, and represents the user's Email address 
* [Mail Nickname](https://learn.microsoft.com/en-us/graph/api/resources/user?view=graph-rest-1.0#properties:~:text=%24select.-,mailNickname,-String): This corresponds to the ```user.mailNickname``` attribute, and represents an alias for th user.
* [Employee ID](https://learn.microsoft.com/en-us/graph/api/resources/user?view=graph-rest-1.0#properties:~:text=a%20user.-,employeeId,-String): This corresponds to the ```user.employeeId``` attribute, and represents an employee identifier assigned by the organization.
* <a href="https://learn.microsoft.com/en-us/graph/api/resources/user?view=graph-rest-1.0#properties:~:text=on%20null%20values).-,onPremisesDistinguishedName,-String">On-premise Distinguished Name</a>: This corresponds to the ```user.onPremiseDistinguishedName``` attribute, and represents the distinguished name (DN) synced from an on-premise Active Directory. 
* <a href="https://learn.microsoft.com/en-us/graph/api/resources/user?view=graph-rest-1.0#properties:~:text=on%20null%20values).-,onPremisesUserPrincipalName,-String">On-premise user principal name</a>: This corresponds to the ```user.onPremiseUserPrincipalName``` attribute, and respresents the userPrincipalName synced from an on-premise Active Directory.
* <a href="https://learn.microsoft.com/en-us/graph/api/resources/user?view=graph-rest-1.0#properties:~:text=%2C%20le).-,onPremisesSamAccountName,-String">On-premise SAM Account Name</a>: This corresponds to the ```user.onPremiseSamAccountName``` attribute, and represents the samAccountName synced from an on-premise Active Directory.
* [Extension Attribute X](https://learn.microsoft.com/en-us/graph/extensibility-overview?tabs=http): Microsoft Entra allows adding up to 15 [extra attributes](https://learn.microsoft.com/en-us/graph/extensibility-overview?tabs=http#extension-attributes) to a user's Entra profile.  These options are there for when your Tableau username does not exist anywhere else in Microsoft Entra.  You can use an extension attribute to store the Tableau username for each Entra user, and then select this option in the Teams app.

If the Tableau authentication API call fails OR returns your Tableau user role as something other than Server/Site Admin, it won’t let you continue.  This is to ensure only a valid Tableau admin is creating the connection.  Don’t forget to enable the connected app in Tableau, otherwise the connectivity test will fail.

If you want to specify some domains to allow-list within the connected app, use the domains listed below:
```
tableau-ms-teams-prod-gycea7csh5hsbfh5.a02.azurefd.net
teams.microsoft.com
*.teams.microsoft.com
```
The first domain is where our app service is hosted, and the next 2 cover Microsoft Teams.  You may need to specify additional domains, if you are using the app in other platforms (outlook, m365, etc).  Microsoft provides a list of domains for their platforms [here](https://learn.microsoft.com/en-us/microsoft-365/enterprise/urls-and-ip-address-ranges?view=o365-worldwide#microsoft-teams).  

Click on the below image, to watch our getting started [video](https://www.youtube.com/watch?v=nsa123RgCO0) on YouTube:

[![Getting Started with the Tableau app for Microsoft Teams](https://img.youtube.com/vi/nsa123RgCO0/0.jpg)](https://www.youtube.com/watch?v=nsa123RgCO0)


## Managing your sites

Once your app has been configured for 1 or more sites, you can always get back to the site manager by using the **Configuration** tab within the Personal App.  This page verifies that you are a Tableau admin, and then shows a list of any connected apps already setup.  You can add or delete connected apps using this page.  We’ve limited the app to just a single connected app per Teams tenant \+ Tableau site.

![manage sites](/public/images/image2.png)



