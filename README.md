# Geyser-Spigot Server Manager
The Geyser-Spigot Server Manager (or GSSM) is a tool that makes setting up a cross-platform Java/Bedrock [Minecraft](https://www.minecraft.net/en-us) server with [Spigot](https://www.spigotmc.org/), [Geyser](https://geysermc.org/), and [Floodgate](https://github.com/GeyserMC/Floodgate/) super easy.

# Dependencies
 - Linux (GSSM was developed to be used on Linux servers, but may be adaptable to other platforms)
 - Java [[java.com](https://www.java.com/en/download/)/[oracle.com](https://www.oracle.com/java/technologies/downloads/)]
 - Python 3 [[python.org](https://www.python.org/downloads/)]
 - git [[git-scm.com](https://git-scm.com/downloads)]
 - wget [[gnu.org](https://www.gnu.org/software/wget/)]
 - md5sum
 - cat
 - konsole (only if you intend to use the `run.sh` script to run `gssm`) [[konsole.kde.org](https://konsole.kde.org/download.html)]

# Basic Setup
 - Ensure you have the required [dependencies](#Dependencies) to run GSSM
 - Download a compressed archive of this repository [here](https://gitlab.com/nickgirga/gssm/-/archive/main/gssm-main.zip) and decompress the archive _**or**_ clone the repository using `git clone https://gitlab.com/nickgirga/gssm.git`
 - Navigate to the root directory of the repository (with either the desktop interface or a terminal). It should at the very least contain `run.sh`, `gssm`, and `LICENSE`, but the server will populate the directory with much more later
 - Double click on `run.sh` if you are using a desktop interface _**or**_ run gssm via a terminal (`./gssm`, `python3 ./gssm`, or `python ./gssm` depending on your system)
 - The first thing you will want to do is build or update the server. GSSM does not ship with any binaries other than itself, so this is the bare minimum to get the server working. You can do this by typing `S` in GSSM's main menu and pressing enter. GSSM will download the latest successful build of Spigot's [BuildTools](https://www.spigotmc.org/wiki/buildtools/) and run it for you. Just go grab a coffee and wait for it to tell you that it has finished. This can take a while (**WARNING**: see [issue #1](https://gitlab.com/nickgirga/gssm/-/issues/1))
 - Next, you will want to download or update the plugins. Again, GSSM does not ship with any binaries other than itself, so if you want Bedrock compatibility, this is a requirement. If you do not need Bedrock compatibility, you can skip this step. You can update the plugins by typing `P` in GSSM's main menu and pressing enter. GSSM will download the latest successful builds of Geyser and Floodgate, create a `plugins` folder if it does not already exist, and place the plugins inside (**WARNING**: see [issue #1](https://gitlab.com/nickgirga/gssm/-/issues/1))
 - Now that you have the files you need, you can run the server. You can do this by typing `R` in GSSM's main menu and pressing enter. GSSM will execute the first JAR file it finds that starts with `spigot-` and ends with `.jar` in GSSM's root directory (although there is a version selector currently being worked on)
 - This _**WILL**_ "crash" the first time to tell you that you must accept the EULA by changing a `false` value to `true` in a text document and saving it. We cannot agree to these terms for you, so this process cannot be automated. Press `Q` in GSSM's main menu to quit, edit the `eula.txt` file in the root directory of the GSSM repository with a text editor, change `eula=false` to `eula=true`, and save if you have read and agree to the terms that are linked within the text document. Then, open GSSM again and run the server by typing `R` in the main menu and pressing enter
 - At this point, you should have a functioning Minecraft server that is generating a new world. Just wait for the server to say "Done" and you're ready to join or shut it back down for configuration. If it's running on your local machine, you can now open an instance of Minecraft and connect to it by connecting to the IP address `localhost` or `127.0.0.1`. You should even be able to join with mobile devices and consoles if they are connected to the same network that is hosting the server. However, this server is only accessible to you on your local network. And that may suffice, so this is the end of the [basic setup](#Basic Setup) guide. However, if you wish to fix this, you must configure Minecraft's server files
 - Stop the server by typing `stop` and pressing enter. The server will save everything and shut down
 - Proceed to [advanced setup](#Advanced Setup) if you wish to open your server to the public. This would also include if you wish to be able to connect to your server on mobile over cell data

# Advanced Setup
 - Follow all of the [basic setup](#Basic Setup) instructions first
 - Then, edit the `server.properties` file in the root directory of the GSSM repository with a text editor. Now is a good time to change any other settings you might want to, but the important bit is the `server-ip` value. It should be filled with the local IP address that will broadcast the server. For example, if you just want to play with a friend over a [Hamachi](https://vpn.net/) tunnel so you don't need to port forward, put your Hamachi IP there (and in that case, you're already done configuring, so you can save the file and start the server again). If you want to port forward so that people can join via your public IP address, put your private IP address there. You can find this by running `ip addr | grep 192.168.` on Linux. You may have multiple returned depending on how your network adapters are set up in Linux, but the right IP address will likely look like `192.168.1.xxx` or `192.168.0.xxx` depending on how your router is configured. It will also have a network device identifier next to it, but this isn't always human-readable (though it may help rule out IP addresses that aren't the one we want, like VMware's `vmnet1` devices). Place the correct IP under `server-ip` and move on to port forwarding
 - Unfortunately, port forwarding is a process that varies across routers (or service providers if you're paying for a server), but the general idea is to connect to your router's administration panel (or find the server provider's configuration page), find the section that allows us to open and close ports to the public, and add a new rule. You can get to your router's administration panel by navigating to the gateway using a web browser, which is likely the first node in your network (e.g. if my private IP was `192.168.1.124`, my gateway is likely `192.168.1.1`, if my private IP was `192.168.0.43` my gateway is likely `192.168.0.1`). This information may have been written by your internet service provider or network administrator somewhere. You will need to log in using the administrator's credentials. This information may also have been written by your internet service provider or network administrator somewhere. Once logged in and adding the rule, the address/device/host the rule should apply to is our private IP that we found earlier. The port that the rule should apply to is the one found under the `server-port` property in `server.properties` (which is `25565` by default) for just Java player support. To add Bedrock player support we need to add an additional rule using the same address/device/host and the port found under `port` in `./plugins/Geyser-Spigot/config.yml` (which is `19132` by default). This is a good time to configure the Geyser server information, so the server is more presentable to Bedrock players. After making the changes to your router or server configuration, you may need to restart your router or server for the changes to take place.
 - You can (and should) test that the port forward worked in 2 ways: ensure that [canyouseeme.org](https://www.canyouseeme.org/) can see you over the ports you chose and run Minecraft off of a mobile hotspot from your phone and try to connect to your server via its public IP address
 - To find your public IP address, you can [search for it on Google](https://www.google.com/search?q=what+is+my+ip), run a network test on a trusted website such as [Speedtest](https://www.speedtest.net/), or accept a random Xbox Live Party invite. I'm sure they'll be happy to tell you your IP address (this is a joke).
 - This particular setup is not recommended on a home network because of exactly what I just joked about. Giving away the public IP address to your home network isn't the brightest of ideas because people can easily use your IP to find you in real life, they can overwhelm your network with data to the point that it won't function (also known as [DoS and DDoS attacks](https://en.wikipedia.org/wiki/Denial-of-service_attack)), and if your network is poorly configured with insecure devices connected, people can even steal information from your network remotely with this public IP address. If IP address privacy is a concern when hosting servers, you should definitely pay for a VPS such as [Linode](https://www.linode.com/) and host the server from there. The benefit of using these types of servers over services that sell game servers is that we can use these servers for whatever we would like and they're priced competitively. For about the price of a Minecraft-specific server from most providers, I can get a general purpose server and also be able to host an apache web server and a TeamSpeak or Mumble server on it along with the Minecraft server. Plus, because you have direct access to the files, you don't have to hope that the server provider has the plugins or mods that you want on your server. Just add them yourself. This is the intended use case for the Geyser-Spigot Server Manager, but it can still be used however you see fit
 - Even if you choose to use a VPN such as [Hamachi](https://vpn.net/) to create a "local" tunnel between devices, make sure you trust whoever joins the Hamachi networks and do not share the password. Anybody in your Hamachi network can essentially do the same things they could do if they were connected to your local area network (which is exactly why this community uses it for Minecraft servers "over LAN"). Make sure you turn Hamachi off and exit it completely when you are done using it to minimize any windows of opportunity an attacker may have

# Usage
 - Menu (easy)
   - Go to the server directory and double click on `run.sh` if you are using a desktop interface _**or**_ run gssm via a terminal (`./gssm`, `python3 ./gssm`, or `python ./gssm` depending on your system)
   - After setup, all you need to know is `S` means update **S**erver, `P` means update **P**lugins, and `R` means **R**un server
 - Direct Commands (designed mostly for scripting, debugging, and quickly performing tasks without jumping through menus)
   - `./gssm help` will print how to use the direct command interface into the terminal
   - `./gssm update [type]` will update the specified files. `[type]` can be `server` to update the server, `plugins` to update the plugins, or it can be left blank (`./gssm update`) to update both
   - `./gssm run` will start the server. If you need to remove the restart prompt for scripting reasons, you can add `--exit-on-stop` to the end
   - `./gssm history` will show the last times you had updated the server files and the plugins
   - You can add `-v` or `--verbose` to the end of almost anything to get a bit more information about things if something is going wrong. This is extremely useful for debugging

# What Works?
 - Downloading Spigot BuildTools and building the server files (otherwise updating the server)
 - Downloading required plugins for Bedrock players to be able to join (otherwise updating the plugins)
 - Running the server
 - Checking the last time the server files or the plugins were updated

# What is Planned?
 - The ability to choose the version of Minecraft the Spigot server will be compatible with
 - The ability to choose the version of Java to use
 - The ability to choose the Spigot server executable to run (in the case that numerous versions are installed)
 - A full plugin manager that will allow users to view installed plugins, enable and disable them, delete them, back them up, revert to backups, download new plugins from specified sources, and keep all plugins (including the custom ones) up-to-date using the provided download sources
 - A world manager that will allow users to view existing worlds, enable and disable them, delete them, back them up, revert to backups, and rename them
 - A player manager that will allow users to view all players that have joined, edit their inventories, edit their player stats, kick/unkick them, ban/unban them, edit their scoreboard stats, and change their teams
 - A server version manager that will allow users to view all available versions of Spigot server executables and remove them
 - A self-update system for GSSM
 - A download verification system to ensure that update files have not been corrupted or tampered with
 - A config editor that allows users to quickly edit common server and plugin settings
 - A config checking system to prompt the user to fix it if they try to run the server with an invalid config
 - A full graphical user interface

 # Thanks
 I would like to give my thanks to the teams working on [Spigot](https://www.spigotmc.org/), [Geyser](https://geysermc.org/), [Floodgate](https://github.com/GeyserMC/Floodgate/), and [Minecraft](https://www.minecraft.net) itself for making this all possible.
