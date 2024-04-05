# What #
GrinSync is an app/website for scheduling, finding, and tracking events happening at Grinnell College. It contains: a home page that showcases events happening soon, a calendar page with multiple different time views, a search page, an event creation/editing page, and a user profile. There are three kinds of users, and users classify themselves upon account creation: community members, Grinnell College staff/faculty/admin, and Grinnell College students; if users do not wish to create an account, they may still use GrinSync in Guest mode.  

# Ideas #
Our idea behind GrinSync is to create a platform that makes it easy for Grinnell students/faculty and community members to get accurate information about Grinnell events. We also want student organizations/admin to be able to communicate with Grinnellians about their events, community members to be able to more easily engage with the college, and students to be able to plan what events they will attend based on their needs, interests, and schedules.

# Goals #
Our goal is to develop both an app (Android and iOS) and a website for GrinSync, though we imagine that the app will be more popular among our target audience. Some of our priorities for GrinSync are user-friendliness, accessibility, and smooth functionality.

## Major features ##
- A comprehensive event calendar with all (or close to all) events happening at Grinnell College.
- The ability for users (those that are allowed to) to add and edit events in the event calendar.
- The ability for users to delete events that they have created from the event calendar.
- The ability for event adders to provide essential event details for an event, such as title, date, time, location, and a brief description of the event.
- A basic filtering scheme to allow users who are trying to discover events to categorize events in the event calendar by type (food is offered, pets are allowed, etc.).
- The ability for users to set preferences to tailor their event recommendations in the app. 
- Automated pulling of events from Grinnell College’s official calendar and Grinnell College Athletics’ calendar to GrinSync’s event calendar. 
- Some sort of GrinSync moderator to ensure the app is accurate (details to be fleshed out still). 
- Account creation for GrinSync users.
- Student organization leaders can link their account with their student organization once they verify their identity. Verification must occur every semester. 

## Stretch goals ##
- Allowing users to select specific events/organizations to get notifications from.
- The option to edit an event after you post it and notify interested users about the changes in event details.
- Adding events to the Student User’s Outlook calendar.
- The option to provide an accessibility report of the event with information about noise levels, accessible entrances, and the availability of interpreters, for example.

# Layout of the GrinSync organization #
The GrinSync organization has three repositories: *.github*, *GrinSyncFrontend*, and *GrinSyncBackend*. 
- *.github*: This is a general repository for general information on GrinSync (as the whole project). It is the place where you can find this README.md file. You can also find our Sprint Planning Reports (in the "Sprint Planning Reports" folder) and Sprint Review Reports (in the "Sprint Review Reports" folder) in this repository.
- *GrinSyncFrontend*: This repository will contain all of the code and software development for the frontend of GrinSync.
- *GrinSyncBackend*: This repository will contain all of the code and software development for the backend of GrinSync.

# Issue-tracking tool #
We use [Trello](https://trello.com/b/uRb8HI8c/grinsync) to track our issues. Currently, we have the main pages of our mobile app that need to be implemented as our high-level "epic" issues in our backlog. In subsequent sprints, we will break them down into sub-issues and allocate them to different members of the group. We have a "Backlog" category that contains future, more high-level issues that need to be completed. We have a "Sprint # Issues" category for the issues we plan on doing for the current sprint (#). We have an "In Progress - Sprint #" category for issues we are actively working on for the current sprint. Then, we have categories of the name "Done in Sprint # (MM/DD - MM/DD)" for the issues we completed for each sprint previously. We will move issues into the different categories as we plan, start, and finish them during a sprint (as appropriate). 

# Operational Use Cases (so far) #
- Ben creates an event by entering his event information in GrinSync (FEATURES NOT YET DONE FOR THIS USE CASE: repeating events option, associating student organization, picture upload (we may switch this to a stretch feature); as of now, we have the MVP for the event creation page - event title, location, date/time, description, event tags).
- Noah looks through the daily, weekly, and monthly calendar views on the Calendar page. 
