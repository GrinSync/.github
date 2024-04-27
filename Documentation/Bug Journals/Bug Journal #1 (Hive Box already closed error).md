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
