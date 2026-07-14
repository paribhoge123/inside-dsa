The Most Interesting One: Integer Overflow


Here's the unsettling part: C++ does not check for overflow. 
No error, no warning, no crash. It just silently gives you the wrong answer and keeps running like nothing happened.
This is what makes it dangerous — a program can look completely correct, pass small tests, and only fail on large real-world inputs.


What's actually happening under the hood:
An int is stored in a fixed number of bits (usually 32).
Those bits can only represent a fixed range of values — roughly -2.1 billion to +2.1 billion. 
When you go one step past the maximum, there's no "extra space" to hold the new value — the bits just wrap around to the minimum, like a car odometer rolling over from 999999 back to 000000.


cpp code: 
int x = 2147483647;   // the actual maximum int value
cout << x + 1;         // prints -2147483648


It doesn't gradually break — it snaps instantly from the largest positive number to the largest negative number.
That's the "wraparound."

A real-world example that made this famous:
In 2014, YouTube's view counter for "Gangnam Style" hit exactly this wall.
YouTube stored view counts as a 32-bit signed integer, and the video was closing in on ~2.1 billion views — the max value an int can hold. 
If it had overflowed, the counter would have gone negative. 
YouTube engineers had to fix this by switching view counts to 64-bit integers before it broke.

Another famous case:
the Ariane 5 rocket failure (1996) — not exactly the same overflow type, but the same root category of bug (a numeric value exceeding what its data type could represent) caused the guidance system to crash, and the rocket self-destructed 37 seconds after launch.
A ~$370 million loss, from a data type limitation.


Why this is exactly your problem in DSA:
Overflow bugs are sneaky because they only appear once your numbers get large enough.
Your code will work perfectly on small test cases, pass initial checks, feel "done" — and then silently fail on the actual large hidden test cases in interviews or competitive programming judges, without any obvious error message.
You'll just get a "Wrong Answer," and often have no idea why, because the bug is invisible unless you know to suspect it.

The instinct to build: the moment you're computing a sum, product, or any accumulation in a loop — pause and ask "could this realistically exceed ~2 billion?" 
If yes (or if you're unsure), default to long long instead of int.
It costs you almost nothing, and it silently prevents one of the most common wrong-answer bugs in competitive programming.
