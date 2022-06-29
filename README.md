# TelegramSTT

Creates a Telegram bot to transcribe audio messages.

It uses an **Azure Logic App** invoked by the **Telegram BOT webhook** that in turn calls the **Azure Speech services** to transcribe the audio and the bot responds with the transcribed message.

![diagram](https://github.com/UnoSD/TelegramSTT/raw/master/TelegramSTT.png)

## How to create a Telegram bot

From Telegram, contact `@BotFather`

`/newbot`
`<name your bot>`
`<write your unique Telegram handle for the bot>`

Now copy the bot auth key returned.

```bash
pulumi config set azure-native:location "West Europe"
pulumi config set bot-handle "<your bot handle from above>"
pulumi config set bot-key "<your bot key returned by @BotFather>"
pulumi up
# Currently this line won't work as the trigger API invoke args do not take the trigger name, getting URL from the LA in the portal
curl -X POST -H "Content-Type: application/json" -d "{\"url\":\"$(pulumi stack output WorkflowTriggerUrl --show-secrets)\", \"allowed_updates\":[\"message\"]}" "https://api.telegram.org/bot$(pulumi config get bot-key)/setWebhook"
```

# How to use it

Forward any audio message in a conversation to your bot and it will respond with the transcription.
