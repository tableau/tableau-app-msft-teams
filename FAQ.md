# FAQ

## How does this app work?
![Architecture Diagram](/public/images/architecture.png)
Microsoft provides an SDK for developing apps that run within the M365 suite of products.  The model is for Microsoft partners to develop and host these M365 apps, which are surfaced within the Microsoft stack.  The Tableau app was developed using this SDK, and runs within Teams, PowerPoint, and Word.  When you open the Tableau app in Teams or Office, Microsoft reaches out to our app in order to dislay the UI.  Our app communicated with Tableau Cloud or Server, which lets end users consume Tableau content directly in Teams and Office.

Since Tableau Cloud is a publicly accessible service, there are usually no extra steps required to ensure the Teams app is able to communicate with Tableau Cloud.  Tableau Server environments are often locked down behind a firewall, though.

### What if my Tableau Server is behind a firewall?
In order to fetch content from your Tableau Server, our app service needs to be able to reach it via HTTP requests.  This means your Tableau Server will need to be accessible to the public internet.  Typically enterprise security will not be OK with allowing incoming traffic from anywhere, so instead create an ingress rule to allow incoming HTTP requests from the below IP ranges:
```
4.152.0.0/15
20.2.0.0/16
```

### What if my Tableau site is using Tableau Cloud IP Filtering allow list?
In order to fetch content from your Tableau Cloud site, our Teams app service needs to be able to reach your site via customer-allowed source IPs or IP ranges. This means your team will need to request that Azure (source) IP ranges be added to the site's IP Filtering (IPF) allow list.  While IPF is in preview access, you will need to work with your AE/CSM to do this. In the future this is planned to be self-service. This is required in order for the site data to be accessible to the customer Teams Server in Azure cloud. To continue to use Tableau Cloud IP Filtering on your site, please request that the incoming traffic from Azure below Azure IP ranges be added to your allow list:
```
4.152.0.0/15
20.2.0.0/16
```

## How does this app authenticate to Tableau?
The app gets the logged in Teams' user's email address, and uses a [Connected App](https://help.tableau.com/current/online/en-us/connected\_apps\_direct.htm) to authenticate as that user.  This means your ```email address``` or ```userPrincipalName``` in Azure AD must match the ```username``` in your Tableau site.

## How does security work when sharing metrics/vizzes?

There are several ways you can share a viz or pulse metric with other users.

1. Copy/paste a link to the viz/metric  
2. Use the "search" message extension while writing a message  
3. Clicking the Share in Teams button from the personal app

Before the content is shared, the Tableau app automatically converts the viz/metric URL into an adaptive card with a preview image of the viz/metric.  This is created using the context of the current user (not who you are sharing with).  This means there are scenarios where you could share a metric/viz with someone who does not have permissions to view that content in Tableau.  

In this scenario the other user will see the preview image (similar to taking a screenshot and sharing the image) but if they click the "Open" button from the card, the Personal App will display an error saying that viz/metric could not be found.  There's no way to say 100% that this is a permission issue (it could have been moved or deleted), so the wording of the error message has to be a bit vague.

## Does every user need to set up their Tableau site?
No, when the Tableau app first loads it looks for any connected app details associated with the Teams tenant.  If some sites have already been configured, the Tableau app will use that information to authenticate the logged in user.  In other words, once an admin sets up the site config, all other teams users should be able to leverage it (assuming they are licensed in Tableau as well)

## Is the Tableau app supported in GovCloud?

No, currently the Tableau app for Teams is supported only on Commercial versions of Teams.  Please add your vote to [this Idea](https://ideas.salesforce.com/s/idea/a0BHp000016KsRlMAK/support-tableau-app-for-microsoft-teams-for-azure-govcloud-environments) posted on the Salesforce IdeaExchange to voice your support for adding Azure GovCloud as a supported environment for the Tableau App for Microsoft Teams.

## I installed the Tableau app and specified a setup policy to auto-install for users, but I don't see it in my Teams client yet.  What's going on?
The push from the Teams admin center to Teams clients is known to take a while.  In fact, it can take [up to 24 hours](https://www.paitgroup.com/blog/how-to-make-a-custom-teams-app-available-to-everyone-in-your-organization\#:\~:text=At%20the%20time%20of%20this%20writing%20(June%202020)%2C%20it%20takes%20around%2024%20hours%20for%20the%20updated%20policy%20to%20be%20pushed%20to%20all%20users%20in%20the%20organization%20%E2%80%93%20both%20in%20a%20test%20environment%20and%20in%20an%20actual%20environment%20%E2%80%93%20so%20don%E2%80%99t%20expect%20to%20see%20the%20changes%20immediately) before your changes show up in the Teams client.

## What if I'm unable to consent to the OAuth permissions?

Some organizations lock down app permissions more than others.  Some customers may need to have their Teams admin grant admin consent, in order for end users to use the app.  This can be done in the [Microsoft Teams Admin Center](https://admin.teams.microsoft.com/policies/manage-apps).  Navigate to the *Manage App* page and find the *Tableau Cloud* app.  Switch to the *Permissions* tab and dlick the ```Grant admin consent``` button.
![Admin consent page](/public/images/app-permissions.png)

## When I try to uninstall the app, I get an error saying "There was an issue while trying to remove Tableau Cloud. Please try again.".  How can I remove the app?
This can happen sometimes, when using multiple clients (web app \+ desktop app).  Generally, clearing the Teams cache seems to work.  Follow the instructions [here](https://learn.microsoft.com/en-us/microsoftteams/troubleshoot/teams-administration/clear-teams-cache).

## What about push notifications from Tableau to Teams?
This functionality is on our roadmap, but not currently available.

## I've deleted the connected app details from Tableau Cloud/Server, and now I can't login to the app

The Tableau app for Teams stores your connected app details in a database, and uses them when it first loads to authenticate you to Tableau Server/Cloud.  If the connected app details are disabled or deleted from Tableau Cloud/Server, the Teams app will no longer be able to authenticate you and just give you a login error message.  This can lead to getting locked out of the app, if you configure it for one site and then delete those connected app details later on.  

From the app's perspective, we want to ensure only Site/Server Admins can add/remove connected app details so if we can't verify your identity then we shouldn't provide a way to delete the connected app details from our backend.  If you need us to delete the connected app details for your site, please submit a [support ticket](https://www.tableau.com/support/case) and provide the full URL to your Tableau environment and provide written consent from your Tableau Admin for us to remove your connected app details.  

[^1]:  The URL can be copied a few different ways.  You can copy/paste the URL from your browser window or use the link from the Share button.  This also works on custom views.  Pulse metrics don't have a share button, so just copy/paste the URL from the browser window.

