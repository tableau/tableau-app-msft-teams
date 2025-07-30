# Release Notes
This document outlines all updates applied to the app.  Please note that not all updates will show up within the MSFT Teams marketplace listing. 

#### Major Release (ex. version 1.X.0)
A major release means updating the listing on the Microsoft Teams Marketplace.  Typically this will mean net new features, which often come with new permissions required to use those changes.  Any change to the app manifest (what's in the marketplace listing) will be classified as a major update.  Most major updates will occur [automatically](https://support.microsoft.com/en-us/office/update-an-app-in-microsoft-teams-3d53d136-5c5d-4dfa-9602-01e6fdd8015b), but if the app requests new permissions then you will need to click the Update button from within Teams.

#### Minor Release (ex. version 1.0.X)
A minor release will include bug fixes and minor changes.  Since we're not adding new features or requesting new permissions, there is no need to updating the marketplace listing.  There is no need for customers to do anything for minor releases, the changes are automatically available.

## Minor Release: 1.0.8 - Jul 30, 2025

* **Custom View Support** - Previously, custom views were supported for link unfurling only.  Now, users can search for custom views by URL within the personal app and the channel app.  This means you can now pin custom views to channel tabs.
  
* **Pulse Insight Summaries** - The Pulse tab in the personal app will now show a summary of your pulse metrics, similar to the Pulse Homepage in Tableau Cloud. These insight summaries are generated as a background job typically once per day, so the language may not match your language setting in Teams.  Whatever language you've set in Tableau Cloud will be used to generate the insight's text.
  
* **Pulse Localized Insights** - The Pulse tab in the personal app shows a list of metrics you are subscribed to, along with a preview image and some insights about the metric.  This was hard-coded to english, but now we take the language from your Teams settings and use that to generate the insight text
  
* **Pulse Alerts** - The Pulse tab in the personal app will now show an alert icon next to any metric that has triggered an alert.  Note that this does not mean notifications are being pushed to Teams, instead they are fetched when the personal app's Pulse tab is loaded.

## Minor Release: 1.0.7 - Dec 19, 2024

* **New User Mapping Attributes** - Previously, there were only 2 options for mappings users within Teams -> Tableau: UserPrincipalName & Email.  The Email option derived the user's email by using the [preferred_username](https://learn.microsoft.com/en-us/microsoftteams/platform/tabs/how-to/authentication/tab-sso-code#:~:text=user%27s%20display%20name.-,preferred_username,-%3A%20The%20app%20user%27s) provided by the Teams SDK.  While this usually mapped to a user's email address, there are some cases where this value did not match the ```user.mail``` attribute from Microsoft Entra user profiles.  To resolve this, we started fetching several new user profile attributes directly from the Microsoft Graph API.  No existing site configurations have been changed, but now instead of ```Email``` you will see it labeled as ```Preferred_Username```.  There is a new attribute ```Primary Email``` that uses the ```user.mail``` attribute from the user's Entra profile.  There are also several new attributes, outlined in the [ReadMe](/README.md)

* **Tableau Server default site** - For Tableau Server customers using the default site, there were several bugs discovered.  For example, clicking on a view's preview card didn't trigger the fully interactive embedded experience.  This has been resolved.

## Minor Release: 1.0.5 - Nov 8, 2024

* **Tableau Server Default Site** - The site name input textbox (on configuration tab and initial setup page) required entering at least 1 character.  This prevented Tableau Server customers from adding their default site (site name must be blank).  This restriction has been removed, so users setting up their default site can leave this blank.

* **Channel Tab Setup Page** - If you had Pulse disabled for you Tableau Cloud site, the Channel Tab's setup page would display an error message and you would not be able to select a view to embed.  Now you will still see the message saying we can't get your Pulse subscriptions, but you will be able to search for Views to embed in the channel tab.

## Major Release: 1.0.0 - Sep 16, 2024
This is the initial release of the app.
