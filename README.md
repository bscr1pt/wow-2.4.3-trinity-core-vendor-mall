### Only for World Of Warcraft 2.4.3 Trinity Core Emu

#### Short Story

I created my own private server to have fun with my friend(s) and i realized to add every item is too tiring, so i tried to search a 'Quick & Dirty' solution for this incompletion, but i did not find any SQLs for 2.4.3 Trinity Core Emu just for 3.3.5. :( Sad.
Hmm but i was so curious about the vendor creating and after a few hour i got a bit information from different kind of forums so i decide it to create my own Vendor Mall, if it won't be perfect it's fine...because you know it's first time :) when i do something like that.

I spent a few hours with the reading of Trinity Core Database documentation [here](https://trinitycore.atlassian.net/wiki/spaces/tc/pages/2130075/Databases) .
There are a lot of interesting information about tables, columns which is sometimes help you to find out the relations between the tables e.g: Check foreign keys/primary keys, the name of the id column etc...A few table/column documentation may empty but it was not a big problem because i had a working database where i can find the solution for my problem, okay it is a bit harder than reading...But i enjoyed it!

After when i finally extract that piece of information from the table columns documentation, i started to write SQL queries, i got every kinda armor and i saved into separated files, these you can see in the [item_ids](item_ids) directory and i did the same thing with the other items e.g: gems, enchants, mounts, etc... The workaround was the same in every kind of items only the query condition(s) was different.

It was a lot of id so i made a little Python script to generate those INSERT sqls which you see below in the next section. It can do with any other language but this is my favourite.

There is another folder called [other-sqls-from-queries](other-sqls-from-queries) so these are the results of my little Python script.

Few hours of effort and i successfully created my own Vendor Mall the last thing that i had to do is to add npcs somewhere in-game.

A lot of people search for similar SQLs i think, so i decided to share it.

Good Luck & Have Fun!
bscr1pt

#### Add NPC Vendor manual steps

- Start MySQL Server ;)

NPCENTRY - Npc id, the number you are going to use when your going to spawn the npc.
DISPLAYID - The model of the npc, e.g: Neutral Draenei npc display id is 18487, so it means the morph or the skin of the npc in the game.
NAME - The npc/vendors name.
SUBNAME - The subname of the npc/vendor (will be below the name (guildname on characters)

- `INSERT INTO creature_template VALUES ('NPCENTRY', '0', '0', '0', '0', '0', 'DISPLAYID', '0', '0', '0', 'NAME', 'SUBNAME', '', '0', '60', '60', '0', '35', '35', '128', '1', '1.4286', '1', '0', '100', '100', '0', '0', '1', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '7', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '', '0', '3', '1', '1', '1', '0', '0', '0', '0', '0', '0', '0', '0', '1', '0', '0', '0', '', '0');`

- Execute that INSERT on the world database, this will 'create' a new creature, okay next step will be we add items to this vendor
- With these SQL insertions you can add items to that:
  ITEMID - Item you want the vendor to sell (ID) check wowhead.
  NPCENTRY - The ID of the NPC you created before.
- ```INSERT INTO npc_vendor VALUES ('NPCENTRY','0','ITEMID',0,'0',0);
  INSERT INTO npc_vendor VALUES ('NPCENTRY','0','ITEMID',0,'0',0);
  INSERT INTO npc_vendor VALUES ('NPCENTRY','0','ITEMID',0,'0',0);
  INSERT INTO npc_vendor VALUES ('NPCENTRY','0','ITEMID',0,'0',0);
  INSERT INTO npc_vendor VALUES ('NPCENTRY','0','ITEMID',0,'0',0);
  ```
- That's all :) you have a new NPC in the creature_template table which mean you can add it in-game with `.npc add NPCENTRY` command and finally you can buy items.

#### Add NPC 'Vendor Mall' Beta v1

**These SQLs is the first version so it's possible that you'll find an item or items which is not in the right vendor, it is rare but possible.**
Scripts are customizable! Change anything you want e.g: DISPLAYID, NAME, SUBNAME, add more items to vendor, **Take care of vendor item pages cannot go over 12-15 pages, im not sure about how many, but if you add more items than the vendor could 'serve' at the moment when you click on the vendor in-game the game crash with WOW Error, so i'm not reccommend this**

1. Start MySQL Server ;)
2. Execute the **mall_vendors_creature_template.sql** to add new NPCs into the database creature_template table's the entry id range is 40001-40032 (Except 40025, accidentally i skipped this :P )
3. If it suceeded, check once more to be sure about it with a simple query, execute this on world database: `SELECT * FROM creature_template WHERE entry >=40001 AND entry <= 40032;`
4. Execute **mall_npc_vendors.sql** to add items to newly created NPCs.
5. Check this once more too with a simple query: `SELECT * FROM npc_vendor where entry >=40001 AND entry <= 40032;`
6. Last step is to spawn these NPCs somewhere you want with `.npc add ID` command within the NPC entry id range what i mentioned in the second step.

##### Repository is not include the Python script.
