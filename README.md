# Setup Guide: Tableau App for Microsoft Teams

## Overview

The Tableau Cloud app for Microsoft Teams is a new app in the MSFT Teams Marketplace (GA this summer), that makes it easy to consume your content within Teams.  As the name implies, the version in the Teams Marketplace is only for Tableau Cloud.  This repository contains the app manifest files required to side-load the Tableau Server app for Microsoft Teams.  Side-loading is required, because the app manifest needs to be updated to point to the customer's Tableau Server environment domains.  

## Prerequisites

In order to use the Tableau App for Microsoft Teams, you need a few things:

* **Tableau Cloud** or **Tableau Server** \- You will need to be an admin on your Tableau Cloud site or Tableau Server.  We use [connected apps](https://help.tableau.com/current/online/en-us/connected\_apps\_direct.htm) to provide a single-sign on experience, so we’ll need a Tableau admin to set that up.  If using Tableau Server, you must be running version 2023.1 or newer  
* **Microsoft E365 Subscription** \- The Tableau app is built to work within Microsoft Teams, so you will need an [E365 subscription](https://www.microsoft.com/en-us/microsoft-365/products-apps-services).  Depending on your organization’s [policies](https://learn.microsoft.com/en-us/microsoftteams/app-policies), you may also need to be an E365 admin in order to install the app.  
* **Microsoft Teams** \- In order to use the Tableau app, you will need access to Microsoft Teams.  You can use either the [Teams client app](https://www.microsoft.com/en-us/microsoft-teams/download-app) or login to Teams using your [web browser](https://teams.microsoft.com/).  
* **User mappings** \- The end users of the Tableau app will already be licensed for Microsoft Teams, but they will also need licenses for Tableau.  Further, the user’s email or UserPrincipalName in Azure AD (used by Teams) must match the username in Tableau (for single-sign on)

# Step 1: Get the Tableau App manifest

If you are using Tableau Cloud, you can install the Tableau Cloud app for Teams in the Microsoft Teams Marketplace.

If you are using Tableau Server, download the manifest (zip file) titled ‘tableau-server-app-for-teams.zip’ from [this repository](https://github.com/tableau/tableau-app-msft-teams/raw/main/appManifest/tableau-app-for-teams-server.zip).  Before we can use it though, we’ll have to make some edits.  Unzip the file, and open up manifest.json.  Search the manifest.json file for all occurrences of ```*.online.tableau.com```.  Replace this with the hostname of your Tableau Server environment.  For example, if your Tableau Server is found at ```https://analytics.company.com``` then you would use ```analytics.company.com``` as the new value.  There should be two occurrences that need to be changed:

* composeExtensions.messageHandlers.value.domains

* validDomains

Once the changes have been saved, re-create the zip file and use that as your app manifest file.

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

When you install the app for the first time, Teams will prompt you to open the app.  This brings you to the Tableau app’s Personal tab.  Assuming no sites have already been configured for the Teams tenant, you will see a different landing page that prompts you to enter some authentication details.  
![Ininital setup](/public/images/image8.png)
Use the below documentation to create a direct trust connected app in Tableau, and enter those details into this form.  

[Tableau Cloud: Create Direct Trust Connected App](https://help.tableau.com/current/online/en-us/connected\_apps\_direct.htm\#create-a-connected-app)

[Tableau Server: Create a Connected App](https://help.tableau.com/current/server/en-us/connected\_apps.htm\#create-a-connected-app)

When you click the **Add Site Config** button, the Tableau app will verify your connected app details actually work before saving them.  It uses the connected app details to create a JWT and tries to authenticate to the Tableau site (using Azure AD email or userPrincipalName).  If the Tableau authentication API call fails OR returns your Tableau user role as something other than Server/Site Admin, it won’t let you continue.  This is to ensure only a valid Tableau admin is creating the connection.  Don’t forget to enable the connected app in Tableau, otherwise the connectivity test will fail.

If you want to specify some domains to allow-list within the connected app, use the domains listed below:
```
tableau-ms-teams-prod-gycea7csh5hsbfh5.a02.azurefd.net
teams.microsoft.com
*.teams.microsoft.com
```
The first domain is where our app service is hosted, and the next 2 cover Microsoft Teams.  You may need to specify additional domains, if you are using the app in other platforms (outlook, m365, etc).  Microsoft provides a list of domains for their platforms [here](https://learn.microsoft.com/en-us/microsoft-365/enterprise/urls-and-ip-address-ranges?view=o365-worldwide#microsoft-teams)

## Managing your sites

Once your app has been configured for 1 or more sites, you can always get back to the site manager by using the **Configuration** tab within the Personal App.  This page verifies that you are a Tableau admin, and then shows a list of any connected apps already setup.  You can add or delete connected apps using this page.  We’ve limited the app to just a single connected app per Teams tenant \+ Tableau site.

![manage sites](/public/images/image2.png)

# How to use the App

Now that the Tableau app for Teams is installed/configured, we’ll cover how to use the features available.

## Personal App

![Personal app](/public/images/image4.png)

[Personal apps](https://support.microsoft.com/en-us/office/use-apps-with-a-personal-view-in-microsoft-teams-e3fcae6e-6da2-4c0c-bbea-941ef70716d9) live in the left navigation bar, and are tailored to the logged in user.  When you open the personal app you will default to the Tableau tab, which will show you the dashboards you’ve marked as [favorites](https://help.tableau.com/current/pro/desktop/en-us/favorites.htm) in Tableau.  Keep in mind that many object types can be marked as favorites in Tableau, and this app is only showing views (not workbooks).  Each card on the page represents a Tableau view, and includes the title and a preview image.  If you are looking for a view that is not marked as a favorite, you can always use the purple **Search** button at the top right.  Clicking on the image will take you to the fully embedded version of the view.   

![embedded view](/public/images/image15.png)
Here, you can interact with the view like you would directly in Tableau.  There is a **Share in Teams** button at the top right, which lets you share this view with a person, group chat, or channel.  If you want to go back to the main page, there is a back button at the top right.

The **Pulse tab** (available only for the Tableau Cloud version of the app), will get any Pulse metrics you’ve subscribed to and embed them into the page as cards.  Similarly, you can use the purple **Search** button to search for additional pulse metrics and click on a given metric to view the fully interactive embedded version of Pulse.  There is also a **Share in Teams** button for quickly sharing your metric with another person, group chat, or channel.  

![pulse tab](/public/images/image23.png)

When viewing the embedded Pulse metric, there is also a subscribe button at the top right.  This lets you subscribe/unsubscribe to metrics you may have found via search.

![pulse embedded](/public/images/image13.png)

## Channel Tab

[Channel Tabs](https://learn.microsoft.com/en-us/microsoftteams/platform/tabs/what-are-tabs?tabs=desktop%2Cdesktop1%2Cpersonal) are web apps that you can “pin” to a channel, personal chat, group chat, or meeting.  For example, channels have a **Plus** button at the top.    

![add channel tab](/public/images/image11.png)
Clicking here will open the app selector, where you can select the Tableau app for Teams.  After adding the app, you will get prompted to select the content you want to display.  You can select up to 1 view and 5 Pulse metrics per tab.  More than this would start to get cluttered, so instead you should add multiple tabs instead.  

![configure channel tab](/public/images/image14.png)

Once you’ve saved your selection, you will see your content rendered as a new tab in the channel.  Each tab will default to the name “Tableau”, but you can adjust this by using the **Rename** button in the tab’s dropdown menu.  Pulse metrics you selected will be rendered as cards, but you can click on them to view the fully embedded version of Pulse.  

![channel tab](/public/images/image21.png)

Despite the name, Channel Tabs can be used in places other than channels.  For example, you can add channel tabs to Meetings.  There is actually a very specific path you need to follow in order to add 3rd party apps to a meeting in Teams.  

First, create the meeting using either the **Meeting** or **Calendar** apps.  You can’t add 3rd party apps during the creation of the meeting, instead find the newly created meeting in your **Calendar** app and click **Edit**.  Now you should see a **Plus** button at the top, where you can select the Tableau app.  You will get the same configuration popup as any other channel tab, and once you click save you will see your content as another tab within the meeting.  

![create meeting](/public/images/image17.png)
Alternatively, you could use the Meeting app’s **Meet Now** button to start a new meeting.  Once the meeting has started, you should see the **Plus** button to add the app.  

![meet now](/public/images/image10.png)

*Note that using the **Meet Now** button from a channel does not allow for any 3rd party apps*

Once you’ve started a meeting and have the Tableau app configured, you should see the Tableau icon at the top.  Clicking here will open the channel tab (with content you selected) in the side-panel.  This is visible only to you so far.

![side panel](/public/images/image3.png)

Clicking the purple **Share to Stage** button will push the channel tab’s content to Stage View.  This means anyone on the call will be able to view the content as you interact with it.  Keep in mind this is similar to a screen share, in that tableau content is being loaded using *your* user identity.  So even if there is row-level security on the dashboard you are sharing, everyone is seeing it exactly how you see it. 

![stage view](/public/images/image6.png)

## Message Extension

[Message Extensions](https://camerondwyer.com/2019/01/13/what-are-microsoft-teams-messaging-extensions-and-why-you-should-consider-build-one/) allow you to incorporate 3rd party content when writing messages.  This can be surfaced in many places within Teams, but generally it’s anywhere you see a message compose box or chat window  

![message compose box](/public/images/image7.png)

The Tableau App for Teams includes 2 ways to include Tableau content.  First, you can copy/paste the URL[^1] for any tableau view or pulse metric into the compose box and it will get unfurled automatically into an [Adaptive Card](https://adaptivecards.io/).  You may have seen this already, when using the **Share in Teams** button.  It’s the same idea, the URL for your Tableau content just gets unfurled automatically into an adaptive card.

Adaptive cards are how all 3rd party content gets rendered within Teams chats/channels, and it generally includes some text and an optional image.  In our case, we embed an image of the dashboard/pulse metric along with some metadata.  There’s also a button at the bottom of each card, which will open up the view/metric within the personal app.  

![view card](/public/images/image22.png)
![pulse card](/public/images/image1.png)

If you don’t have the URL for your view/metric, you can also search for the content you want to embed.  Click on the plus button (actions and apps), and select the Tableau app.    

![message extension](/public/images/image9.png)

This will bring up a modal window where you can search for either a View or Pulse Metric.  

![message extension search query](/public/images/image16.png)

Selecting a view/metric from this list will generate an adaptive card for your selection.  Note that clicking on an item triggers several API calls to Tableau behind the scenes in order to get the content & image for that card, so it may take a second or two to generate.  If you click more than once, you will end up with multiple cards being created.

# FAQ

**How does this app authenticate to Tableau?**  
The app gets the logged in Teams’ user’s email address, and uses a [Connected App](https://help.tableau.com/current/online/en-us/connected\_apps\_direct.htm) to authenticate as that user.  This means your ```email address``` or ```userPrincipalName``` in Azure AD must match the ```username``` in your Tableau site.

**Does every user need to set up their Tableau site?**  
No, when the Tableau app first loads it looks for any connected app details associated with the Teams tenant.  If some sites have already been configured, the Tableau app will use that information to authenticate the logged in user.  In other words, once an admin sets up the site config, all other teams users should be able to leverage it (assuming they are licensed in Tableau as well)

**I installed the Tableau app and specified a setup policy to auto-install for users, but I don’t see it in my Teams client yet.  What’s going on?**  
The push from the Teams admin center to Teams clients is known to take a while.  In fact, it can take [up to 24 hours](https://www.paitgroup.com/blog/how-to-make-a-custom-teams-app-available-to-everyone-in-your-organization\#:\~:text=At%20the%20time%20of%20this%20writing%20(June%202020)%2C%20it%20takes%20around%2024%20hours%20for%20the%20updated%20policy%20to%20be%20pushed%20to%20all%20users%20in%20the%20organization%20%E2%80%93%20both%20in%20a%20test%20environment%20and%20in%20an%20actual%20environment%20%E2%80%93%20so%20don%E2%80%99t%20expect%20to%20see%20the%20changes%20immediately) before your changes show up in the Teams client.

**When I try to uninstall the app, I get an error saying “There was an issue while trying to remove Tableau Cloud. Please try again.”.  How can I remove the app?**  
This can happen sometimes, when using multiple clients (web app \+ desktop app).  Generally, clearing the Teams cache seems to work.  Follow the instructions [here](https://learn.microsoft.com/en-us/microsoftteams/troubleshoot/teams-administration/clear-teams-cache).

**What about push notifications from Tableau to Teams?**  
This functionality is on our roadmap, but not currently available.

**How does security work when sharing metrics/vizzes?**

There are several ways you can share a viz or pulse metric with other users.

1. Copy/paste a link to the viz/metric  
2. Use the “search” message extension while writing a message  
3. Clicking the Share in Teams button from the personal app

Before the content is shared, the Tableau app automatically converts the viz/metric URL into an adaptive card with a preview image of the viz/metric.  This is created using the context of the current user (not who you are sharing with).  This means there are scenarios where you could share a metric/viz with someone who does not have permissions to view that content in Tableau.  

In this scenario the other user will see the preview image (similar to taking a screenshot and sharing the image) but if they click the “Open” button from the card, the Personal App will display an error saying that viz/metric could not be found.  There’s no way to say 100% that this is a permission issue (it could have been moved or deleted), so the wording of the error message has to be a bit vague.


[^1]:  The URL can be copied a few different ways.  You can copy/paste the URL from your browser window or use the link from the Share button.  This also works on custom views.  Pulse metrics don’t have a share button, so just copy/paste the URL from the browser window.

