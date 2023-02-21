## Destiny2DPSGraphPY

**Description:** [Destiny2DPSGraphPY](https://github.com/katzerax/Destiny2DPSGraphPY) is a command-line program that helps users input and simulate the DPS (damage per second) of various weapons in Destiny 2. The program can read weapon data from a JSON file or accept input through the command line. If inputting through the command line, the user is prompted to provide the weapon name and other various data. Once the weapon data is collected, the program calculates the DPS and plots it on a graph using Matplotlib. The resulting graph represents the DPS over time for the given weapon, taking into account reload times and other factors. The user can then compare the graphs of different weapons to determine which has the highest DPS.

### 1. Why DPS over time?

Measuring DPS over time can give a more accurate representation of sustained damage output over an extended period, which is important in many scenarios such as boss fights with phases, where the goal is to deal a certain amount of damage within a fixed time frame. A DPS average, on the other hand, can be skewed by burst damage or moments of low activity, and may not reflect the damage output in a given encounter accurately.

This is intended to be automation of an already existing style of graph used by the Destiny 2 community. The key difference is that usually, these graphs are done by manually keeping track of data points, and then plotting by hand. Automation is the key here.

### 2. Explanation of the code

I will explain parts, but not all of, the code. Make sure you visit the repository itself to check out the code on your own. It is highly likely that my explanations will quickly go out of date, as this is an actively worked on code repository. It might fundamentally change after the date that I am writing this (currently Feb 20, 2023).
https://github.com/katzerax/Destiny2DPSGraphPY

Let's start with the arguments. 

The below code will show how we utilize command line arguments to set up the program in different ways.
```python
agp = argparse.ArgumentParser()
agp.add_argument("-im", "--input-mode", default="file", choices=["file", "cli"], type=str, help="mode for inputting data. options: 'file' or 'cli'. default: 'file'")
agp.add_argument("-rf", "--read-file", default="weapons.json", type=str, help="file of weapon information to read. default: weapons.json")
agp.add_argument("-d", "--dialogue", default="y", choices=["y","n"], type=str, help="beginner friendly dialogue to choose between input modes. options: 'y' or 'n'. default: 'y'")

args = agp.parse_args()
```
As you can see, currently we have 3 main potential arguments, those being input-mode, read-file, and dialogue. With input mode you can choose to specifically skip everything else and just run from a pre-set json file. This can be useful especially in the circumstance of sharing json files with other people. You can choose to use a different json file than "weapons.json" using the read-file argument. The program runs with user friendly dialogue, so disabling that with the respective argument is possible, and useful for more advanced users, who may be running the program over and over.

Next up I will display how the program uses those arguments to initialize, and run through the basics, of the program itself.
```python
if args.dialogue == "y":
    print("*Note: this dialogue can be toggled off using '-d n' or '--dialogue n' from the command line")
    choice = ""
    while choice not in ("cli", "file"):
        choice = input("Choose an input mode:\n\tcli - input weapon information via command line\n\tfile - input weapon information via the 'weapons.json' file\n")
    args.input_mode = choice

if args.input_mode == "cli":
    # Initialize list to store weapon dictionaries
    weapons = []
    perk = 0
    perks = []
    # Loop to input weapon data
    while True:
        # Input weapon data
        perks = []
        perk = 0
        weapon = {}
        weapon["name"] = input("Enter weapon name: ")
        weapon["fire_rate"] = float(input("Enter Rounds Per Minute: "))
        weapon["reload_time"] = float(input("Enter reload time: "))
        weapon["damage_per_shot"] = float(input("Enter damage per shot: "))
        weapon["magazine_capacity"] = int(input("Enter magazine capacity: "))
        weapon["ammo_reserve"] = int(input("Enter ammo reserve: "))
        weapon["delay_first_shot"] = bool(int(input("Enter whether to delay the first shot (1 - true, 0 - false): ")))
        weapon["add_perks"] = bool(int(input("Enter whether to apply perks (1 - true, 0 - false): ")))
        
        if weapon["add_perks"] == True:
            weapon["enhanced_perks"] = bool(int(input("Assume all perks are enhanced? (1 - true, 0 - false): ")))
            weapon["weapon_class"] = int(input("Weapon Ammo Type (1 - Primary, 2 - Special, 3 - Heavy): "))
            print("Enter a perk from the following list to add:\n1) Triple Tap\n2) Fourth Time's\n3) Veist Stinger\n4) Clown Cartidge\n5) Overflow\n6) Rapid Hit\n7) Vorpal Weapon\n8) Focused Fury\n9) High Impact Reserves\n10) Firing Line\n11) Explosive Light\n12) Cascade Point\n13) Explosive Payload\nEnter -1 to Stop\n")
            teehee = 0
            while(perk!=-1):
                teehee += 1
                [print("Perk", teehee)]
                perk = int(input("input: "))
                if((perk!=-1) and ((perk>=1) and (perk<=13))): #change upper bound with new perks
                    perks.append(perk)
            weapon["perks"] = perks

        # Add weapon to list
        weapons.append(weapon)
        
        # Check if user wants to add more weapons
        add_more = input("Add more weapons? (y/n) ")
        if add_more.lower() != "y":
            break
    
    # Write weapons data to JSON file
    with open(args.read_file, "w") as f:
        json.dump({"weapons": weapons}, f)
```
You can see here that by default, it will use the user friendly dialogue, and start to create a dictionary for the weapon information that it then intends to write to the json file.

### 3. Additional Credits
- Roxy/vcat
- Snark
<img src="images/d2dpsgraphs.png?raw=true"/>

### 4. Potential To-Do's:

WIP
