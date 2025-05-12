# EthosFederationList

EthoseFederation is a method for Ethos Agents to export the Federation List from 1776Federation on Telegram, santize it, normalize it, store it, and send back (or import) back to the Telegram Federation.  

The initial export of the 1776Federation will have mostly standardized information except for the reason for the FBAN.  The reasons will be the area of focus to standardize into a certain format (mentioned in TASK.md).

Once the intial data is standardized and saved into a long term database, our project will generate a new CSV or JSON file and import it back into the 1776Federation so that all replies and logging are standardized.  This operation will be run once on a newly linked Federation.  It could also be used as a sanity check and run on a conservative cadence.

This santized and standardized data now in the EthosProject Federation database can be used by other areas of EthosProject for data anlysis, comparisons, and cross referencing other trends or incidents.

There will be EthosFederation Telgram Bot accounts watching the 1776Federation log channel for new FBANs and promptly check for duplicate IDs, sanitize, standardize, and store into the long term database to keep things in sync.

There will be methods in the UI that allow for single or batch additions into the 1776Federation long term storage database and a nice UI to visually look at, change, and manipulate the 1776Federation Data.

There will be options as well from the UI which will allow the 1776Federation to take advantage of other reputable Federations on Telegram.  For example, the authors of Rose also have a massive Federation with thousands of spammers, scammers, bots, etc.  A radio button to subscribe the 1776Federation to another Feeration or multiple Federations will make this easy and instant.



There will be an additional EthosFederation bot sitting in the Federation logging channel which will watch for future FBANs and an EthosAgent will be controlling this bot by watching for logs to arrive and promptly adding them into the long term database in real time.  This will effectively keep the EthosProject database synced up with the live Federation on Telegram.  Duplicate addtions to the Federation will be stopped at the Telegram level but logic should exist to prevent duplicates from being added into the long term database


After the tool exports the CSV list from Rose, it will eventually end up in a long term persistent database of sanitzed and normalized data.  Once the initial data santization has been completed (notated further down in this duc
EthosFederation is a Telegram tool that supplements the very popular spam fighting bot called Rose (@MissRoseBot).  Rose provides what is called a Federation Ban List which allows for multiple Telegram channels to subscribe to a Federation List and take advantage of the list of banned users and channels which prevents them from entering into a channel.  This project will take an existing Federation list that I have been maintaining and turn it into a tool to be leveraged by channel administrators and owners to proactively prevent spammers from entering and/or spamming anything.  The Federation list will be managed, standardized into a JSON format, and updates will be posted into an Ethos Federation notification channel.  This data will also be used in other Ethos modules for pattern matching and cross referncing professional spammers.  This project is to create an interface to export, standardize, import, and publish regular updates for subscribers.

## Features

- **Centrally Managed**: The 1776Federation will now be centrally managed within the EthosProject inside of the EthosFederation module.
- **

## Prerequisites

- A verified Telegram account.  This main Telegram account is the MASTER account.  Main accounts start with the number 1 in their user ID.
- An existing Rose Federation
- 
