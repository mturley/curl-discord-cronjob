# curl-discord-cronjob
Send the output of a cron job to a Discord Webhook, via cURL.

![screenshot](https://raw.githubusercontent.com/mturley/curl-discord-cronjob/master/screenshot.png)

## Dependencies
bash, cron, curl, python (any old version, just for JSON escaping)

## Setup
1. Clone this repo on a server somewhere.
2. Edit the `send-message` script so it contains a real Discord Webhook URL.
3. Add a usage of `send-message` to your cron jobs. You can do this with `sudo crontab -e`, or just by symbolically linking a script that runs `send-message` into your `/etc/cron.daily` or `/etc/cron.hourly` if present on your system:
```
sudo ln -s /path/to/curl-discord-cronjob/disk-usage-report /etc/cron.daily
```
This will schedule a daily disk usage report to be posted to your Discord channel.

### NOTE: `disk-usage-report` depends on a git submodule.
The `disk-usage-report` script provided will only work if you initialize the `ProgressBar` submodule, by using these stupid steps that git decided were necessary:
1. Go to the root directory of this repo.
2. `cd ProgressBar`
3. `git submodule init`
4. `git submodule update`
Then you can `cd ..` and run `./disk-usage-report` to make sure that worked!

## About the `send-message` script
`send-message` takes two command line arguments: a username, and a message. The username can be arbitrary,
this is just the name the user-facing "bot" will use when posting.

If you want a quick and easy way to schedule a disk usage report, just use the `disk-usage-report` script as in my example above.
Otherwise, you can use that script as a usage example for `send-message`:

```
message=`df -h ~`
$DIRNAME/send-message "Disk Usage Report" "\`\`\`$message\`\`\`"
```

Note: The escaped triple-backticks around `$message` in this example are what provide the code-block styling in the screenshot above. Those are optional, and any Markdown can be used.

# Update:
The screenshot at the top of this file matches the `df -h ~` example, but that's no longer what `disk-usage-report` does, I made some updates.

Hold my beer, I'm using [git submodules](https://git-scm.com/book/en/v2/Git-Tools-Submodules).

Bringing in https://github.com/mturley/ProgressBar to make the output of `disk-usage-report` a little more fancy...