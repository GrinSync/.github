## Reproductions Steps
The error occurs on start-up, so simply running the program on the emulator in its current state causes the error.
1. Open your local repo of the project in VS Code.
2. Have your emulator running (for here, just follow the steps to start the emulator you usually use).
3. In the bottom right corner, click “No Device.”
4. Choose your running emulator.
5. Click the “Run and Debug” tab on the left in VS Code.
6. Click the “Run and Debug” button in that page.
7. As the app tries to build, the 'Hive Box already closed error' exception will show and prevent the app from running.

## Hypothesis About The Bug
The bug occurs at start-up, so I think it occurs during app initialization. Maybe there's something in the main function that runs when the app is started. It could also be something on the main Home page, but usually this
returns an error on the page and not during initialization, so I am leaning away from this idea.
### Possibilities
- The Hive.close() call in main.dart is somehow getting called before the Hive.openBox() call... This shouldn't even happen since Hive.close() is in the dispose() function, not the main function like Hive.openBox() is. The main function is called first.  
- The Hive.openBox() call isn't working and so the Hive Box never opens... We would probably get an error on the Hive.openBox() call if this were the case, not a closing error like we are now.
- We have a Hive.close() call somewhere else in our code (not in main) that occurs on startup, which closes the Hive Box already... This is probably our most plausible hypothesis (see our work for it below!)

## Journal Entries
**4/27/2024, 6:32 pm**: I think the error occurred sometime in the last few commits since the app worked for our demo on Wednesday. After parsing through the exception stack call, the error appears to be occurring in our getAllTags function, so I'll see where we call that function. I'll look through the recent commits and see if I can find anything. 

**4/27/2024, 6:40 pm**: After looking through the commit history, it appears that the line "await getAllTags();" was added to the main function. This must be why when the main function is called in start-up, the error appears. I'll look into why that is throwing an error.

**4/27/2024, 6:52 pm**: The error appears to be taking place when we close the Hive box (as the error name suggests), but this code looks identical to the code we have written before to open and close Hive boxes, so I am confused as to why it is breaking now.

**4/27/2024, 6:55 pm**: If I just comment out the line of code that closes the box in the getAllTags function, the program appears to work fine, so closing the box in the main function is definitely what is causing the error.

**4/28/2024, 10:05 am**: I don't see anything that jumps out as bad from this code chunk (the main function and the function calls in there), but maybe I can do some Google searching and exploring elsewhere.

**4/28/2024, 11:12 am**: I think I found the error. Interestingly, I think this might be an example of race conditions from our CSC-213 class. The setLoginStatus() function call is an asynchronous function, thus allowing the program to continue executing code while it executes. However, the getAllTags() function executes immediately after, and both functions are attempting to access the same Hive Box. I think that both functions are getting there at the same time, opening the box, and then one closes the box, so when the second function attempts to close the box, we get a "Hive Box already closed" error.

**4/28/2024, 11:15 am**: Okay, it appears that was the error since things ran when we removed one of the two Hive box close calls. This probably isn't a great long-term solution, though, so we should discuss as a group how we want to move forward in terms of regulating which functions and when to access the Hive Box. 

## Solution
I currently have deleted one of the box.close() calls arbitrarily from the getAllTags() function. Since we know that the setLoginStatus() function closes the Hive Box, it does get closed eventually. However, this could be a larger issue since we open and close this box frequently in the program. Are there any other places that have race conditions that we are unaware of? It's possible, but we aren't seeing anymore of this bug since we deleted one of the box.close() calls. We should discuss how to regulate this solution moving forward. 

## Strategies To Solve The Bug
I first attempted to narrow down where the bug took place using the error message returned by the program as well as by looking at past commits that have affected that area of the code. I then tried debugging the code using breakpoints and other debugging methods, but this did not provide much help. I then commented out the line of code with the bug to check if the program ran when the bug was commented out, and it did run as expected. I thought about reasons why the box would be closed after we just opened it and was able to come to the conclusion something else must have been closing the box. After inspecting other functions that could have closed the box (which ended up being setLoginStatus()), I came to the conclusion the asynchronous setLoginStatus() function could be closing the box at the same time, which ended up being our error. Throughout this, I talked to my metaphorical duck, which helped me come to the conclusions I had. Also, Brian was able to help me talk through the debugging as well (yay for Brian & ducky!). Also, note that consulted the internet for some help. 

## Reflection
I was hoping I would be done with race condition bugs after 213 :). After solving this bug, we learned that we definitely need to check into some of our other code to make sure we don't have any other asynchronous calls that could cause this error. I also spent a lot of time just staring at the function where the error took place and didn't even consider that another function call could be causing the error to take place initially. In the future, I will take a step back and look at the bigger picture when debugging, as that can offer insight into how the bug came about. This experience has taught me to definitely be more careful with those asynchronous calls. 
