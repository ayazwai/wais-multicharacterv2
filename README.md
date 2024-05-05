# WAIS-MULTICHRACTERV2
- #### Author: Ayazwai#3900 - Ayazwai
- #### Linkedin: [Ayaz Ekrem](https://www.linkedin.com/in/ayaz-ekrem-770305212/)
- #### Instagram: [Ayaz Ekrem](https://www.instagram.com/ayaz.ekremm/)
- ### Sold exclusively at [0resmon](0resmon.tebex.io) tebex.

# How to set it up

- Do not change the name of the script
- Put the script in the resources folder, do not use any folderisation.
- Prefer to start manually in server.cfg as follows.
- Take care to start the script manually after your core.

```
ensure [yourcore]
ensure wais-multicharacterv2
```

- Read the `wais_multicharactersv2` and `wais_multicharactersv2_payment` table into SQL

- ### Framework Settings
- - You can change the framework variable to `esx` or `qbcore` depending on the type you use

- - In the ResourceName section, you should write the file name of the framework. Usually these are [es_extended, qb-core, nc-core blah blah blah]

- - If your infrastructure is new, the `NewCore` variable must be `@true`, if not, it must be `@false`. ESX Legacy and QBCore are newcore 

- - If you are using SharedEvent old core, you can find and paste the shared event from any script or from the infrastructure itself. This is important.


- ### Important for QBCore servers!
- - For old QBCore users, qb-core/server/player.lua file should be opened.
- - `function QBCore.Player.Login(source, citizenid, newData)` Function must be available.
- - `function QBCore.Player.Login(source, citizenid, newData)` Function should be found. `else` part of `if citizenid then` query should be opened.
- - the function `QBCore.Player.CheckPlayerData(src, newData)` should be made as follows.

```
    local required = QBCore.Player.CheckPlayerData(src, newData)
    TriggerEvent('wais:sendNewCharacterData', required.source, required.cid, required.citizenid)
```
![oldseems](https://imgur.com/NakKIwJ.png)

- - After this section is made as above, the following path should be followed.
- - `function QBCore.Player.CheckPlayerData(source, PlayerData)` The specified function must be found and go to the last line of this function. 
- - Then the following codes should be pasted.

```
    return {
        source = src,
        cid = PlayerData.cid,
        citizenid = PlayerData.citizenid
    }
```
![oldseems2](https://imgur.com/UHPBUUR.png)

- - For New QBCore users, qb-core/server/player.lua file should be opened.
- - `function QBCore.Player.Login(source, citizenid, newData)` Function must be available.
- - `function QBCore.Player.Login(source, citizenid, newData)` Function should be found. `else` part of `if citizenid then` query should be opened.
- - the function `QBCore.Player.CheckPlayerData(src, newData)` should be made as follows.

```
    local required = QBCore.Player.CheckPlayerData(source, newData)
    TriggerEvent('wais:sendNewCharacterData', required.source, required.cid, required.citizenid)
```
![newseems](https://i.imgur.com/YbqDztO.png)

- - After this section is made as above, the following path should be followed.
- - `function QBCore.Player.CreatePlayer(PlayerData, Offline)` The specified function must be found and go to the last line of this function. 
- - Then the following codes should be pasted.

```
    return Offline and true or {
        source = src,
        cid = PlayerData.cid,
        citizenid = PlayerData.citizenid
    }
```
![newseems2](https://i.imgur.com/YUr9KC7.png)

# How do I transfer old characters / What is TransferData?

- It is used to transfer your old character data to the new multicaracter script. Set the value of Config.TransferData variable to `@true` then restart the script or restart the server. When you see that all characters have been transferred, you can restart the server or script again by setting `@false`. 

# How do I open more slots / How do I set the Default Slot Count?

- Find the Config.DefaultSlotValue variable in the Config file and write the number of slots that should be open by default. 
- Max: 5

# How do I open or close a slot with command / Is there a Slot Command?

- To open any slot for a person who is or is not on the server, follow these steps.

## Slot open command
- - /open playerid or player license slotnumber
- - /open license:42132141515 5 => This command will open slot 5 for this licence holder.
- - /open 1 5 => This command will open slot 5 for this playerid.

## Slot close command
- - /close playerid or player license slotnumber
- - /close license:42132141515 5 => This command will close slot 5 for this licence holder.
- - /close 1 5 => This command will close slot 5 for this playerid.

# Discord authenticator / Activating Slot with Discord Role

- First of all, we need to create a bot for our server on the [Discord Developer Portal](https://discord.com/developers/applications). 
- This video will be an example for you for bot installation. [Creating a Discord Bot](https://www.youtube.com/watch?v=wGK3b7QDF24).
- After getting the bot on the server, go to the `Config_sv.lua` file
- Enter the ID of your server as the value in the `ConfigSV.GuildID` Variable.
- After entering the GuildID, copy the token of the bot you created. Then paste it into the variable `ConfigSV.BotToken`
- After these operations, create 5 different roles for the Multicaracter Slot on your server. Or create 2-3 or maybe 1 different role according to your own wishes. Your choice.
- Copy the Rolids of the roles you created and find the `Config.SlotRoles` variable in the Config file and replace the data of the variables from 1 to 5 with the Rolids you copied.

Example:
```
Config.SlotRoles = { 
    -- [@number] = "@string" 
    -- [slotNumber] = 'Slot Discord Role ID'
    
    [1] = 'YOUR ROLE ID',
    [2] = 'YOUR ROLE ID',
    [3] = 'YOUR ROLE ID',
    [4] = 'YOUR ROLE ID',
    [5] = 'YOUR ROLE ID',
}
```

# Tebex Settings

If you want to activate the Tebex sale, you need to apply what I said.

## Connecting Tebex to the server

- Create a tebex account, login if you have
- After logging in to your account, let's go to the page called "Game Servers" from the left side of the page.
- Click the "CONNECT GAME SERVER" button at the top right of the page.
- Let's choose the place named "PLUGIN" from the page that comes up and continue.
- Then let's choose your server name and in which packages these sales will be active, but "SELECT ALL" will help you.
- After continuing, it will give you an sv_tebexSecret key on the page that opens in front of you, let's copy it and paste it anywhere in server.cfg, but let's make sure that this code is on the top lines. Then let's start the server.
- When you start the server, the text "NOT CONNECT" on the top right of the page will change to "CONNECT" after a few seconds. After seeing this article, press the continue button and now there is a communication system between your server and tebex.

## Creating a Package Sending News to the Server

We have connected tebex with our server above, and now we will transfer the sales from tebex to the server instantly and make a system where the player can claim it.

- We click on the "PACKAGES" section from the left side and we are directed to the page.
- Then we click on the "ADD NEW" button on the top right of the page and choose the package option.
- On the page that opens before us, we enter the title of the product. This is very important because we will also use the product title you entered in the script, so be careful to write neat and plain text without emojis.
- After entering the other necessary information of the product, "Configure what your customers should receive upon purchasing this package".We select the "+GAME SERVER COMMANDS" option from the section.
- From the drop-down section, we choose the server name that we just created in the above steps. Then click the "ADD COMMAND" button at the bottom.
- After clicking the button, we paste the following code in the "Enter the command to execute on your server" section. This code: ```tebex:buypackage {"tid": "{transaction}",  "mail": "{email}", "pname": "{packageName}", "price": "{price}"}```
- After pasting the code, click the settings icon on the right. From this section, we select our server in the "Game server to Execute On" section. From the "Require Player To Be Online" section, we select the "Execute the command even if the player is offline" option and create our package.
- Copy the product name you created. Find the TebexPack table and create a new table in it as follows: 

```
Config.TebexPackages = { 
    -- Names of the packages created in Tebex. In order to find the purchased slot
    -- ["@string"] = @number
    -- ["Tebex Package Name"] = Number of the slot to be opened
    -- ["Make Sure You Spell The Package Name You Create Correctly!"] = Slot number to be opened after purchase,

    ["Multicharacter - Slot 1"] = 1,
    ["Multicharacter - Slot 2"] = 2,
    ["Multicharacter - Slot 3"] = 3,
    ["Multicharacter - Slot 4"] = 4,
    ["Multicharacter - Slot 5"] = 5,
}
```

- Now when a product is sold, it will be forwarded to your server.

## How Do I Change the Default Spawn Point / I Want the Character to Spawn Where I Want When First Created

- Go to the config file and find the variable `Config.DefaultSpawnCoords`. Copy and paste the `vector3` coordinate where you want the character to be spawned on the first record as the value

## How do I access the character menu again / How do I activate the Logout & Char commands?

- You can change the command in the `Config.LogoutCommand` table in the config file or set it to be used only by administrators.

```
Config.LogoutCommand = {
    ['command'] = "Where to type the command you want to change",
    ['onlyadmins'] = Type `true` if you want administrators to use it and `false` if you want all users to use it 
}

```

## I want to block UIs like hud or something like that when multicaracter is opened / Switch different UIs on and off

- There are some functions that work when the multicaracter is switched on or off. You can access them from the Config file.
- You can check the content below for example uses

```
Config.OpenUIs = function()
    -- You can reopen closed UIs from here with export or trigger again
   TriggerEvent('wais:hideHud', false)
end

Config.CloseUIs = function()
    -- You can close UIs that you do not want to appear on the screen with export or trigger here
    TriggerEvent('wais:hideHud', true)
end
```

## How do I trigger the Spawnselector / How do I turn on the Spawnselector

- First of all, I would like to state that in esx versions, if your spawnselector script is manually opened with a trigger, that is, if it is not automatic, change the function below in the config file with your own event.

FOR ESX:
```
Config.SpawnSelector = function()
    -- ONLY USE FOR ESX FRAMEWORKS
    -- You can spawn the character selector with export or trigger here
    -- if you put the export or trigger please change return value to true

    TriggerEvent('wais:openspawnselector')
    return true
end
```

- In QBCore servers, spawnselectors generally work automatically via qb-apartment, but if you need to open it with a manual trigger, you need to find the function below in the Config file and edit it according to yourself. Here is an example

FOR QBCORE:
```
Config.CharacterSelected = function(charid)
    -- When a person selects a character and presses the play button, the selected character comes as citizenid or char1...5
    TriggerEvent('wais:openspawnselector')
end
```

## How do I change the starting items / How do I give items to new person?

- For this you need to go to the Config_Sv.lua file. Then, if you are using ESX, you should use the place specified below.

FOR ESX:
```
ConfigSV.StarterItems = Config.Framework.Framework == "esx" and {
    -- This is the place where the startup items specified for the ESX Framework will be written.
    {item = "itemname",amount = 1},
    {item = "itemname",amount = 1},
    {item = "itemname",amount = 1},
} or wFramework.Shared.StarterItems ~= nil and wFramework.Shared.StarterItems or {}
```

- For the QBCore framework, if it is a new framework, usually the startup items are located in the qb-core/shared/main.lua file. This script automatically pulls them from there. But if you are using old QBCOre, you can make changes in the following place

```
ConfigSV.StarterItems = Config.Framework.Framework == "esx" and {
    -- This is the place where the startup items specified for the ESX Framework will be written.
} or wFramework.Shared.StarterItems ~= nil and wFramework.Shared.StarterItems or {
    -- This is the place where the startup items specified for the QBCore Framework will be written.
    
    {item = "water", amount = 1},
    {item = "bread", amount = 1},
}
```

## How do I set it to my language? / Is there a Language file?

- Yes, there is a file and a table. You can organise it according to your language.
- Go to `wais-multicharacterv2/html/public/Config.json` file and edit the data in `languages` variable according to yourself.

## Where can I change the text of content such as Blacklist words, Countries and Months / How can I add new ones?

- You can make such content by opening the following file. `wais-multicharacterv2/html/public/Config.json`

## How do I change the colours to my own server's theme / Can I change the colours?

- Yes, you can change the colours as you hope to see them. Posts, icons, etc. Visit this file for the main content. `wais-multicharacterv2/html/public/css/app.css`
- Find the contents of `:root` in the first few lines. Set the variables in it; HEX or RGBA codes according to your own colour code without disturbing the opacity settings too much.
- Set the colours of the drawings that appear on the main screen again from the Config.json file. `wais-multicharacterv2/html/public/Config.json`
