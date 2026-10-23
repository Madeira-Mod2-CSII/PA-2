# Programming Assignment II

## The Data

From the National Park Service:

The NPS Social Science Program is responsible for coordinating visitor use statistics reporting for units administered by the NPS. ‘Visitor Use Statistics’ refers to a formal set of statistics collected by the National Park Service that address how many people visit parks and how long they stay. Program staff work with parks to develop appropriate data collection procedures and provide quality control for visitor use data. Such coordination ensures that visitor use data are consistent and reliable throughout all units of the National Park System. This data package contains data collected from 1979-2025.

## The Task

You will write a Python program that processes the NPS Visitor Use Statistics data package and produces reports based on user input.

The program will use the XML file to build two dictionaries and the CSV file to build a list of observations. It will then use these data structures to provide the reports described in the Menu section.

The state and territory abbreviations and names are already stored in a dictionary in main.py. You do not need to create this dictionary.

### Required Data Structures

1. The state / territory abbreviations and names is already in a dictionary that is in ```main.py``` for you.

You are going to process the XML data to create two dictionaries:

1. A dictionary to store the unit codes with their bounding coordiantes. These unit codes describe the bounding area that the attendance was counted for.

1. A dictionary to store the translation of Statistic codes and the English Language explanation.

You will process the relational data in the csv file to output the reports described in The Menu section below. For each row in the csv, you will use ```readline().split(',')``` to create a list for that observation. You will then add this to a ```park_data``` list. Each row has the following format:
```
UnitCode	Year	Month	Statistic	Value	StateID
```
Each observation must retain all six fields. Convert values to numeric types as needed for calculations.


## The Menu

|Option|Task|
|---|---|
|1| Find state(s) associated to a unit code observations |
|2| Find total visits for a statistic code and state code |
|3| Write every entry for a statistic code to a user-named file |
|4| Print the coordinates for a user-entered unit code| 
|5|Quit|

### Option 1

Ask the user to enter a unit code, such as ABLI. Search park_data for observations whose UnitCode matches the entered code. Collect the associated StateID values. Each state should only be listed once. Use the provided state dictionary to display the full state or territory names.

### Option 2

The program should:

    - Ask for a statistic code, such as BC.

    - Ask for a state abbreviation, such as MD.

    - Find the statistic's description in the XML-derived dictionary.

    - Find all CSV rows matching both Statistic and StateID.

    - Add their Value fields and display the result using an f-string.

For example, the output template could be:
```
Backcountry Overnight Stays
Total visits in Maryland: 123456
```

### Option 3

Ask for the statistic code and output filename, then write every matching CSV record to the requested file. If the user's proposed filename already exists, ask the user for a new filename. You can use ```os.path.isfile``` or ```os.path.exists``` depending on how you decide to do it. Documentation for these two functions can be found here:
[Python Docs for os.path](https://docs.python.org/3/library/os.path.html)

The output file should contain the header and every matching data row, preserving the original fields and order. The output file *must not* overwrite an existing file.

### Option 4

Ask for a Unit Code, find its corresponding record, and print the bounding coordinates as a tuple, for example:

(West, East, North, South)

For instance, for ABLI the tuple should be

(-85.7408, -85.6355, 37.6183, 37.5293)

The program looks up a unit code in unit_codes and prints the corresponding four-element tuple. If the user enters an invalid code, they should be given an error message and returned to the main menu.

## Processing the files

### Processing the XML File

Open the XML file and read its contents. Use regular expressions to locate the unit-code bounding coordinates and statistic-code definitions. Build both dictionaries from the same file contents.

#### NPS Content Unit Link / Unit Codes

For each block:
```xml
<geographicCoverage system="content unit link">
        <geographicDescription>NPS Content Unit Link: ABLI</geographicDescription>
        <boundingCoordinates>
          <westBoundingCoordinate>-85.7408</westBoundingCoordinate>
          <eastBoundingCoordinate>-85.6355</eastBoundingCoordinate>
          <northBoundingCoordinate>37.6183</northBoundingCoordinate>
          <southBoundingCoordinate>37.5293</southBoundingCoordinate>
        </boundingCoordinates>
</geographicCoverage>
```

You need to create an entry in a dictionary called ```unit_codes``` which contains the code as a key and contains a tuple in the format (West, East, North, South). For the block above, the dictionary entry would look like:
```python
{"ABLI" : (-85.7408, -85.6355, 37.6183, 37.5293)}
```

#### Statistic codes

You will make a dictionary called ```stats_codes``` to translate the Statistic codes into their definition. For each block:
```xml
<codeDefinition>
<code>BC</code>
<definition>Backcountry Overnight Stays</definition>
</codeDefinition>
```
For the block above the dictionary entry should be
```python
{"BC" : "Backcountry Overnight Stays"}
```

**Technical Note:** Implementation 

You should use the concepts that have been discussed in class. In particular, you will want to break your work up into functions so that you can organize your code and reuse common tasks.

- Do **NOT** hardcode the dictionaries. You should do the kind of processing that was demonstrated in class.

- You should *not* use try / except for error handling or input validation. All of these things can be accomplished with a function implementing an appropriate looping construct. This includes the ```with`` construct commonly used with files.

- You should not use modules that we have not discussed in class. For instance ```beautiful soup``` and XML parsers are off limits. The points is for you to practice using regular expressions.

- You are required to use a list of lists. The ```csv`` module should not be used to subvert this requirement.

- ```map```, ```filter```, ```reduce```, and ```lambda``` functions are not to be used. 

- If you have questions or concerns regarding the concepts you are using or the methods you are implementing *please* ask. 

*Remark:* This data set was retrieved from 
[NPS Data](https://catalog.data.gov/dataset/nps-visitor-use-statistics-data-package-2024)