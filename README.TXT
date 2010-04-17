Release Notes: DAVILA 0.1

About DAVILA

DAVILA is an open source relational database schema visualization and annotation tool.  It is written in Processing using the toxiclibs physics library.

DAVILA is named after John Adams' 1790 treatise on political philosophy, "Discourses on Davila: a series of papers on political history," an extensive commentary of the Italian historian and diplomat Enrico Caterino Davila's 1630 "Istoria delle guerre civili di Francia."

DAVILA was built with funds and support from NINES (http://nines.org)
	* In particular Dana Wheeles and Laura Mendel for their encouragement and great design suggestions

Thanks also go to 
	* Ben Fry and Casey Reas for creating Processing and then sharing it with the rest of us
	* Karsten Schmidt (a.k.a. toxi) for toxiclibs and lighting fast responses to my questions
	* Daniel Shiffman for great toxiclibs tutorials
	* PhiLho, Cedric, TfGuy44, and all others on the Processing fora who helped me out

Copyright 2010, Jean Bauer http://www.jeanbauer.com

Topics covered:
	* System Requirements
	* Interacting with DAVILA
	* Parsing your schema with DAVILA
	* Annotating your schema with DAVILA
	* Common reasons to alter the code
	* Known Issues

********************
System Requirements

Processing 0182 (released 3//29/2010) or later
toxiclibs complete library package 0016 (or later)

To download the Processing language and the Processing Development Environment (a simple IDE), visit:
http://processing.org/download

To download toxiclibs visit:
http://hg.postspectacular.com/toxiclibs/downloads

Download the toxiclibs-complete-0016.zip (or later version) package and add it your Processing Library 
(see http://processing.org/reference/libraries/ for instructions).

To learn more about this excellent library visit:
http://toxiclibs.org

********************
Interacting with DAVILA

Before you annotate your own schema, play around with the example included in the download.

You can either run the program from the Processing Development Environment (PDE), or open the index.html file located in the example_applet folder to run it as an applet in a web browser.

To learn more about PDE see http://processing.org/reference/environment/

You can also run Processing code in Eclipse, see here for more information
http://processing.org/learning/eclipse/

Each entity appears in the window in a translucent, color-coded rectangle surrounding the entity's name with arrows showing its relationship to the other entities in the database.  Entities that are central to their module will display with bolded text. DAVILA sets the starting positions of each entity randomly, and then lets the toxiclibs spring algorithm place them in the window with minimal overlap. Your interaction options are as follows:

Using the mouse:
	* Drag and drop the entities into a better pattern for the database design
	* LEFT CLICK an entity to lock it into place
	* RIGHT CLICK an entity to unlock it (it will place itself according to the spring algorithm)
	* DOUBLE LEFT CLICK an entity to toggle between displaying only the name of the Entity and also displaying the annotation text and list of attributes.  If it is the central entity of its module the color coded band around the expanded display will be 2.5x thicker.
	* DOUBLE RIGHT CLICK on a central entity to toggle between hiding and displaying the other entities in that module -- this is particularly useful when you want to focus on one aspect of a complicated database.  

Pressing the SPACE KEY (will not work if running as an applet)
	* will save a snapshot of the diagram as a vector-scaled PDF file in the sketch folder (SKETCH --> VIEW SKETCH FOLDER) as diagramTIMESTAMP.pdf.

********************
Parsing your schema with DAVILA

DAVILA is designed to load and parse the schema file for your database.  Version 0.1 is released with a parser (found in the file Parser.pde and called as the last line of the setup() method in DAVILA.pde).  This parser (named parseRails2fkcSchema) can extract the necessary information from a Rails 2.x schema that uses SQL92 inserts to create foreign key constraints.  

Overtime I hope to increase the parser library until it can handle all Rails schemas and most SQL schema variations.  However, since SQL is a non-orthogonal language (and therefore every developer seems to have her/his favorite way of writing SQL Schemas), I will be relying on the user base to help improve the parser library.

The Parser.pde file is heavily commented and should be relatively easy to modify for a different Rails/SQL schema.  If you create a new parser, PLEASE contribute it back to the project and let me know how you want to be credited in the code comments and larger documentation.  

Processing makes use of the Java regex library.  

If your schema is called something other than schema.rb (or is located outside of the data folder) you will need to alter the code splits the file into an Array.
	* Inside the setup() method there is line that reads
		schema = loadStrings("schema.rb");
	* Replace the "schema.rb" with the name and location of the file (if your file is still in the sketch folder but not in the data folder you do not need a path) -- this is the only line of code you need to change
	* you can also use a URL to pull the schema from a server -- just remember that you need to update the CSV file every time you add a new Entity to the database, make sure you include the full path "http://yourschemafile.sql"

********************
Annotating your schema with DAVILA

DAVILA makes it easy for database developers to create visually appealing and semantically rich interactive diagrams to document their programs.  Ideally, an instance of DAVILA should make the design and purpose of the database clear to non-technical users (or other members of the design team who are unfamiliar with database structure).  

Along with the database's schema, DAVILA loads a CSV file (using "|" as the delimiter rather than comma).  A sample schema and CSV file have been included with this installation.  They can be found in the data folder.

The CSV File has been heavily annotated for use as a template.  You can either replace the relevant lines or make a copy.  If you name your CSV file customize.csv and leave it in the data folder you will not need to alter the program.  If you change the name and/or location of the CSV file you will need to alter the code splits the file into an Array.
	* Inside the setup() method there is line that reads
		custom = loadStrings("customize.csv");
	* Replace the "customize.csv" with the name and location of the file (if your file is still in the sketch folder but not in the data folder you do not need a path) -- this is the only line of code you need to change
	* you can also use a URL to pull the csv file from a server, make sure you include the full path "http://yourcustomizationfile.csv"

********************
Common reasons to alter the code

DAVILA has been designed for non Processing users as well as those familiar with the language.  Everything you see in the example applet (or by running the code, unmodified) was parsed from the schema and the CSV file (except for the attribution text in the lower right corner).  However there are some standard customizations which cannot be handled through the CSV file.  

* Changing the size of the display window -- this is handled by the size(height, width) method (see http://processing.org/reference/size_.html for more information).  It is the first method called in setup(), found in DAVILA.pde  
	* NOTE: If you change the size and plan to export your sketch as an applet, check the applet to make sure that everything is visible in the window.  The applet export feature in Processing can generate weird results, and depending on the complexity of your schema, some entities may "fall off" the bottom or left side of the sketch.  Play with the applet size until you find something that works.

* Changing the font -- DAVILA ships with the ttf files for two open source fonts Bitstream Vera and Bitstream Vera Bold.  If you want to use another font, you must add the new ttf files to the data folder and change the createFont lines in setup().  If you plan to redistribute your modified version of DAVILA make sure you use fonts that can be redistributed with the code (or revert to the Vera fonts).  

* Changing the color of the text or arrows -- this is handled by a series of fill() methods throughout the code.  See http://processing.org/reference/fill_.html for more information about this method.

* Writing a new parser -- place the code for your new method in the Parser.pde file and call it in setup() in DAVILA.pde.  Comment out or delete the current method call parseRails2fkcSchema in the last line of setup().

********************
Known Issues

* Error message while running the code: "textMode(SHAPE) is not supported by this renderer." -- Ignore this error message.  It happens every time the code runs, but does not actually indicate a problem.  In fact, the PDF renderer does support textMode(SHAPE) and if it is removed will not render bold fonts in the PDF snapshot.  Go figure.

* Error message while compiling the code "It looks like you're mixing "active" and "static" modes" -- this is an open bug in Processing 0182.  Basically all errors in this latest version route you to the setup() method in the main sketch (here DAVILA.pde) and display this message, regardless of the actual problem (typo, missed semicolon at end of line, wrong number of parentheses, etc.).  Best solution at the moment: look closely at the code you just changed and figure out what went wrong. To follow this bug and/or vote for its importance see http://dev.processing.org/bugs/show_bug.cgi?id=1539.

* Bold text rendering as normal weight in PDF export -- this will happen if you are running a version of Processing earlier than 0182 or are not using ttf (or otf) files while creating your bolded font.

* Arrows acting weirdly -- This is a function of the code used to connect the rectangles with arrows.  It turns out this is a non trivial problem and an interesting thread has developed on possible solutions (see http://processing.org/discourse/yabb2/YaBB.pl?num=1269916012/0).  Currently I am still using the first solution I developed (approximating the rectangle as an ellipse and making the Entity displays translucent so that the arrow heads still display -- see the draw() method in DAVILA.pde).  If you have "lost" an arrowhead when expanding an Entity, click and drag it around until it reappears.  If you have a better solution please let me know.

* Overlapping Entities -- toxiclibs is an excellent physics library but it is not very good at collision avoidance.  Play with the String lengths in the createRelationship method in DAVILA.pde to get a better fit for your schema.  

* Self joins represented with only 1 arrow -- toxiclibs only allows one spring to connect two entities, so that means one arrow between a self-join and its related table.  Any and all suggestions welcome.

If your problem is not addressed above, first check out the documentation and forums on http://processing.org to make sure this is not Processing (or toxiclibs) issue rather than a DAVILA issue.