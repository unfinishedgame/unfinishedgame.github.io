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
  * ROW: Takes two parameters: a y position, and a color, which is passed directly to ctx.fillStyle. Fills the row at that y position.

        # Fills the top row light blue
        ROW 0 lightblue
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
  * DLA: Takes one parameter: a target frame rate. Tries to match the frame rate of the program to that frame rate whenever possible. The default value is 30.
  
        # Set target frame rate of program to 60 fps
        DLA 60
  * JMP: Takes three parameters: the first two are values and if they are equal, the third parameter, a subroutine, is called.

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
  * PLY: Takes two parameters: the name of an audio and a 0/1 representing weither it should loop or not, with 1 meaning that it should and 0 meaning it shouldn't.

        # Plays the audio mySong on loop
        PLY mySong 1
  * DBG: Takes no parameters and prints out the contents of long term memory and short term memory to the browser's console.
  * NOP: Does nothing.
  * SYS: Takes two parameters: a system property, and a value. The available properties are:
      - 0: The name of the program. For the value, underscores are replaced with spaces.

            # Set the title of the program to "Hello World"
            SYS 0 Hello_World

Long term memory goes from addresses 0 to 255. Short term memory is addressed from 0 to 15. "Values" as described here are signed 8-bit integers and do not describe memory locations.


The syntax is as follows:
  * Comments are declared with a hashtag. They can only be declared as the first character of a line, and anything else on that line is skipped over.
    
        # This is a comment!!!
        SAV 0 16    # This, unfortunately, is not, because it comes in the middle of a line.
    
  * There are three special functions: start, update, and audio. These are declared with an exclamation mark and ended with an "!endspecial":
    
        !start
            # do something as soon as the program is loaded
        !endspecial

        !update
            # do something every frame the program is active
        !endspecial

        !audio myAudio
            # create some audio
        !endspecial
  * Regular subroutines are declared with a colon and ended with an "END":
    
        :doSomething
            # do something!
        END
       Note that indentation is not required in subroutines and is stripped when the line is processed.

   * Recursive subroutines can optionally use a special syntax, which tells the interpreter to represent it internally as a loop.
      
         # loops until location 0 is no longer less than 10
         :<countToTen
            ADD 0 1
            LOD 0 0
            # the > operator at the beginning declares this as the condition for the loop
            >CMP 10 $0
         END
      This syntax is completely optional and only needs to be used if you're worried about stack overflows or performance issues.

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
  * Audio can be played using the audio sub-syntax, which is used inside !audio tags. Each line has five pieces of information: 
      1. The type of wave to use. The options are square, triangle, sine, and sawtooth.
      2. The track to play on. There are four tracks total, and while each track can have multiple instruments/notes playing on it simultaneously, it can only have one volume. 
      3. The frequency of the wave. Tools exist online to see which frequency corresponds to what note.
      4. When the note begins. Zero is when the audio is first played, and after that, it's measured as a delay in seconds. 
      5. The duration of the note. This is also measured in seconds.

             !audio aSongIMade
                  # With a sine wave, play A for a second, and then B
                  sine 0 440 0 1
                  sine 0 493.88 1 1
             !endspecial
      To set the volume for all notes after it, use /volume. The volume inputted is multiplied by the default volume to get the final volume, so a value of 0.5 would make the volume half of the default and 2 would double it.

        !audio playingWithVolume
            # set the volume to half of the normal volume
            /volume 0.5
            triangle 0 440 0 0.5

            # set it to 10 times the normal volume (not recommended for safety reasons)
            /volume 10
            triangle 0 440 0.5 0.5

            # if i play a note on another track, that track's volume is now also 10
            triangle 1 440 1 0.5
        !endspecial
      The audio can be addressed with the name that comes after "!audio". For the example above, that would be "playingWithVolume". Audio is played with the PLY command, so this would be played with "PLY playingWithVolume 0".

These are some important bits of information that are useful to know:
1. The size of the screen is 128x72 pixels, with the bottom right most corner being at 127, 71.
2. Memory registers hold signed 8-bit integers, meaning they go from -128 to 128. The fact that they are integers means that division will not be accurate and should not be used for anything precise. Long term memory has 256 registers and short term memory has 16.
3. Most commands that provide an output write the output to long term memory. If you want to use the output, it needs to be transferred to short term memory with the LOD command.
    
To run your file in the virtual console, navigate to the main menu, go to the virtual console page, and drag the code file out of your file manager and into the window. This will run the program. If your code has bugs, it might not always show as an error and may instead silently crash, or it may display a cryptic error message that forces you to look through the implementation of the virtual console to find out what may be causing the issue. This is fairly useless advice, but try to not have bugs in your code.
