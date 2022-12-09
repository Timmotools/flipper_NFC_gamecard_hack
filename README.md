# flipper_NFC_gamecard_hack

In Melbourne AU we have a popular arcade chain that uses the EMBED card system with Mifare ULTRALIGHT cards for game cards. I did some checks with the small pool of cards I have and found the last 2 octets are not changing from 0x00.

This is a modded file for the built in flipper NFC app that adds the option to create a random game card based off my findings. In theory it should work but who knows. I dont think it checks data stored on the card.

DO NOT USE THIS TO STEAL OTHER PEOPLES MONEY. THIS IS A PROOF OF CONCEPT TO SHOW IN INSECURITY OF ONLY USING THE UID IN A CARD BASED SYSTEM. I HAVE NOT TESTED THIS AT ALL. THE CHANCE OF YOU FINDING A CARD THAT HAS MONEY, IS A STAFF, A TECH OR A VALID CARD IS SUPER LOW. YOU WILL PROBABLY GET SPOTTED BY A STAFF MEMBER IF YOU ARE TRYING THIS. IT IS NOT MY RESPONSIBILITY IF YOU GET CAUGHT, FINED, BANNED, JAILED OR THE EARTH OPENS UP AND SWALLOWS YOU WHOLE.


Instructions:
Copy this file over the top of the original file and build the firmware. tested with unleashed 018

Do not copy to the flipper. nothing will change.
