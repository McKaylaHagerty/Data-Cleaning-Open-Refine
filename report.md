Homework 2
================
McKayla Hagerty

CS 625, Fall 2020

## Part 1: OpenRefine Report

### General Cleaning Plan

Since this was my first experience with OpenRefine and my first attempt at cleaning data, I wanted to first briefly explore the data and set a general plan of attack. I knew I would encounter unexpected challenges, but I also knew that a logically set plan would keep me on track. 

1. Kind of Pet Column
At first glance, this column appears to be relatively easy to clean which is the main reason I decided to start here. I quickly knew the most popular type of pet in this set of data is dog. The main problems were in inconsitancies with how data was entered and in the 21 entries labeled "other" or blank. I wanted to try to fill those in if enough additional information was provided. At this point, I wasn't sure how many of the types of pets could and should be combined. My plan was to make this decision after sorting through the types provided. 

2. Age Column 
After initially looking at the age column, I recognized room for growth in my knowledge of and understanding in OpenRefine. I hadn't dealt with numbers much in any previous tutorials so I knew I would have to consult other sources. In the initial investigation, I saw that the majority of entries were in years, either without a unit or followed by "years," "yrs," year," and several other variations. Some data included other notes that would need to be removed. Other ages were entered in months. I figured I would convert all these to years without a unit. My challenge would be in learning how to best make that conversion.  

3. Pet Name Columns
The pet name columns, both full name and nickname, seemed to need the least cleaning. I did notice multiple nicknames in some cases. I knew I didn't want to delete any of these multiple nicknames, but that I needed to somehow separate them in order to answer the questions about most popular names. This was something that would need more thought. 

4. Breed Column 
The breed column initially looked the more daunting. As someone who knows, or at least knew, very little about breeds, I left this for last to allow myself to gain familiarity and confidence in OpenRefine first.  

### Plan in Action, Unexpected Discoveries, and The Learning Process

#### Kind of Pet Column

Starting with the *What kind of pet is this (Dog, cat, Bird, Other)* column, I initially saw a variety of animals with only 19 labeled *other* and are therefore unknown. I also saw 2 blanks in this column which I decided to investigate later. There were also some odd submissions like Ca, Car, Card Board Poster, Roomba, Robot, and Virus. To start, I chose to cluster by moving through the default methods and keying functions. With this I was able to quickly merge 1115 dog rows and 492 cat rows as well as some various spellings, capitalizations, and subcategories of other types of pets.

I was able to confirm some of the merges by browsing the cluster and viewing the other columns. Some other suggestions I rejected. For instance, metaphone3 provided the merge of Ca into Cow. After investigating the other columns in the row with Ca, I could not find anything to confirm that it should be cow. This was more likely a typographic error and should be cat. Also, I decided Robot should not be merged with Rabbit because the given pet name was Roomba, a common vacuum robot. Similarly, the pet type *God* was suggested to be *Cat*. However, the species name was entered as *Golden retriever* so I felt comfortable manually changing that kind of pet to *Dog*.

Once this was complete, I adjusted the radius for the nearest neighbor approach. During this process, I realized I had been grouping all fish together but that one of the questions asked for the number of Gold Fish and Betta Fish so I went back into my work and decided that I could leave the pet bread column to specify this difference. I found two columns that were originally betta fish (150 and 1286) and manually added/specified the type of fish in the pet breed column. I could then change the type of pet to fish. 

The automatic clustering was struggling with properly grouping any type of pet that had the word other in it, so I manually edited the three rows that included the word *other* along with the type of pet. It was easy to search for these columns using a text filter. At this point, I was left with some lingering individual issues with names like *Kitty Meow* and *Tortoise* as well as some *blanks/others* that could be filled in using the additional information. These few were edited manually with the guidance of the combination of information in all the columns. I made sure to move the too specific answers to the breed column for now as to not lose any information that may be helpful in cleaning other columns. 

I also found a row that was a combination of what should be 4 individual rows. I used the split cells option to create these 4 individual rows. The rest of the data in the row followed the same pattern of separation by comma or spaces so I ensured the right data was now in each new row.   

After clustering and making some necessary manual changes, I decided to delete some rows that were not relevant to the data (like *Card Board Poster, a blank row, Virus, Server, Roomba, Robot, two unidentifiable other rows, and Car*). After looking more into these rows, I decided they were very likely not rows of data that should be taken seriously. At this point, I had 23 animal types and 1778 records. 

#### Age Column 

Next, I moved to the age column. Using the text facet, I found 200+ entries that are not purely numeric. I used GREL custom facet to remove years, year, yr, y, ~, >, and =. When I say that I have replaced anything in the remainder of the report, this is the method I used.

`value.replace("years","").replace("year","").replace("yr","").replace("y","").replace("~","").replace(">","").replace("=","")`

I changed the whole column to lowercase to make additional bulk editing easier. In retrospect, this should have been done first so I repeated the custom facet above. Throughout this process, I occasionally used clustering which allowed for some additional initial cleaning (mostly by removing typographical errors). Some of the text heavy data needed to be manually changed. After continuing to remove unneeded text like *ish* and replacing *1/2* with *0.5*, I needed to convert the months/weeks to years. 

For converting the weeks to years, I used:
`toNumber(value.replace("weeks",""))/12.0`

For converting months to years, I used:
`toNumber(value.replace("weeks",""))/52.0`

At first, I divided by 12 and 52, but this resulting in numbers without any decimals. Since I was working with a portion of years, the decimal places were needed. This was fixed by using 12.0 and 52.0. This took care of most of the conversions. I did miss a few that didn't have the exact unit of *weeks* or *months*. These cases became more obvious once that majority was converted. I adjusted these units and repeated the converting process above.

Some key choices were made along the way. I averaged out the years for the data that indicated a range. There were also two rows with the pets age of *4/1/02*. I looked back at the original date of the data collection and determined the age to be 17 as of that date. To indicate the deceased pets without cluttering the age column, I created a new column by splitting the age column. Only those pets who have passed away are marked deceased in this column and the age in the age column are as of the death date. Cleaning this column ended up taking longer than I expected because I was experimenting and trying to learn the most efficient way to improve my efficiency and speed for future dataset cleaning. 

In the process, I found another row for *Iggy and Ziggy* which should have been two rows, so I split the row and looked at the original data to make sure the age was still correct. I used text filtering to search the pet full name column for *and, commas*, and *semicolons,* to try to find any additional rows that should be multiple rows. I found two additional cases of this with *Bella Nina; Thomas James; and Archie* and *Dr. Watson & Dr. Crick* which I took the same steps for splitting before moving forward.

#### Pet Name Columns

I first switched the entire column to title case. I scanned through this column and made minor adjustments like taking out some extra notes in parentheses. As for pet everyday name, many entries have multiple names, so I converted the separators into a semicolon and split into multiple columns. I figured this was the best approach based on the questions I knew I would have to answer about name popularity. I found the multiple names by filtering the column by various potential separators (*comma, or, and, aka, /*). I left the original column and added additional columns splitting by the semicolons. For pet names, I chose to be particular in the clustering I accepted. Various spelling choices and unique names are expected in both these columns. For instance, one of the everyday names was *Zooey*. While there is a chance that this should have been *Zoey*, I didn't feel like that was my call to make. 

#### Pet Breed Column

Last to be addressed was the pet breed. I'm glad I chose to work with the other columns first. This allowed me to more easily determine any rows that needed split and to provide myself with the most a clearest information when trying to interpret and clean the breed column data. My initial thought was to filter by pet type and work with the breeds from there. I started with changing to title capitalization and clustered with the various settings. At first, I again forgot the capitalization which I quickly realized slowed cleaning down. If only one breed was listed but also that it was a mix (example: Terrier Mix), I chose to remove the *Mix* label and leave just the given breed. 

Throughout this process, I was forced to learn much about pet breeds, like that Heinz 57 is another name for a mutt or mixed breed dog. I decided that any entry that had 4 or more breeds listed would be considered and labeled *Mixed*. I alternated between bulk clustering and individual editing, keeping the mixes separated by a slash. Once I had condensed the number of choices to about 300 different breeds/combinations, I split the columns using the inserted slashes as separators. My thought was that someone could use just that first separated column with the assumption that people typically list the most prevalent breed first. The other columns add more context if desired. 

This column took extensive research. For instance, many of the cat breeds were not breeds at all and many nicknames were used throughout the column. I may have not caught all of them, but I did put in many hours researching specifics after and during grouping. With the issue of the number of cat breeds that were colorations, not breeds, I changed the ones where I had enough information to do so and left the coloration on the others. Any submissions that indicated the cat was a *stray* or *feral* or *mixed*, I changed to *Unknown*. Blanks were either blanks before cleaning or had useful breed information like simply *Cat.* Once I was down to 30 cat breeds, I went down the list to make sure they were all a breed. I also added the word *coloration* after calico and tuxedo for increased clarity. I learned that shorthair, mediumhair, and longhair are typically mixed breeds, but are recognized as breeds.

Isolating and sorting through the breeds for hamsters, birds, chicken, elephant, and other pet kinds was easier with the low numbers. I shifted through them with multiple text facets and filters. Some changes to note include deleting the elephant row because it seems to be a joking reference to Dungeons and Dragons, changing the kind of pet for birds and chickens to bird, and adding a prairie dog type of pet. 

[Sources for dog breeds](https://dogtime.com/dog-breeds/profiles)
[Sources for cat breeds](https://wamiz.co.uk/cat/breeds)
[Sources for hamster breeds](https://squeaksandnibbles.com/hamster-breeds/)

Once each type of pet had been checked systematically, I split the breed column by "/ " to allow for the separation of the mixed breeds (primarily for the dogs). Some edits were needed that weren't caught when in the combined mixes column. There was also a blank row that I deleted at this point.

Something that I wish I had done early on was to replace *Labrador Retriever* (and its common misspellings) with *Lab* and then *Lab* with *Labrador Retriever*. There were many variations and misspellings of how Labrador retrievers were reported. This would have eliminated some manual editing. 

For the breed column, I could have condensed to less choices (like grouping all Terriers), but I followed my resources and didn't combine breeds which are officially considered separate breeds. Many mixes are not considered breeds of their own, so I left the two or three breed names. 

Something else that I discovered and implemented later than I would have liked was how to remove leading and trailing whitespace. As I was replacing and removing words/phrases/numbers in bulk, I occasionally accidentally left unwanted spaces before and after the word, phrase, or number. This led to more manual cleaning because I didn't want to remove all spaces, just the leading a trailing space. This is until I discovered the following: edit cells > common transformations > trim leading and trailing whitespace. Even implementing it towards the end of the cleaning process, I caught almost 200 hidden spaces throughout the data that may have led to inaccuracies in answering the assignment questions.  

The last step I took was to change the column names to better represent the column. I took a final quick look at each of the column text facets and downloaded the data as a csv file. 

### Conclusions and Final Thoughts

The variety of data in this dataset allowed for significant learning throughout my first data cleaning attempt. Choices I made throughout the process had a significant impact on my final presentation of data. From first glance, the most impactful of these choices was to split the pet everyday name and breed columns into multiple columns. I chose to keep the original columns as well in case they are wanted for later analysis. For the first breed column with all the breeds listed, I left the original order. For instance, there is a *Beagle/ Jack Russell Terrier* category and a *Jack Russell Terrier/ Beagle* category. I cautiously chose not to combine these because of the assumption that people tend to list the most prevalent breed first. This allows the second breed column to give a better representation of the single breed associated with each pet should that information be needed. 

The assignment was truly a learning process. I didn't know what I still needed to know when I started. It wasn't until I found myself embarking on certain time-consuming processes that I realized I needed to find another method. I chose to not undo previous actions but instead move forward and find a more efficient method for the remaining edits.  

## Part 2: Report Questions

*1. How many types (kinds) of pets are there?*

There are 22 types of pets. During my cleaning, I found a prairie dog mixed in the *Dog* category so I made a new category called *Prairie Dog*. I chose not to combine the rodent categories because not all had breed information. By combining them, I felt information for some rows would be lost. 

*2. How many dogs?*

I ended up with 1132 dogs. Some of these were from previously combined rows. 

*3. How many breeds of dogs?*

The first breed column indicates 279 breeds/breed combinations. The second column has 174 breeds. This column is the only or first breed listed as described in my report. Also note that I chose to not combine freely. Instead I referenced my sources and stayed consistent with the recognized breeds. For context, there are a total of about 339 recognized dog breeds.

*4. What's the most popular dog breed?*

The most popular dog breed is the Golden Retriever followed by the Labrador Retriever. There are also two pets that are listed as simply retrievers. I did not have enough information to farther categorize these two cases.

*5. What's the age range of the dogs?*

The dogs are between 0.1154 (6 weeks) and 22 years old.

*6. What's the age range of the guinea pigs?*

The guinea pigs are between 1 and 5 years old.

*7. What is the oldest pet?*

The oldest pet is Bruce Springsteen, the 24 year old cat.

*8. Which are more popular, betta fish or goldfish? How many of each?*

Betta fish are more popular. One fish without a breed listed is named "Alpha the Beta" so I decided to edit the row and add its breed.

Betta count: 14
Goldish count: 7

*9. What's the most popular everyday name for a cat?*

Using the second everyday name column, Kitty is found to be the most common with 8 occurrences. 

*10. What's the most popular full name for a dog?*

Maggie and Sadie are the most popular dog full names. Both have 7 counts. There is also one count each of Maggie Peaches, Maggie Pie Thatcher, Sadie Blue, Sadie Jo, and Sadie Mae.

## Resources

1. [Sources for dog breeds](https://dogtime.com/dog-breeds/profiles)
2. [Sources for cat breeds](https://wamiz.co.uk/cat/breeds)
3. [Sources for hamster breeds](https://squeaksandnibbles.com/hamster-breeds/)
4. [Beginner OpenRefine Tutorial](http://web.archive.org/web/20190105063215/enipedia.tudelft.nl/wiki/OpenRefine_Tutorial)
5. [Transforms](https://nesclic.github.io/openrefine-socialsci-update/04-transforms/index.html)
6. [Data Cleaning Tutorial](https://multimedia.journalism.berkeley.edu/tutorials/openrefine/)
7. [Dates in Data Cleaning](https://icantiemyownshoes.wordpress.com/2014/04/24/clean-up-dates-and-openrefine/)
8. [General Refine Expression Language](https://github.com/OpenRefine/OpenRefine/wiki/General-Refine-Expression-Language)

