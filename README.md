# EthosFederationList

## EthosFederationList Overview
EthosFederation is a supplemental tool which is part of the Telegram module on Ethostools.ai, to provide a web interface for users to work with and manage a Telegram Rose Federation Ban List.  There should be main components on the screen.  A visualizer for the data inside of a database that neatly displays the contents of a Telegram Federation list of banned users which was imported into it.  There should be a section to monitor the Federation Logging channel which is resizesable.  There are other tasks, which are detailed below, that should be arranged on the screen for the user to be able to see and use.  There should also be an audit section that allows the user to see who did what on this web utility.

This is meant to be a self contained module that works with managing a Federation Ban List but the data in the databases can be queried and used for forensics by other parts of the ethostools.ai suite.  


## Prerequisite Information Needed
This project will require some information before it can be started and become functional.  This information, upon the first run of the application, will prompt the user for the following (notice the labels in parens at the end of each item so that I can reference them later in this doc) and should be stored in a long term database:
- Channel ID For The Federation Logging (logging)
- Channel ID For Issuing Authorized Federation Bot Commands (auth)
- Bot API Key for a bot to monitor, watch, and take action on new FBAN messages (botmon)
- Bot API Key for the bot to issue Rose Federation Admin commands from the Issuing Authorized Federation Bot Commands channel ID (botapi)
- The 1776Federation Hash Value (fedhash) (Example: 19e4d335-50df-43dc-9e89-42afb9456613)
  - **Must** be the Federation Owner

This EthosFederationList project should be its own web based module that opens up and is meant to manage a Telegram Rose Bot Federation List and should start up with the information gathered from the prerequisite section above.  Now that we are able to talk to Telegram, our channels, our bots, and know the Federation hash then the next step will be to import the Federation Ban list Data Into the EthosFederationList project by exporting it from Telegram.  This should be done from the EthosFederationList UI module by pressing a `EXPORT` key.  First, we need to have a long term database created that can store this information shown below.  Keeping in mind that at this writing we have about ten thousandd bans (or database records) to be created but we need to make sure that this database persists, is free and reliable, and can potentially hold hundreds of thousands of records.  

## Federation Ban List Database Creation / Information
The Federation Ban List should be stored into a long term database.  On the first run of this application, there won't be a database set up yet for this so on the first export then a database should be created.  Each banned UserID will be a record in the database.  Each record should contain 6 fields.  Those fields within each record are as follows:
- 1st field, UID - call it `uid` (numeric UID of banned account)
- 2nd field, First Name - call it `firstname` (string: can contain emoji, cyrillic, etc) (can be empty in most cases - replace NULL with BLANK as field data if the field is empty)
- 3rd field, Last Name - call it `lastname` (string: can contain emoji, cyrillic, etc) (can be empty in most cases - replace NULL with BLANK as field data if the field is empty) 
- 4th field, Username - call it `username` (string: can contain emoji, cyrillic, etc) (can be empty in most cases - replace NULL with BLANK as field data if the field is empty)
- 5th field, Reason - call it `reason` (String) (can be empty or can contain lots of information - whole field should be a record)
- 6th field, Fed Admin - call it `fedadmin` (String) This is username of the Federation Admin that issued the ban and will be used for tracking and auditing
  - It is vital to know that this field will only be filled in by the (botmon) account observing the (logging) channel and parsing the messages that Rose will post when Federation Bans occur.
  - It is vital to know that the exported Federation Ban lists will not contain this field within the CSV file but should still be created (and N/A stored as the value) for future use after the (botmon) goes live.

## First Run Only Operations
Once this blank database is created as part of the first run, then we need to add the Federation ban list data into it.  The user should be prompted with:
- Point to a Federation Banlist CSV file on your local computer and upload it (meaning you downloaded it yourself and have it locally on your system)
  - This will then ensure the filename is created, stored, and saved properly (methods are shown in the next section: *Exporting and Sorting Federation Data Snapshot Files*)  
- Once the initial CSV file has been uploaded to the system, then the user will be prompted to `IMPORT` the data into the database detailed above.
  - Instructions for this will be in a below section called:  *Import A Federation Banlist From A CSV File Snapshot*

## Exporting and Storing Federation Data Snapshot Files
- **it is critical to remember that only the Federation Owner can issue the import / export commands to and from a Federation.** If a Federation Admin attempts to run the import/export commands, they will receive an error message from Rose that they must be the channel owner.
- **It is critical to remember that each time a Federation Banlist is exported, it will be renamed and saved in the folder tree below for one year.**
- **It is important to remember that all of the downloaded exports can be used as `RESTORE POINTS` if something bad happens to the Federation list.**
  - There should be an automatic `EXPORT` performed once a day and saved according to the naming conventions below.
  - Manual `EXPORTS` will also be saved in the folder tree mentioned below.
- Every time a user clicks the `EXPORT` key
  - Near the `EXPORT` button, there needs to be a some data recording the last time that the Federation had a successful export command issued.  The `EXPORT` button should be a grayed out for 30 minutes after a successful run.  There should be a countdown also showing how long until the next time they can click `EXPORT`
  - When a user successfully clicks the `EXPORT` key, the time and date should be recorded and who the user was that pressed the button to export.  This data will be linked to the filename of the CSV export mentioned below so that we can know what user requested an export, what time and date it was at, and where it has been stored neatly (more info below).
- When the user clicks `EXPORT`, then the (botapi) bot will send a command to the (auth) Channel
  - Command to send: `/fedexport`
    - This will let Rose know to export the Federation Ban List from (fedhash) in a CSV formatted document.  The (botapi) bot should be waiting for a reply from Rose in the same channel which will contain a link to download `fbanned_users.csv` which it should then download.
    - Once the `fbanned_users.csv` is downloaded, it needs to be stored in a folder structure of something like FederationExports/(fedhash)/FILENAME where its new FILENAME becomes formatted as the following:
      - Date in the format of 05122025
      - Time in the format of 1605
      - Body in the format of fbanned_users.csv
    - **Example of archived export filename:** 05122025-1605-fbanned_users.csv
    - **Example of path to newly named export file:** FederationExports/19e4d335-50df-43dc-9e89-42afb9456613/05122025-1605-fbanned_users.csv
    
## Providing A Local Export CSV File Option
- There should be an area where a locally stored export file can be chosen to be loaded instead of having to click the EXPORT button.  Doing this will upload the local CSV file into the file folder containing all of the snapshots.

## Import A Federation Banlist From A CSV File Snapshot
There should also in the UI be a button that says `IMPORT` and when clicking that, a GUI interface opens up showing all of the saved CSV snapshots that we have stored.  Then, the user should be able to select the file that they want to use to populate the database.  Once the user selects the file they want to import in from out list of neatly named files, they should be prompted one last time asking if they are sure they wish to proceed.

Once they confirm, then the contents of this CSV file should be imported into our database structure mentioned above.

Now the user can `BROWSE` our database using a visualizer that allows them to scroll through, edit, search, search and replace, and filter results or simply just view everything.  There should be some statistics generated after the import is successful on things like:
- How many total records there are
- How many `reason` fields are similar matches
- Any kind of strange patterns with UIDs such as sequential IDs or other anomalies
- If any changes are made they should be saved and there should be a main audit log somewhere detailing what user changed what and on what day and what time

Once the user has imported a successful snapshot CSV file, they then should have a button show up that they can press now called `PUBLISH`  The next section details these actions.

## Publishing A New Federation List
This section goes into the ways needed to effectively push out a new version of the ban list up to your Telegram Rose Federation and activate it into Production.

### Reasons To Manually Publish
- If you have made manual changes within the GUI tool to the records in the database on the EthosFederationList GYI tool.  (Such as reformatting information in a field on all records, etc)
- If you need to restore back to a previous date for some strange reason (I have never had to do this before)

### Instructions To Publish (or import) A New CSV Into Rose Federation
This is an operation that the bot (botapi) will perform.  In the Telegram Desktop when a human does this, it is a two step process shown below:
- A Copy of the new csv file is uploaded into the (auth) channel
- A message is then sent in REPLY to the csv file just sent saying `/fedimport overwrite`
  - Once this command is sent, Rose will then use this to overwrite values in the Federation with the provided CSV file and changes are live instantly


## Bot Monitoring Federation Log Channel
We want to have a bot (botmon) idling in the Federation Log Channel (logging).  

This (botmon) should:
- Be connected via API to this bot (botmon) which is idling and watching in the Federation Logging Channel (logging)
- The intent is to have this be a ongoing data stream which could appear in a window section on the EthosFederationList user interface in a resizable window.
  - The want is to be able to be attached to this bot and all of the new FedBans that are added during the day by humans will scroll in this window as an almost real time view.  If it is easier for buffering this data then do it that way.  

## Batch Adding Federation Ban IDs
There are times when a new list of user IDs are gathered and need to be added into the federation ban list.  We want this to be a section in the GUI on the page and a button to press that says `BATCH ADD`.  This will allow the user make a choice how to add IDs to ban.

### Providing the list of IDs to add to the Federation Ban List
- One way is to take a screenshot of a bunch of UID numbers.  The system should then be able to OCR the ID numbers from this picture and created a list of IDs to be banned.
- Another way is to put a list of IDs into a field with commas in between each ID number, or, the user can paste a list of IDs with one ID per line.

### Providing a Reason For The FBAN
- The user can choose to provide a reason for the block of IDs that will be added into the Federation Ban List.  This will be done by typing in a reason into a field which will be set into the `reason` field for these new records.  The user can also choose not to give a reason and pick from a list of "frequent reasons".  This list of frequent reasons should be configurable by the user through a small utility where they can set up canned reasons to be selected if they choose not to type out a reason.  These reasons will be appended to the commands used to initiate the Federation Bans by our bot.

The end result is now that our tool has a list of IDs that now need to be added into the Federation in real time.  This will be completed by our bot (auth).  

### Instantiating the Federation Bans
Our (botapi) API bot will then already be idling (always listening) in our Command issuing channel (auth) and will issue the commands to send to the (auth) channel to set the new Federation Bans.  It will iterate through the list of IDs that have been batch requested and it will issue the correct command to create the new FBAN.  
- Example:
  - We have 5 IDs to batch add (6005000000, 5023400000, 5567852300, 5023783700, 2000300600)
  - The API Bot (botapi) would issue five separate commands with a 2 second delay in between each ID, example:
    - /fban 6005000000 05122025|BATCH|`REASON`
    - /fban 5023400000 05122025|BATCH|`REASON`
    - /fban 5567852300 05122025|BATCH|`REASON`
    - /fban 5023783700 05122025|BATCH|`REASON`
    - /fban 2000300600 05122025|BATCH|`REASON`
      - Notice the space delimiter and notice the 3rd field.  The third field will be constructed of the date followed by a | symbol, the word BATCH followed by a | symbol, and then the `REASON` that the user typed in on the batch creation dialog (or the reason they selected from canned reasons).
   
- All batch additions should be logged in the system recording auditlog with what user executed the batch, what was added into the batch, the reason, date, and time.

## Rose Federation Documentation and Commands
In order to show our Agents what commands are available to users related to Federations, the relevant commands below are broken down into two main sections.  Almost all of the commands that EthosFederationList will send to our bots in Telegram will be from the Owner Commands below.  We might use more of these commands in the future as this project grows but just providing this for context and documentation and learning.

### Section 1 - Federation Owner Commands

These are the list of available fed owner commands. To run these, you have to own the current federation.

Owner Commands:
- /newfed <fedname>: Creates a new federation with the given name. Only one federation per user.
- /renamefed <fedname>: Rename your federation.
- /delfed: Deletes your federation, and any information related to it. Will not unban any banned users.
- /fedtransfer <reply/username/mention/userid>: Transfer your federation to another user.
- /fedpromote: Promote a user to fedadmin in your fed. To avoid unwanted fedadmin, the user will get a message to confirm this.
- /feddemote: Demote a federation admin in your fed.
- /fednotif <yes/no/on/off>: Whether or not to receive PM notifications of every fed action.
- /fedreason <yes/no/on/off>: Whether or not fedbans should require a reason.
- /subfed <FedId>: Subscribe your federation to another. Users banned in the subscribed fed will also be banned in this one.
Note: This does not affect your banlist. You just inherit any bans.
- /unsubfed <FedId>: Unsubscribes your federation from another. Bans from the other fed will no longer take effect.
- /fedexport <csv/minicsv/json/human>: Get the list of currently banned users. Default output is CSV.
- /fedimport <overwrite/keep> <csv/minicsv/json/human>: Import a list of banned users.
- /setfedlog: Sets the current chat as the federation log. All federation events will be logged here.
- /unsetfedlog: Unset the federation log. Events will no longer be logged.
- /setfedlang: Change the language of the federation log. Note: This does not change the language of Rose's replies to fed commands, only the log channel.

### Section 2 - Fed Admin Commands

The following is the list of all fed admin commands. To run these, you have to be a federation admin in the current federation.

Commands:
- /fban: Bans a user from the current chat's federation
- /unfban: Unbans a user from the current chat's federation
- /feddemoteme <fedID>: Demote yourself from a fed.
- /myfeds: List all feds you are an admin in.


## Example Federation Banlist CSV Data
This is a snipped of the comma delimited data from the main Federation banlist after an export to show examples of the data that will be put into the database.  Remember that this only shows 5 fields of information but we specified 6 earlier in this document to be created in the Database.  The 6th will add a field containing the Federation Hash (fedhash).

```
8023555835,Kim,,,eat shit
5507699310,Eva,Adore,,hi spammer
7806939524,Gordi,,,spammer
7978514484,Jassica,,jessica620t,bot
7352990838,Amy,Obrien,,04292025|SPAM|turkish commiie political sway bots spotted
7720114513,,,,04212025|SPAM|BOTLEVEL:3|CHANNEL: SPARTIALHUB FOR PATRIOTS
7874823767,,,,04212025|SPAM|BOTLEVEL:3|CHANNEL: SPARTIALHUB FOR PATRIOTS
1579416988,Increaser Jo,,regulus1,04212025|SPAM|BOTLEVEL:1|CHANNEL: SPARTIALHUB FOR PATRIOTS
1240739228,Luminated_Patriot,,,04212025|SPAM|BOTLEVEL:1|CHANNEL: SPARTIALHUB FOR PATRIOTS
7192579365,CryptoTechCommunity,,,04212025|SPAM|BOTLEVEL:3|CHANNEL: SPARTIALHUB FOR PATRIOTS
6469434315,Dave xrp,,,04212025|SPAM|BOTLEVEL:2|CHANNEL: SPARTIALHUB FOR PATRIOTS
7166494417,Hamita 4,,haaaamita4,04212025|SPAM|BOTLEVEL:3|CHANNEL: SPARTIALHUB FOR PATRIOTS
7863032634,Sologenic & XRP Community Chat,,,04212025|SPAM|BOTLEVEL:3|CHANNEL: SPARTIALHUB FOR PATRIOTS
7043349908,Jane,Hagenbuch,janehagen,04212025|SPAM|BOTLEVEL:3|CHANNEL: SPARTIALHUB FOR PATRIOTS
5033999290,Prashant A U,,southpau79,04212025|SPAM|BOTLEVEL:2|CHANNEL: SPARTIALHUB FOR PATRIOTS
7731396530,juju😀,,alexamarrlliin0104,04222025|SLUTBOT|BOTLEVEL:3|CHANNEL: +t3rpRi6d-ENlYzNi 2604452010
2604452010,,,,04222025|SLUTBOT|BOTLEVEL:2|CHANNEL: +t3rpRi6d-ENlYzNi 7731396530
7529194502,,,,04222025|SPAM|BOTLEVEL:3|CHANNEL: SPARTIALHUB FOR PATRIOTS - SUPPLEMENTAL BOTS
812160458,Marduk,,peterframpton11,04222025|SPAM|BOTLEVEL:1|CHANNEL: SPARTIALHUB FOR PATRIOTS - SUPPLEMENTAL BOTS
6444921265,Liam,Ward,lward92508,04222025|SPAM|BOTLEVEL:3|CHANNEL: SPARTIALHUB FOR PATRIOTS - SUPPLEMENTAL BOTS
6064203218,Tanmay,,i_am_yours_tanmay,04222025|SPAM|BOTLEVEL:3|CHANNEL: SPARTIALHUB FOR PATRIOTS - SUPPLEMENTAL BOTS
1740748544,,,,04222025|SPAM|BOTLEVEL:1|CHANNEL: GITMO_TV - OWNER
2042188003,LT.DIRECTOR STAFF,-DE OPPERSSO LIBERE-,deopperssolibere,04252025|SPAM|BOTLEVEL:2|CHANNEL:  SPARTIALHUB FOR PATRIOTS
1217008361,Ragupathi,K,,05092025|SPAM|BOTLEVEL:1|REASON: royal cash crypto scammers
7816160405,Joanne M Mitchell,,,05092025|IMPERSONATOR|BOTLEVEL:3|hi spammer
```
