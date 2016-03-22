{\rtf1\ansi\ansicpg932\deff0\nouicompat\deflang1033\deflangfe1041{\fonttbl{\f0\fnil\fcharset0 Calibri;}}
{\*\generator Riched20 6.3.9600}\viewkind4\uc1 
\pard\sl240\slmult1\f0\fs22\lang9 # Event Query Language\fs36\par
\fs22 Event Query Language is a search language for ordered datasets.  Event Query Language allows you to search such datasets for relationships between events.  \par
\par
Event Query Language implements this by allowing relative search terms between events.  Eg:\par
\par
* Find all events with a Description containing 'dog' and events with a Description containing 'cat' that have a Starttime after the previous event\par
* Find all events with a Description containing 'cat' and find all events with a Description containing 'dog' that have the same Count as the previous event\par
\par
Such linked events are returned as entire sequences that fulfill the search criteria.  \par
\par
Eventlist:\par
''' [\{EventID: 1, Description: cat, starttime: 12am, count: 5\},\par
\{EventID: 2, Description: cat, starttime: 3pm, count: 1\},\par
\{EventID: 3, Description: dog, starttime: 9am, count: 5\},\par
\{EventID: 4, Description: dog, starttime: 6pm, count: 1\}]\par
'''\par
\par
The EventIDs of the events that the 1st search returns:\par
* [1, 3]\par
* [2, 3]\par
\par
The EventIDs of the events that the 2nd search returns:\par
* [1, 3]\par
* [2, 4]\par
\par
## Install\par
Download eventsearch.py and place it in your working directory, simply import eventsearch to access the methods\par
output.py is required for the testing script only\par
\par
## Examples\par
To use Event Query Language, you must format your events as a dictionary:\par
\par
''' \{Descriptor: Value, Descriptor: Value\}\par
'''\par
Dates must be stored as python's datetime.datetime format.  String values are not case sensitive. \par
\par
Events are then packaged into a list of events to form an eventlist.  \par
\par
The two main functions of Event Query Language are the methods Evaluate and Translate.  \par
\par
Evaluate takes an eventlist and three synchronized lists called queries, connectors and gets.  \par
Translate implements a simple syntax to turn a human readable string into the lists of queries, connectors and gets that Evaluate requires.  \par
Translate and Evaluate operate independently.  \par
\par
Translate separates words by spacebar and uses ALLCAPS to find keywords, underscores are turned into spacebars after the words have been split.  Here are a few examples of a string that Translate parses:\par
\par
This searchstring finds all possible combinations of events with description "intracranial hypertension" and events with a description "endadmission".  ALLCAPS keywords that are not reserved by Translate are used to find a descriptor in your events.  \par
''' DESCRIPTION intracranial_hypertension \par
AND DESCRIPTION endadmission \par
ENDSEARCH\par
'''\par
\par
Relationships between events use the ASPREVIOUS keyword, this refers to the event directly previous to this event by default (the same as ASPREVIOUS-1). \par
''' DESCRIPTION intracranial \par
NOT AND DESCRIPTION intracranial \par
    COUNT ASPREVIOUS GREATERTHAN COUNT \par
ENDSEARCH\par
'''\par
\par
Special followedby and precededby keywords allow easy linking using a starttime descriptor; without having to explicitly write out the relationships. \par
''' DESCRIPTION intracranial \par
FOLLOWEDBY DESCRIPTION obesity  \par
ENDSEARCH\par
'''\par
Compared with:\par
''' DESCRIPTION intracranial \par
AND DESCRIPTION obesity  \par
    STARTTIME ASPREVIOUS AFTER STARTTIME \par
    NOT UUID ASPREVIOUS UUID \par
ENDSEARCH\par
'''\par
\par
Searches can be nested inside brackets, criteria applied to the bracket will apply to all events returned from inside the bracket. \par
''' DESCRIPTION intracranial\par
FOLLOWEDBY (\par
    DESCRIPTION obesity \par
    FOLLOWEDBY TEST ferritin\par
) \par
OR FOLLOWEDBY DESCRIPTION endadmission \par
ENDSEARCH\par
'''\par
}
 