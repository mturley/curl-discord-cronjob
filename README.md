# curl-discord-cronjob
Send the output of a cron job to a Discord Webhook, via cURL.

## Setup
1. Clone this repo on a server somewhere.
2. Edit the `send-message` script so it contains a real Discord Webhook URL.
3. `sudo crontab -e`, and add a usage of `send-message` to your crontab.

`send-message` takes two command line arguments: a username, and a message. The username can be arbitrary,
this is just the name the user-facing "bot" will use when posting.

If you want a quick and easy way to schedule a disk usage report, just use the `disk-usage-report` script.
Otherwise, you can use that script as a usage example for `send-message`:

```
message=`df -h ~`
$DIRNAME/send-message "Disk Usage Report" "\`\`\`$message\`\`\`"
```
