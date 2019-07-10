![minerstat logo](https://cdn.rawgit.com/minerstat/minerstat-asic/master/docs/logo_full.svg)

# minerstat ASIC Hub (Non-SSH)

## What is this?
Monitoring and management software - ASIC Hub (Non-SSH) makes possible to monitor your ASICs without SSH connection established.

**Supported ASICs:**
* Bitmain's Antminer ASICs
* Dayun's ASICs
* Innosilicon's ASICs
* Obelisk's ASICs
* Spondoolies's ASICs
* StrongU's ASICs

Including the latest ones (not full list):
* [Antminer B7](https://minerstat.com/hardware/antminer-b7)
* [Antminer DR5](https://minerstat.com/hardware/antminer-dr5)
* [Antminer E3](https://minerstat.com/hardware/antminer-e3)
* [Antminer S15](https://minerstat.com/hardware/antminer-s15)
* [Antminer S17](https://minerstat.com/hardware/antminer-s17)
* [Antminer S17 Pro](https://minerstat.com/hardware/antminer-s17-pro)
* [Antminer T15](https://minerstat.com/hardware/antminer-t15)
* [Antminer T17](https://minerstat.com/hardware/antminer-t17)
* [Antminer Z9](https://minerstat.com/hardware/antminer-z9)
* [Antminer Z11](https://minerstat.com/hardware/antminer-z11)
* [Obelisk DCR1](https://minerstat.com/hardware/obelisk-dcr1)
* [Obelisk DCR1 Slim](https://minerstat.com/hardware/obelisk-dcr1-slim)
* [Obelisk GRN1](https://minerstat.com/hardware/obelisk-grn1)
* [Obelisk GRN1 Immersion](https://minerstat.com/hardware/obelisk-immersion)
* [Obelisk GRN1 Mini](https://minerstat.com/hardware/obelisk-grn1-mini)
* [Obelisk SC1](https://minerstat.com/hardware/obelisk-sc1)
* [Obelisk SC1 Dual](https://minerstat.com/hardware/obelisk-sc1-dual)
* [Obelisk SC1 Immersion](https://minerstat.com/hardware/obelisk-sc1-immersion)
* [Obelisk SC1 Slim](https://minerstat.com/hardware/obelisk-sc1-slim)
* [Dayun Zig D1](https://minerstat.com/hardware/dayun-zig-d1)
* [Dayun Zig M1](https://minerstat.com/hardware/dayun-zig-m1)
* [Dayun Zig Z1](https://minerstat.com/hardware/dayun-zig-z1)
* [Dayun Zig Z1+](https://minerstat.com/hardware/dayun-zig-z1-plus)
* [Innosilicon A9 - ZMaster](https://minerstat.com/hardware/innosilicon-a9-zmaster)
* [Innosilicon D9 - DecredMaster](https://minerstat.com/hardware/innosilicon-d9-decredmaster)
* [Innosilicon S11 - SiaMaster](https://minerstat.com/hardware/innosilicon-s11-siamaster)
* [Spondoolies SPx36 profitability](https://minerstat.com/hardware/spondoolies-spx36)

Work in progress for more ASIC support.

## Features

* Sync with minerstat dashboard
Hashrate Data, Temps Data, Chains Data

* Full Control
Reboot, Restart, Full Config Edit

* Config Edit
On first sync hub upload your actual cgminer, bmminer config to minerstat Config Editor:
You are able to edit **pool, frequency, voltage** from **online** (in bulk too)!

* Profit Switch
Configure and mine the always the most profitable coin with your ASIC! 
Between coin changes with Non-SSH Hub no machine reboot needed.

## Installation on Linux

### Install dependencies

``` sh
$ sudo apt-get update
$ sudo apt-get install wget curl jq screen
```

### Download and Setup

**NOTE for Raspberry-Pi:** Use hub-pi package instead of hub-linux

Login to your server over SSH OR Open Terminal and execute the following command(s):

``` sh
$ wget https://github.com/minerstat/minerstat-asic-hub-non-ssh/releases/download/latest/hub-linux && chmod 777 hub-linux
$ sudo cp -rf hub-linux /usr/bin
$ hub-linux --help
```

You need to see this response:

```
================ © minerstat OÜ in 2019 ================
-t|--token   : Website Login Key
-g|--group   : Group/Location
-l|--limit   : How many request allowed at once
-d|--debug   : Show Detailed Debug Output [ 0 | 1 ]
-v|--version : Print current version, build number
-h|--help    : Print this help menu
```

You can start one or multiple instances to monitor one or more groups at once.

This command is for testing, for production usage use crontab solution.
``` sh
$ hub-linux --token YOURACCESSKEY --group GROUPTOMONITOR --limit 32 --debug 1
```

### How to start Asic Hub with the system?

You'll need to edit crontabs. Here is an example:

``` sh
$ crontab -e

Select an editor.  To change later, run 'select-editor'.
  1. /bin/ed
  2. /bin/nano        <---- easiest
  3. /usr/bin/vim.tiny

Choose 1-3 [2]: 2

```

Edit the file with your start line e.g:

(Not required to use separate groups for hub, but more easily to maintain and monitor multiple locations)

``` sh
* * * * * screen -A -m -d -S hub-s17 hub-linux --token 4cc355k3y --group s17 --limit 128 --exit 1
* * * * * screen -A -m -d -S hub-s15 hub-linux --token 4cc355k3y --group s15 --limit 128 --exit 1
* * * * * screen -A -m -d -S hub-t15 hub-linux --token 4cc355k3y --group t15 --limit 128 --exit 1
```

CTRL + O -> SAVE

CTRL + C -> CLOSE

### Importance of ulimit

#### What is ulimit?
Ulimit is the number of open file descriptors per process. Sometimes, you will get the error message like “too many files open “. This happens because you have reached the limit of the number opened files, so you need to increase the value of ulimit parameter.

#### How to increase ?

``` sh
$ sudo nano /etc/security/limits.conf
```

Edit this file to the following:

``` sh
*   soft    nofile    65535
*   hard    nofile    65535
```

Modify /etc/systemd/user.conf

``` sh
DefaultLimitNOFILE=65535
```

CTRL + O -> SAVE

CTRL + C -> CLOSE

After a reboot

``` sh
$ ulimit -n
```

Should say 65535

## Installation on macOS

### Install dependencies

``` sh
$ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
$ brew install wget curl jq
```

### Download and Setup

Login to your server over SSH OR Open Terminal and execute the following command(s):

``` sh
$ wget https://github.com/minerstat/minerstat-asic-hub-non-ssh/releases/download/latest/hub-mac && chmod 777 hub-mac
$ sudo cp -rf hub-mac /usr/local/bin
$ export EDITOR=nano
$ hub-mac --help
```

You need to see this response:

```
================ © minerstat OÜ in 2019 ================
-t|--token   : Website Login Key
-g|--group   : Group/Location
-l|--limit   : How many request allowed at once
-d|--debug   : Show Detailed Debug Output [ 0 | 1 ]
-v|--version : Print current version, build number
-e|--exit    : Exit after SYNC, for every minute crontab [ 0 | 1 ]
-h|--help    : Print this help menu
```

You can start one or multiple instances to monitor one or more groups at once. (This for test, use crontab for real usage)

``` sh
$ hub-mac --token YOURACCESSKEY --group GROUPTOMONITOR --limit 32 --debug 1
```

### Importance of ulimit

#### What is ulimit?
Ulimit is the number of open file descriptors per process. Sometimes, you will get the error message like “too many files open “. This happens because you have reached the limit of the number opened files, so you need to increase the value of ulimit parameter.

#### How to increase ?

``` sh
$ ulimit -n 4096
```

##

***© minerstat OÜ*** in 2019


***Contact:*** app [ @ ] minerstat.com


***Mail:*** Sepapaja tn 6, Lasnamäe district, Tallinn city, Harju county, 15551, Estonia

##
