# Setting the bot up

### Overview:

* Getting a Discord and Telegram bot token
* Cloning it to your local machine/server
* Installing Node dependencies (and maybe Node itself)
* Configuration
  * Getting a Discord Webhook plus a channel ID
  * Getting a Telegram group ID
* Running it
* Additional "tweaks"
* ~~Profit?~~

## Getting the bot tokens

To create a Discord bot, head over to the [Discord developer portal ](https://discord.com/developers/applications) and create yourself an application. (Make sure to "convert" it to a Bot account!)

To create a Telegram bot, head over to [@BotFather](https://t.me/BotFather), type /newbot and follow the instruction it gives you.

## Make sure to save your tokens and the Webhook URL somewhere for the time being!

## Cloning the repo

Install git:
* \# `apt install git` (for Debian based distros)
* \# `pacman -S git` (for Arch based distros)
* Download the installer binary for Windows from [here](https://git-scm.com/download/win)
* ...

Open up your terminal/CMD and cd to wehre you'd like the bot to be stored and clone the repo:
`git clone https://github.com/JaxTheWolf/tg-dc-link,git`.
After it's done, cd into the directory: `cd tg-dc-link`.

## Installing Node and the dependencies

Install Node and npm:
* `curl -sL https://deb.nodesource.com/setup_14.x | sudo -E bash -
sudo apt-get install -y nodejs` (for Debian based distros)
* \# `pacman -S nodejs npm` (for Arch based distros)
* Download the installer binary for Windows from [here](https://nodejs.org/en/download/)
* ...

##### *\*\# in front of the commands means that they need to be ran as root.*

When in the bot's folder, run `npm i`. That will install all the required dependencies needed to run the bot.


## Configuring it

In a Discord server, right-click the channel you wish the bot to sync to. Then click "Copy ID". Store it somewhere for the time being. Then, right-click the channel again and then click "Edit Channel". Then go to the Webhooks section and click "Create Webhook". Name it as you wish (the name doesn't really matter). Copy the Webhook URL.

This is how it looks like:
`https://discordapp.com/api/Webhooks/722802138262274078/a3t3PC7mrUQ592X9G9f6-E5nzydmZA5gXXi--ziGXJ4brhsXIRCUZ8sl3L5XSI3pqBsi`.

The first "number" in the URL is the Webhook ID, and the second part is the secret. Save both for now. 

Locate the conf_example.json file and rename it to conf.json. Open it in your favourite text editor and fill in your Discord and Telegram bot tokens plus the Webhook secret and token like this(make sure to remove the "// Telegram group id (-somenumber)" part, else it won't work!): 

```json
{
    "tg_token": "your telegram bot token",
    "dc_token": "your discord bot token",
    "dc_chann_id": "discord channel ID where messages will be sent",
    "tg_group_id": -7878, // Telegram group id (-somenumber)
    "discord_webhook": {
        "id": "Webhook ID",
        "secret": "Webhook secret/token"
    }
}
```

## Running it

After you're done, try running the bot with `node src/index.js`.

## Additional "tweaks"

If you're using a Linux server which uses systemd as the service manager, then you can add a simple unit file to automatically start the bot whenever your server starts/reboots. (You can also set it to automatically restart, etc, etc. [Here](https://www.freedesktop.org/wiki/Software/systemd/) are the systemd docs.)

Sample unit:
```ini
[Unit]
Description=Starts tg-dc-link
After=network.target

[Service]
Type=simple
User=$YOUR_USER
WorkingDirectory=$WHERE_YOU_INSTALLED_tg-dc-link_TO
ExecStart=$NODE_EXEC_PATH $WHERE_YOU_INSTALLED_tg-dc-link_TO/src/index.js
Restart=always

[Install]
WantedBy=multi-user.target
```
(make sure to replace everything starting with "$")

##### Tip: To get *node's* executable path, run `which node`.

Save this file in `/etc/systemd/system/tg-dc-link.service`. Then run `# systemctl daemon reload` so systemd acknowledges the new unit. To start the bot, run `# systemctl start tg-dc-link`. To make it start on server bootup, run `# systemctl enable tg-dc-link` (this won't start the bot. To also start it, add the --now flag.)
## **Enjoy the bot!**