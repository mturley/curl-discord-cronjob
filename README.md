# curl-discord-cronjob

Send the output of a cron job to a Discord Webhook, via cURL.

![screenshot](https://raw.githubusercontent.com/mturley/curl-discord-cronjob/master/screenshot.png)

## Note: The `example` script here does still report disk usage...

...but I moved the much fancier `disk-usage-report` script over to [stuffware/discord-disk-usage-report](https://github.com/stuffware/discord-disk-usage-report), because I wanted to make it easier to fork and rename this `curl-discord-cronjob` repo multiple times for other cron webhooks, without necessarily having a git submodule dependency in each one (which the new repo introduces to display an ASCII progress bar). Because git submodules are wack, yo, and sort of an unnecessary hassle for you, the person finding this on Google, I figured I'd keep them contained to the specific webhook(s) where I use them, so you can keep it simple.

Anyway, if you head [over yonder](https://github.com/stuffware/discord-disk-usage-report), you'll get a nice handy `setup` script, and you'll get to impress your friends with the following "bot":

![screenshot2](https://raw.githubusercontent.com/mturley/curl-discord-cronjob/master/screenshot2.png)

Isn't that nice? Make sure you read the README there, though. If you want something a little less involved, stay here and read on.

## Dependencies

* bash
* curl
* python (any old version, just for JSON escaping)
* cron, or something like cron, to make it act like a "service".

## Setup

1. Clone (or fork and then clone!) this repo on a server somewhere.
2. Copy, rename and edit the `example` script:
   * Change the `webhook_url` variable to contain your real Discord Webhook URL.
   * Change the `message_username` variable to the nick you want the bot to use.
   * Change the `message_content` variable to whatever you want. Don't let your dreams be dreams.
3. Give the script a test run! You can just `./example` or `./my-renamed-example` from the command line.
4. If your message showed up in Discord, you're almost done. To make the script run automatically, there are a number of ways, but the simplest is to just use `cron`. You have two options for adding a `cron` job:
   * You can configure any custom timing you like with `sudo crontab -e`, which requires you to read the manual a little bit...
   * ...or if you just want the webhook to be called hourly, daily, weekly, or monthly (etc), if your system is running a new enough version of `cron`, you can just symbolically link your script into `/etc/cron.daily/` or `/etc/cron.hourly/`, etc, if they are present on your system:

   ```sh
   sudo ln -s /path/to/curl-discord-cronjob/my-renamed-example /etc/cron.daily/
   ```

   This will schedule a daily disk usage report to be posted to your Discord channel.
   (If you rename `example` to `my-renamed-example`. Which, of course, you should not.)

## About the `send-message` script

`send-message` takes three command line arguments: a Discord Webhook URL, a username/nick for the bot to use, and a message. The username can be arbitrary,
this is just the name the user-facing "bot" will use when posting.

To see `send-message` in action, just take a look at my `example` script! It's very short. Here it is again:

```bash
#!/bin/bash

SCRIPT=`realpath $0`; DIRNAME=`dirname $SCRIPT`

webhook_url='YOUR_DISCORD_WEBHOOK_URL_HERE'
bot_username='Disk Usage Report'
message=`df -h ~`

$DIRNAME/send-message "$webhook_url" "$bot_username" "$message"
```