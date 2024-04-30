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
In the new_event_info.dart file, the error occurs on the line:
```
var json = jsonDecode(result.body);
```

## Hypothesis About the Bug
The new_event_info.dart file deals with sending event information to the backend, where the backend will store it in the GrinSync database. So, I'm hoping that this is a backend issue that Brian can magically fix because he always does that.  
### Possibilities
- I just implemented sending the student org name to the backend if users choose to create an event under a student org. Perhaps Brian hasn't pushed the changes that would allow for the frontend to send the student org name yet, and that's why we see the error.
- Brian changed the API URL, and I didn't know, so I'm sending the information to an invalid URL.
- I somehow am sending invalid info for the student org, but the debugger on VS Code said that my student org variable is "GrinSync Team," which is right... So I don't think this is it.
- The entire backend server is broken! I feel like this is unlikely since I can still log into the app. Maybe the server is broken for just creating events?
- I'm sending a null value for one of the variables, and the backend doesn't like that. 

## Journal Entries
**4/29/2024, 3:52 pm**: I have just encountered the error. Without a second thought, I took a picture of it and sent it to Brian & Livia because, at this point, I am confident that this is a backend issue and that Brian or Livia can magically fix it.
The full error (on the line that I wrote above) I sent to the backend team:
```
Exception has occurred.
FormatException (FormatException: Unexpected character (at line 2, character 1)
<!doctype html>
^
)
```

**4/29/2024, 4:08 pm**: Uh oh. Livia said it's not the backend that's giving the error. She said it's an "html rendered error", so it's not something that she (the backend) is returning. She suggested that I print the error so that we could get a better sense of what was going on. 

**4/29/2024, 4:10 pm**: I added a print statement as I wrote it below.
```
var json = jsonDecode(result.body);
print(result.body);
```

**4/29/2024, 4:11 pm**: What?! It didn't even print the error. It just gave me the same message at the same line again?? Where would it even print anyway? The Output panel, right? I checked the Test Results panel. There's nothing there. I checked the Comments panel. There's nothing there. I checked the Output panel again. There's nothing there. Am I printing the error right? Did I just find another bug? 

**4/29/2024, 4:28 pm**: WOW. I can't believe I've made this mistake. I also can't believe I've wasted more than 15 minutes of my life looking at the print statement, wondering why it wasn't working. Of course the print statement won't execute if it's after the line of code that is raising the error. The error will be raised, and the program won't run anymore, and the print statement won't execute. DUH! My fix: 
```
print(result.body);
var json = jsonDecode(result.body);
```

**4/29/2024, 4:30 pm**: Yes! The error printed in the Output panel, as it should. Why am I so happy I solved this when I still have another bug to solve? Anyway, here is the output of the original bug/error:
```
<!doctype html>
<html lang="en">
<head>
&nbsp;<title>Server Error (500)</title>
</head>
<body>
&nbsp;<h1>Server Error (500)</h1><p></p>
</body>
</html>
```
So this HAS to be a backend issue. I'm sending this new error output to Brian and Livia because it says server, and server = backend.

**4/29/2024, 6:48 pm**: Brian replied! He said he's going to check it out. He asked what student org info I am sending right now. I told him that I was sending the student org name. He then asked what API variable I was sending it under, and I told him it was 'org_id'. Now I shall await for his response.

**4/29/2024, 6:52 pm**: Wait. 'org_id' is supposed to be an int because it's an ID number given to each student org. But I'm sending a String because I want to send the name of the student org during event creation. I send this note to Brian. As I am writing this, Brian has gotten back to me. He said that what I just realized was definitely the issue. I was sending a String for an int variable. He said I should send the name to 'orgName' instead, which accepts Strings. I didn't know there was an 'orgName' variable?!? Oh wait, I do remember Brian mentioning that briefly during lab time... I should've taken notes. Oops! 

**4/29/2024, 7:02 pm**: I quickly made the fix to send the student org name to 'orgName' instead of 'org_id'. I tried to create an event under a student org, and it worked! The event can be seen in the GrinSync calendar, and the hostname of the event is the student org now! Yes! Everything has been fixed :)

**4/29/2024, 9:24 pm**: Brian has informed me that he configured the backend to return a more helpful error with some feedback in case that Server Error 500 happens again. So hopefully next time, I don't have to waste Brian's time!

## Solution 
Previously, this is how I sent the student org name for a newly created event to the backend:
```
if (org != null) {
  body['org_id'] = org;
}
```
What this code did was check that 'org' is not null (i.e., there's a student org to send to the backend), and then set the API variable 'org_id' to 'org'. However, since 'org_id' is expecting an int, this gave me a huge error. Now, I am sending the student org name to another API variable, which is actually meant for the name as a String. Now: 
```
if (org != null) {
  body['orgName'] = org;
}
```
Also, Brian configured the backend so that in case something like this happens again, there's a more informative error (more descriptive than Server Error 500, he switched a setting to allow for more detail to be returned).
So the solution was simply to send the student org name (a String) to the correct API variable on the backend! 

## Strategies to Solve the Bug
The first thing I did was ask my team for help. Perhaps the way I did it was unproductive, however, since I assumed I could just hand off the problem to them and not worry about it. Still, it was good I asked for help! After getting their input, I printed the error message so I could see what was going on. I looked at the other frontend developers' code to see how to print the error since I wasn't sure what that action would entail. Turns out, it was simply: 'print(result.body)'. Another strategy that I kind of used was just talking/texting it out. Once I texted Brian what exactly I was doing with the student orgs and what I was sending, I realized what the issue was. Had I just gone through my code earlier on and talked to my rubber ducky about it, perhaps I would've realized that I'm sending a String to an int variable and this whole fiasco could have been avoided. So, I asked for help, used print statements, and talked it out! 

## Reflection
I am quite embarrassed about this bug, to be honest. I considered not writing this Bug Journal because I didn't want the whole world to see my epic failure. It truly was such a simple fix. Type errors as simple as the one I had are things that beginner coders get stuck on, not someone in their fourth year of their computer science major. Nonetheless, this experience has both humbled me and also reminded me that sometimes, these bugs/errors aren't quite as big as you think they are. This experience has also taught me how important descriptive error messages would be. Seeing the Server Error 500 was pretty scary because I thought there was a huge issue with our code, but turns out there was just a type error that was fixed very easily. It would've been helpful to see a more descriptive error message so that we could come to a solution much quicker, but this experience has brought us back to our roots. It has reminded us of the basics and how important they are, even when you're doing advanced stuff like making an app. 
