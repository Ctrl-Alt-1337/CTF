# Installing FlareVM

First off I installed [FlareVM](https://github.com/fireeye/flare-vm). Since I had no prior experience with CTF assignments I watched the playlist from [HacerSploit](https://www.youtube.com/watch?v=ZKObRxxbOCQ&list=PLBf0hzazHTGMSlOI2HZGc08ePwut6A2Io).

After setting up the sandbox I started with the first and easiest assignment.

# FLARE-On Challange 5 (step-by-step approach)

Challanges found on http://flare-on.com/, pw: infected

## MinesweeperChampionshipRegistration

 - Opening the file ```MinesweeperChampionshipRegistration.jar``` opens a program looking like this: ![alt text](https://github.com/Ctrl-Alt-1337/CTF/blob/master/MinesweeperChampionship1.png)
 - From here, I started to look into how a .jar file could be decompiled. I uploaded the file to ```http://www.javadecompilers.com/``` and got the following code back: ![alt text](https://github.com/Ctrl-Alt-1337/CTF/blob/master/MinesweeperChampionship2.png)
 - Easily is it to see if we type ```GoldenTicket2018@flare-on.com``` we will pass the check. 
 - Result: ![alt text](https://github.com/Ctrl-Alt-1337/CTF/blob/master/MinesweeperChampionship3.png)


# FLARE-On Challange 6 (step-by-step approach)

## Memecat Battlestation

- First We are presented with a simple game written in .NET. Since .NET written code can be easily dissambled to readable C# code.
- My approach:
  - First I took a snapshot, then opened the file and saw how the game looked like.
  - Since it's a .NET file, there is no need to statically or dynamically analyze the binary.
  - Used the tool ```ilSpy``` in C:\ProgramData\Microsoft\Windows\Start Menu\Programs\FLARE\dotNET to open the file.
  - Got familiar with the code and went to the function ```FireButton_Click``` to find the first message to insert into the memecat program: RAINBOW.
  - I understood ```isValidWeaponCode``` was somehow XOR'ing some input with the 'A' character, and I could see some kind of comparison was made with the char array: '\u0003', ' ', '&', '$', '-', '\u001e', '\u0002', ' ', '/', '/', '.', '/'. Here I had to get [help](https://www.fireeye.com/content/dam/fireeye-www/blog/pdfs/FlareOn6_Challenge1_Solution_MemecatBattlestation.pdf):
"The values may be confusing to read if you are new to reversing .NET in dnSpy, but the characters like the
first that begin with ‘\u’ are specified in hexadecimal and do not have an ASCII representation. One
approach would be to convert all of the values to hexadecimal and enter them into a hex editor, or enter
the printable characters into the ASCII view and the other values into the hex view of your favorite hex
editor. The hex values for this sequence are: '03 20 26 24 2D 1E 02 20 2F 2F 2E 2F'. Most hex
editors will provide a single-byte XOR capability. Alternatively, you could perform this operation with
CyberChef (https://gchq.github.io/CyberChef/).
If you XOR decode each byte of this sequence with ‘A’ (hex ‘41’) then you get this array of characters
‘Bagel_Cannon’."
   - After typing ‘Bagel_Cannon’, a victory screen was seen.
   

  
  
  





