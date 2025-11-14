# Release Notes
This document outlines all updates applied to the app.  Please note that not all updates will show up within the MSFT Teams marketplace listing. 

#### Major Release (ex. version 1.X.0)
A major release means updating the listing on the Microsoft Teams Marketplace.  Typically this will mean net new features, which often come with new permissions required to use those changes.  Any change to the app manifest (what's in the marketplace listing) will be classified as a major update.  Most major updates will occur [automatically](https://support.microsoft.com/en-us/office/update-an-app-in-microsoft-teams-3d53d136-5c5d-4dfa-9602-01e6fdd8015b), but if the app requests new permissions then you will need to click the Update button from within Teams.

#### Minor Release (ex. version 1.0.X)
A minor release will include bug fixes and minor changes.  Since we're not adding new features or requesting new permissions, there is no need to updating the marketplace listing.  There is no need for customers to do anything for minor releases, the changes are automatically available.

## Major Release: 2.0.0 - TBD

* **Microsoft PowerPoint & Word support** - The Tableau Cloud app now appears in the ribbon within Word and PowerPoint.  This enables users to easily find and embed their Tableau content with documents and presentations.  The Tableau content is embedded as images, so we also provide a way to refresh those images when needed.

## Minor Release: 1.0.11 - Nov 13, 2025
* **Pulse Trendlines** - When using the Tableau Cloud app in meetings, the side panel used to be a fixed width.  Now Microsoft allows users to adjust the width, so we've updated the pulse trendlines to auto-fit whatever width is available.  There were also some cases of the axis formatting rendering incorrectly when adjusting the sidepanel's width while viewing 2 or more metrics, which has been resolved.
  
## Minor Release: 1.0.10 - Oct 24, 2025
* **Pulse Link Unfurls** - Preview cards of metrics were not rendering correctly, after Pulse introduced forecasts as part of the trendlines.  Preview cards (generated via link unfurling or the search message extension) now correctly render the metric's trendline.

* **iOS Support** - While this app has always worked on iOS devices, the ability to view interactive dashboards/metrics was not supported due to a 3rd party cookies issue.  Now the app will detect your device type, and for iOS devices we will render dashboards/metrics as static images instead.  While not interactive, this does allow the users to view a high-resolution image of the dashboard/metric.

* **Site Selector** - The personal app contains a site selector dropdown menu, which allows users to choose which Tableau Cloud site they are viewing data from.  Previously, changes made through the site selector menu were saved automatically as the user's new "default site".  This behavior has been corrected, so going forward changes made will not auto-save the site as a new "default site".  You can change the default site through the configuration tab in the personal app.
  
## Minor Release: 1.0.9 - Aug 14, 2025
* **Pulse Insight Summaries** - If Tableau returned an error when fetching a Pulse Insight Summary, we just displayed the error code.  That wasn't very helpful though, so now we have friendlier messages for the most common errors

* **Pulse Timeframes** - The labels for timeframes of Pulse metrics would always display as Month/Week/Year to Date, which did not account for metrics defined as Last Month/Week/Year/etc.  This issue has been resolved.

* **Pulse Alerts** - Pulse metrics show a bell icon when the metric has an alert associated with it, but this was only within the Personal App.  This functionality has been added to channel tabs as well

* **Pulse Subscribe icon** - The old icon for subscribing to Pulse metrics was a bell.  However that made more sense as the icon for alerts, so there is a new icon for subscribing/unsubscribing to Pulse metrics

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
