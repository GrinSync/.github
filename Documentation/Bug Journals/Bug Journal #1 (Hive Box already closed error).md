## Reproductions Steps
The error occurs on start-up, so simply running the program on the emulator in its current state causes the error.
## Hypothesis About The Bug
The bug occurs at start-up so I am thinking it is occuring during app initialization, maybe in the main function that runs when the app is started. It could also be something on the main page, but usually this
returns an error on the page and not during initialization so I am leaning away from this idea.
## Journal Entries
**4/27/2024, 6:32 pm**: I think the error occured sometime in the last few commits, since the app was working for our demo on Wednesday. The error appears to be taking place in our getAllTags function so I'll see where we call that function. I'll look through the commits and see if I can find anything. 

**4/27/2024, 6:40 pm**: After looking through the commit history it appears that the line "await getAllTags();" was added to the main function. I'll look into why that is throwing an error.

**4/27/2024, 6:52 pm**: The error appears to be taking place when we close the Hive box, but this code looks identical to the code we have written before to open and close Hive boxes, so I am confused as to why it is breaking now.
**4/27/2024, 6:55 pm**: If I just comment out the line of code that closes the box the program appears to work fine, so closing the box in the main function is definitely what is causing the error.

**4/28/2024, 10:05 am**: I am not seeing anything that jumps out as bad from this code chunk, but maybe I can do some google searching and exploring elsewhere.

**4/28/2024, 11:12 am**: I think I found the error. Interestingly, I think this might be an example of race conditions from our CSC-213 class. The setLoginStatus() function call is an asynchronous function, thus allowing the program to continue executing code while it executes. However, the getAllTags() function executes immediately after, and both functions are attempting to access the same Hive Box. I think that both functions are getting there at the same time, opening the box, and then one closes the box, so when the second function attempts to close the box, we get a "Hive box already closed" error.

**4/28/2024, 11:15 am**: Okay it appears that was the error since things run when we remove one of the two Hive box close calls. This probably isn't a great long-term solution though, so we should discuss as a group how we want to move forward.

## Solution
I currently have deleted one of the box.close() calls arbitrarily. However, this could be a larger issue since we opne and close this box a lot in the program. Are there any other places that have race conditions that we are unaware of? We should discuss how to fix this moving forward. 

## Strategies To Solve The Bug
I first attempted to narrow down where the bug took place using the error message returned by the program as well as by looking at past commits that have affected that area of the code. I then tried debugging the code using breakpoints and other debugging methods, but this did not provide much help. I then commented out the line of code with the bug to check if the program ran when the bug was commented out, and it did run as expected. I thought about reasons why the box would be closed after we just opened, and was able to come to the conclusion something else must have been closing the box. After inspecting other functions that could have closed the box, I came to the conclusion the asynchronous setLoginStatus() function could be closing the box at the same time, which ended up being our error. 

## Reflection
