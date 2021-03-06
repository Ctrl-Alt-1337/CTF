# Intro

When I have the time I will upload more solutions and step-by-step approaches. Mainly wrt. reverse engineering.

# FLARE-On Challenge 5 (step-by-step approach)

Challenges found on http://flare-on.com/, pw: infected

## MinesweeperChampionshipRegistration

 - Opening the file ```MinesweeperChampionshipRegistration.jar``` we look at a program: ![alt text](https://github.com/Ctrl-Alt-1337/CTF/blob/master/MinesweeperChampionship1.png)
 - From here, I started to look into how a .jar file could be decompiled. I uploaded the file to ```http://www.javadecompilers.com/``` and got the following code back: ![alt text](https://github.com/Ctrl-Alt-1337/CTF/blob/master/MinesweeperChampionship2.png)
 - Easy is it to see if we type ```GoldenTicket2018@flare-on.com``` we will pass the check. 
 ![alt text](https://github.com/Ctrl-Alt-1337/CTF/blob/master/MinesweeperChampionship3.png)

## UltimateMinesweeper

- Statically analyzing the file through pestudio, written using .NET, 32-bit
- Opening the file through ilspy, looking at two functions; ```InitializeComponent()``` and ```MineFieldControl_MouseCLick()```. Tells me flags, bombs etc are inserted into an array possibly.
- Trying to debug with olly, it's likely some anti-debugger is present, atleast it crashed. Cheat-Engine works, however.
- I see 'Mines remaining' counter changes value when I flag the minefield (right-click), this way I can find the address for 'Mines Remaining'. By the principle of locality wrt. data structures, the rest of the code may be relatively near. By using Cheat Engines "Dissect Structure" we can see the relevant datastructures are relatively near the 'Mines Remaining' address:
![alt text](https://github.com/Ctrl-Alt-1337/CTF/blob/master/UltimateMinesweeper1.png)
- We are now scrolling down and finding some interesting offsets; "mineField" and some other interesting offsets. 
![alt text](https://github.com/Ctrl-Alt-1337/CTF/blob/master/UltimateMinesweeper2.png)
- Looking more into "mineField" it's possible to find an array 900 large, as big as the amount of fields. We identified the relevant structures. 
![alt text](https://github.com/Ctrl-Alt-1337/CTF/blob/master/UltimateMinesweeper3.png)
- It should now be possible to iterate through the array and find 3 elements not set to 1.
- We find the pointer chain so we have the correct addresses everytime we start a new game.
![alt text](https://github.com/Ctrl-Alt-1337/CTF/blob/master/UltimateMinesweeper4.png)
- By changing the values in minesVisible from 0 to 1 we can now reveal where all non-mine squares are.
![alt text](https://github.com/Ctrl-Alt-1337/CTF/blob/master/UltimateMinesweeper5.png)

# FLARE-On Challenge 6 (step-by-step approach)

Challenges found on http://flare-on.com/, pw: flare

## Memecat Battlestation

- First off, we are presented with a 32-bit game written in .NET. 
- My approach:
  - First I took a snapshot of the VM, then opened the file and saw how the game looked like.
  - Used the tool ```ilSpy``` to open the file.
  - Got familiar with the code and went to the function ```FireButton_Click``` to find the first message to insert into the memecat program: RAINBOW.
  - I understood ```isValidWeaponCode``` was somehow XOR'ing some input with the 'A' character, and I could see some kind of comparison was made with the char array: '\u0003', ' ', '&', '$', '-', '\u001e', '\u0002', ' ', '/', '/', '.', '/'. Here I had to get [help](https://www.fireeye.com/content/dam/fireeye-www/blog/pdfs/FlareOn6_Challenge1_Solution_MemecatBattlestation.pdf) to understand I had to XOR decode each byte of the sequence with ‘A’ (hex ‘41’) to get the array of characters: ‘Bagel_Cannon’.

   

  
  
  





