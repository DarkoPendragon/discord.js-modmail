# discord.js-modmail
***
A very simple mod mail system for users to contact mods via a bots dm (similar to Reddit) that doesn't require a database. Do note this was written while I was half asleep, and some features mentioned are a WIP and don't work. And although this works without a database I'm planning on adding support for sqlite (local) and MongoDB (online). The `depth` option has no effect yet. Messages sent may contain images or files (replies only accept images). 

Here's some images of the logging using method 0:  
![Normal Image](https://media.discordapp.net/attachments/717445703449837568/784088412651126815/Screenshot_9.png "Title") 
![Normal Image](https://media.discordapp.net/attachments/717445703449837568/784088495399501854/Screenshot_10.png "Title")   
The CausticPenguin "bot" is an actual person, the script uses webhooks to send messages to use their avatars and names. Method 1 with only send their name and their message. Method 0 also can have up to 10 images per message, while 1 will only give you links to the files. For either only messages using the reply command will send to users. Talking in the channel will not send messages. Why would you want to use method 1 over 0? Depending on activity method 1 might be faster than 0 and less rate limited. But unless you're an extremely active server you won't have that problem. Or, shouldn't. Probably.  

There's a few options you can pass to customize this a bit.  
| Option | Type | Description | Default |  
| --- | --- | --- | --- |
| id | String | ID of the server to run Mod Mail for | " " |
| categoryID | String | ID of a category that channels will be created to. | " " |
| loggingID | String | Logging channel id (logs replies, messages, channel delete/create) | " " |
| method | Number | 0 = webhook system (faster, less limited), 1 = normal messages (slower, much less fancy) | 0 |
| depth | Number | 0 = no database, 1 = sqlite, 2 = MongoDB | 0 |
| cooldown | Boolean | Limits messages (sent, not replies) to a 5 second cooldown. | false |
| logStaff | Boolean | Logs staff talking in the mod mail channels | false |
| limitedMsg | Boolean | Makes it so users can only send a message to open a channel, and no more | false |
| reasonAlert | Boolean | Adds who closed a channel to the !close command | false |
| prefix | String | Prefix for the commands | "!" |
| help | String | Name for the horrible help command | help |
| reply | String | Name for the reply command | reply |
| close | String | Name for the close command | close |
| errors | String | Name for the errors command | errors |

Setting up is easy. All you have to do (at minimum) is provide a guild id, a category id, a logging channel id, and the script will do the rest. Everything else is optional.  
A very basic example of adding this:  
```js
const Discord = require('discord.js')
const client = new Discord.Client()
require('./mailMod.js')(client, {
  id: "783968289789313065",
  categoryID: "783968359645052939",
  loggingID: "784043168315211807"
})
```

"Staff" are recognized by a simple function:  
```js
function elevation(message) {
    let permlvl = 0;
    if (message.member.permissions.has("KICK_MEMBERS")) permlvl = 1;
    if (message.member.permissions.has("BAN_MEMBERS")) permlvl = 2;
    if (message.member.permissions.has("ADMINISTRATOR")) permlvl = 3;
    if (message.member.id === message.member.guild.ownerID) permlvl = 4;
    return permlvl;
  }
```
You must at least be level 2 (able to ban users) to use the staff commands.

# To Do
***
* Finish cooldown
* Finish limitedMsg
* Multiple prefixes
* Better closing command
* Add database functionality
* More commands (?)
