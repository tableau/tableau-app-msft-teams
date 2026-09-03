[Tableau Supported](https://www.tableau.com/support-levels-it-and-developer-tools)

# Tableau App for Microsoft M365

Header Image

## Overview

The Tableau Cloud app for Microsoft M365 is an app in the MSFT Teams Marketplace that makes it easy to consume your content within Teams and Office.  Currently, the app is enabled as a [Teams app](https://learn.microsoft.com/en-us/microsoftteams/apps-in-teams#partner-apps-created-by-independent-app-developers).  It can be used from the below Microsoft Products:

- Microsoft Teams

This app (specifically, this branch) is a variation on the Tableau Cloud app in the Microsoft Marketplace.  Some customers have a requirement where they cannot install any application that uses Resource Specific Consent (RSC) permissions.  There are several features that required those RSC permissions, so this version of the app removes the following features to meet the "no RSC permissions" requirement:

- Office Add-in: All office addins need the ability to read/write to files in SharePoint.  To do this requires an RSC permission, specifically the `Document.ReadWrite.User` permission.  Since we can't include that permission, none of the MSFT Office addin functionality can be included.
- Teams App -> Meetings: The standard app allows users to pin Tableau content within meetings, including the ability the share that content via Teams' meeting stage.  This requires RSC permissions as well, so this functionality was removed.



# Setup Guide

For instructions on how to setup the Tableau app, please see the [Setup Guide](SETUP.md) document.

# How to use the App

For instructions on how to use the Tableau app, see the below How To guides:

- [Microsoft Teams](HOW_TO_USE_TEAMS.md)



## FAQ

For frequently asked questions, please see the [FAQ](FAQ.md) document.

## Release Notes

Check out the [release notes](/ReleaseNotes.md), for the details of each update.