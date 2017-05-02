![](http://i.imgur.com/C6nTeCS.png)

The easy way to write Telegram bots.

[![Build Status](https://travis-ci.org/mullwar/telebot.svg)](https://travis-ci.org/mullwar/telebot) [![Dependency Status](https://david-dm.org/mullwar/telebot.svg)](https://david-dm.org/mullwar/telebot) ![Node.js Version](http://img.shields.io/node/v/telebot.svg) [![TeleBot Chat](https://img.shields.io/badge/telegram-TeleBot%20Chat-blue.svg)](https://t.me/joinchat/AAAAAEF1Ivern8XS4DKn-A)

**Library features:**

- Simple. Easy to use.
- Full [Telegram Bot API](https://core.telegram.org/bots/API) support.
- Supports [plugins](https://github.com/mullwar/telebot/tree/master/plugins)!
- Build-in modification and event system.
- Extendable and hackable.
- No callbacks, Promises only.
- Readable [changelog](https://github.com/mullwar/telebot/releases).

## Installation

Download and install via [npm package manager](https://www.npmjs.com/package/telebot) (stable):

```
npm install telebot
```

Or clone fresh code directly from git:

```
git clone https://github.com/mullwar/telebot.git
cd telebot
npm install
```

## Usage

Import `telebot` module and create a new bot object:

```js
const TeleBot = require('telebot');

const bot = new TeleBot({
    token: 'TELEGRAM_BOT_TOKEN', // Required. Telegram Bot API token.
    polling: { // Optional. Use polling.
        interval: 1000, // Optional. How often check updates (in ms).
        timeout: 0, // Optional. Update polling timeout (0 - short polling).
        limit: 100, // Optional. Limits the number of updates to be retrieved.
        retryTimeout: 5000, // Optional. Reconnecting timeout (in ms).
        allowedUpdates: [], // Optional. List the types of updates you want your bot to receive. Specify an empty list to receive all updates.
        proxy: 'http://username:password@yourproxy.com:8080' // Optional. An HTTP proxy to be used (supports Basic Auth).
    },
    webhook: { // Optional. Use webhook instead of polling.
        key: '__YOUR_KEY__.pem', // Optional. Private key for server.
        cert: '__YOUR_CERT__.pem', // Optional. Public key.
        url: 'https://....', // HTTPS url to send updates to.
        host: '0.0.0.0', // Webhook server host.
        port: 443 // Server port.
    },
    usePlugins: ['shortReply'], // Optional. Use build-in plugins from /plugins folder.
    pluginConfig: { // Optional. Plugin configuration.
        // myPluginName: {
        //   data: 'my custom value'
        // }
    }
});
```

Or just:

```js
const TeleBot = require('telebot');
const bot = new TeleBot('TELEGRAM_BOT_TOKEN');
```

*Replace `token` value to your [Telegram Bot API](https://core.telegram.org/bots#create-a-new-bot) token key.*

To start polling updates, use ```bot.start()```.

```js
bot.on('text', msg => {
  let fromId = msg.from.id;
  let firstName = msg.from.first_name;
  let reply = msg.message_id;
  return bot.sendMessage(fromId, `Welcome, ${ firstName }!`, { reply });
});

bot.start();
```

This code will send a "welcome" to every users `text` type message as a reply.

***[See more examples!](/examples)***

## Events

Use ```bot.on(<event>, <function>)``` to handle all possible TeleBot events.

To catch a command with arguments, just add a slash:

```js
bot.on('/hello', msg => {
  let [cmdName, firstName, lastName] = msg.text.split(' ');
  return bot.sendMessage(msg.from.id, `Hello, ${ firstName } ${ lastName }!`);
});
```

Also, you can catch multiple events:

```js
bot.on(['/start', '/help', 'sticker'], msg => {
  return bot.sendMessage(msg.from.id, 'Bam!');
});
```

### TeleBot events:

- **/&#42;** – any user command
- **/\<cmd\>** – on specific command
- **start** – bot started
- **stop** – bot stopped
- **reconnecting** – bot reconnecting
- **reconnected** – bot successfully reconnected
- **update** - on update
- **tick** – on bot tick
- **error** – an error occurred
- **inlineQuery** - inline query data
- **inlineChoice** - inline query chosen result
- **callbackQuery** - button callback data

#### Action events:

*keyboard*, *button*, *inlineKeyboard*, *inlineQueryKeyboard*, *inlineButton*, *answerList*, *getMe*, *sendMessage*, *forwardMessage*, *sendPhoto*, *sendAudio*, *sendDocument*, *sendSticker*, *sendVideo*, *sendVoice*, *sendLocation*, *sendVenue*, *sendContact*, *sendChatAction*, *getUserProfilePhotos*, *getFile*, *kickChatMember*, *unbanChatMember*, *answerInlineQuery*, *answerCallbackQuery*, *editMessageText*, *editMessageCaption*, *editMessageReplyMarkup*, *setWebhook*

### Telegram message events:

- **&#42;** - any type of message
- **text** – text message
- **audio** – audio file
- **voice** – voice message
- **document** – document file (any kind)
- **photo** – photo
- **sticker** – sticker
- **video** – video file
- **contact** – contact data
- **location** – location data
- **venue** – venue data
- **edited** – edited message
- **pinnedMessage** – message was pinned
- **userJoined** – new member was added
- **userLeft** – member was removed
- **newTitle** – new chat title
- **newPhoto** – new chat photo
- **deletePhoto** – chat photo was deleted
- **groupCreated** – group has been created
- **channelCreated** – channel has been created
- **supergroupCreated** – supergroup has been created
- **migrateTo** – group has been migrated to a supergroup
- **migrateFrom** – supergroup has been migrated from a group


*Read more about Telegram Bot API response types: https://core.telegram.org/bots/api#available-types*

## Modifiers

You can add modifier to process data before passing it to event.

```js
bot.mod('text', data => {
  let msg = data.message;
  msg.text = `📢 ${ msg.text }`;
  return data;
});
```

This code adds emoji to every `text` message.

### TeleBot modifiers:

- **property** - mod form properties
- **updateList** - list of updates in one tick
- **update** - every update
- **message** - process any type of message
- **\<type\>** - specific type of message (*text, voice, document, photo, sticker, video, contact, location* or *venue*)

## Plugins

Use ```bot.use(require(<plugin_path>))``` to use a plugin.

***[Check out plugins folder!](/plugins)***

## Methods

### TeleBot methods:

##### `on(<events>, <function>)`

Handles events.

##### `event(<event>, <data>)`

Invokes the event handlers.

##### `mod(<name>, <fn>)`

Add data modifier.

##### `modRun(<names>, <data>)`

Run data modifiers.

##### `use(<function>)`

Use module function.

##### `keyboard([array of arrays], {resize, once, selective})`

Creates `ReplyKeyboardMarkup` keyboard `markup` object.

##### `button(<location | contact>, <text>)`

Creates `KeyboardButton` button.

##### `inlineButton(<text>, {url | callback | inline})`

Creates `InlineKeyboardButton` button object.

##### `inlineKeyboard([array of arrays])`

Creates inlineKeyboard object for normal bot messages.

##### `answerList(<inline_query_id>, {nextOffset, cacheTime, personal, pmText, pmParameter})`

Creates `answerInlineQuery` answer list object.

##### `inlineQueryKeyboard([array of arrays])`

Creates inlineKeyboard object for answerList articles.

##### `start()`

Start polling updates.

##### `stop(<message>)`

Stop polling updates.

### Telegram methods:

TeleBot use standard [Telegram Bot API](https://core.telegram.org/bots/api#available-methods) method names.

##### `getMe()`

A simple method for testing your bot's auth token.

##### `answerQuery(<answerList>)`

Use this method to send `answerList` to an inline query.

##### `getFile(<file_id>)`

Use this method to get basic info about a file and prepare it for downloading.

##### `sendMessage(<chat_id>, <text>, {parse, reply, markup, notify, preview})`

Use this method to send text messages.

##### `forwardMessage(<chat_id>, <from_chat_id>, <message_id>, {notify})`

Use this method to forward messages of any kind.

##### `sendPhoto(<chat_id>, <file_id | path | url | buffer | stream>, {caption, fileName, serverDownload, reply, markup, notify})`

Use this method to send photos.

##### `sendAudio(<chat_id>, <file_id | path | url | buffer | stream>, {title, performer, duration, caption, fileName, serverDownload, reply, markup, notify})`

Use this method to send audio files, if you want Telegram clients to display the file as a playable voice message.

##### `sendDocument(<chat_id>, <file_id | path | url | buffer | stream>, {caption, fileName, serverDownload, reply, markup, notify})`

Use this method to send general files.

##### `sendSticker(<chat_id>, <file_id | path | url | buffer | stream>, {fileName, serverDownload, reply, markup, notify})`

Use this method to send `.webp` stickers.

##### `sendVideo(<chat_id>, <file_id | path | url | buffer | stream>, {duration, width, height, caption, fileName, serverDownload, reply, markup, notify})`

Use this method to send video files, Telegram clients support `mp4` videos (other formats may be sent as `Document`).

##### `sendVoice(<chat_id>, <file_id | path | url | buffer | stream>, {duration, caption, fileName, serverDownload, reply, markup, notify})`

Use this method to send audio files, if you want Telegram clients to display the file as a playable voice message.

##### `sendLocation(<chat_id>, [<latitude>, <longitude>], {reply, markup, notify})`

Use this method to send point on the map.

##### `sendVenue(<chat_id>, [<latitude>, <longitude>], <title>, <address>, {foursquare, reply, markup, notify})`

Use this method to send information about a venue.

##### `sendContact(<chat_id>, <number>, <firstName>, <lastName>, { reply, markup, notify})`

Use this method to send phone contacts.

##### `sendAction(<chat_id>, <action>)`

Use this method when you need to tell the user that something is happening on the bot's side.

##### `getUserProfilePhotos` as `getUserPhoto(<user_id>, {offset, limit})`

Use this method to get a list of profile pictures for a user.

##### `getFile(<file_id>)`

Use this method to get basic info about a file and prepare it for downloading.

##### `getChat(<chat_id>)`

Use this method to get up to date information about the chat.

##### `leaveChat(<chat_id>)`

Use this method for your bot to leave a group, supergroup or channel.

##### `getChatAdministrators` as `getAdmins(<chat_id>)`

Use this method to get a list of administrators in a chat.

##### `getChatMembersCount` as `countMembers(<chat_id>)`

Use this method to get the number of members in a chat.

##### `getChatMember` as `getMember(<chat_id>, <user_id>)`

Use this method to get information about a member of a chat.

##### `kickChatMember` as `kick(<chat_id>, <user_id>)`

Use this method to kick a user from a group or a supergroup.

##### `unbanChatMember` as `unban(<chat_id>, <user_id>)`

Use this method to unban a previously kicked user in a supergroup.

##### `editMessageText` as `editText({chatId & messageId | inlineMsgId}, <text>)`

Use this method to edit text messages sent by the bot or via the bot (for inline bots).

##### `editMessageCaption` as `editCaption({chat & message | inline}, <caption>)`

Use this method to edit captions of messages sent by the bot or via the bot (for inline bots).

##### `editMessageReplyMarkup` as `editMarkup({chat & message | inline}, <markup>)`

Use this method to edit only the reply markup of messages sent by the bot or via the bot (for inline bots).

##### `answerCallbackQuery` as `answerCallback(<callback_query_id>, <text>, <show_alert>)`

Use this method to send answers to callback queries sent from inline keyboards.

##### `setWebhook(<url>, <certificate>, <allowed_updates>, <max_connections>)`

Use this method to specify a url and receive incoming updates via an outgoing webhook.

##### `getWebhookInfo()`

Use this method to get current webhook status.

#### `deleteWebhook()`

Use this method to remove webhook integration if you decide to switch back to getUpdates. Returns `True` on success.


## Documentation

Read [wiki on GitHub](https://github.com/mullwar/telebot/wiki).
