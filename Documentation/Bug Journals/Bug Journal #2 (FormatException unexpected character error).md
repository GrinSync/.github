# FormatException unexpected character error on event creation under student organizations

## Reproduction Steps
This error happened after I integrated creating an event as a student organization (org) on the Create Event page. I simply added a dropdown menu to the top of the Create Event page that gave the users options to create events as their linked student orgs (if they have any). But when I tried to create an event under a fake tester student org, GrinSync, I got a FormatException error. 
1. Have the app up and running.
2. Log into the app.
3. If you don't have an account, follow Steps 4-8 to use our test account to reproduce the bug. Otherwise, ignore these steps.
4. Press the 'Profile' button in the bottom navigation bar.
5. Click 'Login'.
6. In the username/email field, type 'admin'.
7. In the password field, type 'testpassword'.
8. Log in!
9. Press the 'Create' button in the bottom navigation bar.
10. Fill out the event title, location, start date-time, and end date-time with some random but valid data (start date-time must be before end date-time).
11. At the top, click the dropdown for the Student Orgs and select "GrinSync Team".
12. Go to the bottom and click the "Create Event" button.
13. The event will not submit to the GrinSync calendar because there's an error instead! Ah!
14. 
