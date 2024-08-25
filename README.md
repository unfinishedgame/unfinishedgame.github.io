# My Website
My website is a website made by me. It's a place where I can put code that I write. Unlike the majority of coders, I actually like using JavaScript, so I mess around with it a lot. I wanted a place to put all the stuff I write in it, so here we go!

Don't expect any consistency, effort, or just general usability.


## Mobile Friendliness
It has come to my attention that the website is not mobile-friendly. This is a feature, not a failure.

## Virtual Console
The website now contains a virtual console. This feature is a work in progress that accepts a file containing instructions and runs them. It is meant to resemble the functionality of an old video game console. Note that the goal was not to be accurate to the programming languages and hardware of the time. These are the commands that the console accepts as of now:

  * SAV: Takes two parameters: a memory location in long term memory to save to, and a value to save. Saves the value at that location.
    
        # Saves the value 65 at the location 5
        SAV 5 65
  * LOD: Takes two parameters: a memory location in long term memory to load from, and a memory location in short term memory to save it to.
    
        # Loads the value saved at location 5 into the short term memory to be used at location 0
        LOD 5 0
  * VID: Takes three parameters: an x position, a y position, and a color, which is passed directly to ctx.fillStyle. Draws a pixel at that location. The bounds of the screen are from (0, 0) to (127, 71).
    
        # Places a green pixel around the middle of the screen.
        VID 64 36 green
  * ADD: Takes two parameters: a memory location in long term memory, and a value to add to the value stored there.
    
        # Adds five to the number stored at location 4
        ADD 4 5
  * SUB: Same thing as ADD, but it subtracts the value.
    
        # Subtracts five from the number stored at location 4
        SUB 4 5
  * MUL: Same thing as ADD, but it multiplies the value.
    
        # Multiplies the number stored at location 4 by five
        MUL 4 5
  * DIV: Same thing as ADD, but it divides the value. Note that this is integer division and should not be used in scenarios where precision is needed.
    
        # Divides the number stored at location 4 by five and rounds up the result
        DIV 4 5
    DLA: Takes one parameter: a target frame rate. Tries to match the frame rate of the program to that frame rate whenever possible. The default value is 30.
        # Set target frame rate of program to 60 fps
        DLA 60
    JMP: Takes three parameters: the first two are values and if they are equal, the third parameter, a subroutine, is called.
        # Go to subroutine "gameover" if location 5 equals 0
        JMP $5 0 :gameover
  * CMP: Takes three parameters: the first two are values and if the first is greater than the second, the third parameter, a subroutine, is called.
    
        # Go to subroutine "buypotion" if location 10 is greater than 20
        CMP $10 20 :buypotion
  * BTN: Takes two parameters: A memory location in long term memory, and a number. The number represents the button desired:
      - 0, 1, 2, and 3 are up, left, down, and right respectively. 4 and 5 are A and B. 6 and 7 are SELECT and START.
        On keyboard, the directions are mapped to the arrow keys, A and B are the keyboard keys 'a' and 'b', SELECT is the space bar, and START is the enter key.
    
            # Puts a 1 in memory location 10 if the START button is pressed, otherwise a 0
            BTN 10 7
  * DBG: Takes no parameters and prints out the contents of long term memory and short term memory to the browser's console.
  * NOP: Does nothing.

Long term memory goes from addresses 0 to 255. Short term memory is addressed from 0 to 15. "Values" as described here are signed 8-bit integers and do not describe memory locations.


The syntax is as follows:
  * Comments are declared with a hashtag. They can only be declared as the first character of a line, and anything else on that line is skipped over.
    
        # This is a comment!!!
        SAV 0 16    # This, unfortunately, is not, because it comes in the middle of a line.
    
  * There are two special subroutines: start and update. These are declared with an exclamation mark and ended with an "!endspecial":
    
        !start
            # do something as soon as the program is loaded
        !endspecial

        !update
            # do something every frame the program is active
        !endspecial
  * Regular subroutines are declared with a colon and ended with an "END":
    
        :doSomething
            # do something!
        END
  Note that indentation is not required in subroutines and is stripped when the line is processed.

  * Commands are run by entering the command in all caps, and then the parameters seperated by spaces.
    
        VID 10 10 blue
  * Commands that take a subroutine as a parameter use the following syntax:
    
        # This command will call mySubroutine.
        JMP 0 0 :mySubroutine
  * Any value in a command (see the definition of "value" right below the list of commands) can also be replaced with a short term memory access. The short term memory access syntax is a dollar sign followed by a short term memory address. Short term memory is addressed from 0 to 15.
    
        # Add 1 to long term memory location 0
        ADD 0 1
        # Add the value of the short term memory register 5 to long term memory location 0
        ADD 0 $5

    These are some important bits of information that are useful to know:
        1. The size of the screen is 128x72 pixels, with the bottom right most corner being at 127, 71.
        2. Memory registers hold signed 8-bit integers, meaning they go from -128 to 128. The fact that they are integers means that division will not be accurate and should not be used for anything precise. Long term memory has 256 registers and short term memory has 16.
        3. Most commands that provide an output write the output to long term memory. If you want to use the output, it needs to be transferred to short term memory with the LOD command.
    
To run your file in the virtual console, navigate to the main menu, go to the virtual console page, and drag the code file out of your file manager and into the window. This will run the program. If your code has bugs, it might not always show as an error and may instead silently crash, or it may display a cryptic error message that forces you to look through the implementation of the virtual console to find out what may be causing the issue. This is fairly useless advice, but try to not have bugs in your code.
