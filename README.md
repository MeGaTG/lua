# lua-telegram-bot

### Available Variables

```lua
id
```
```lua
username
```
```lua
first_name
```

### Available Functions

```lua
getMe()
```
```lua
getUpdates([offset] [,limit] [,timeout])
```
```lua
sendMessage(chat_id, text [,parse_mode] [,disable_web_page_preview] [,disable_notification] [,reply_to_message_id] [,reply_markup])
```
```lua
forwardMessage(chat_id, from_chat_id [,disable_notification], message_id)
```
```lua
sendPhoto(chat_id, photo [,caption] [,disable_notification] [,reply_to_message_id] [,reply_markup])
```
```lua
sendAudio(chat_id, audio, duration [,performer] [,title] [,disable_notification] [,reply_to_message_id] [,reply_markup])
```
```lua
sendDocument(chat_id, document [,caption] [,disable_notification] [,reply_to_message_id] [,reply_markup])
```
```lua
sendSticker(chat_id, sticker [,disable_notification] [,reply_to_message_id] [,reply_markup])
```
```lua
sendVideo(chat_id, video [,duration] [,caption] [,disable_notification] [,reply_to_message_id] [,reply_markup])
```
```lua
sendVoice(chat_id, voice [,duration] [,disable_notification] [,reply_to_message_id] [,reply_markup])
```
```lua
sendLocation(chat_id, latitude, longitude [,disable_notification] [,reply_to_message_id] [,reply_markup])
```
```lua
sendChatAction(chat_id, action)
```
```lua
getUserProfilePhotos(user_id [,offset] [,limit])
```
```lua
getFile(file_id)
```

#### Inline Mode functions

```lua
answerInlineQuery(inline_query_id, results [,cache_time] [,is_personal] [,next_offset])
```

```lua
answerCallbackQuery(callback_query_id, text [, show_alert])
```

#### Bot API 2.0 functions

```lua
kickChatMember(chat_id, user_id)
```

```lua
unbanChatMember(chat_id, user_id)
```

```lua
editMessageText(chat_id, message_id, inline_message_id, text [, parse_mode] [, disable_web_page_preview] [, reply_markup])
```

```lua
editMessageCaption(chat_id, message_id, inline_message_id, caption [, reply_markup])
```

```lua
editMessageReplyMarkup(chat_id, message_id, inline_message_id [, reply_markup])
```

#### Bot API 2.1 functions

```lua
getChat(chat_id)
```

```lua
leaveChat(chat_id)
```

```lua
getChatAdministrators(chat_id)
```

```lua
getChatMembersCount(chat_id)
```

```lua
getChatMember(chat_id, user_id)
```

### Helper functions:


```lua
downloadFile(file_id [,download_path])
```
- Downloads file from Telegram Servers.
- `download_path` is an optional path where the file can be saved. If not specified, it will be saved in `/downloads/<filenameByTelegram>`. In both cases make sure the path already exists, since LUA can not create folders without additional modules.

```lua
generateReplyKeyboardMarkup(keyboard [,resize_keyboard] [,one_time_keyboard] [,selective])
```
- Generates a `ReplyKeyboardMarkup` of type `reply_markup` which can be sent optionally in other functions such as `sendMessage()`.
- Displays the custom `keyboard` on the receivers device.

```lua
generateReplyKeyboardHide([hide_keyboard] [,selective])
```
- Generates a `ReplyKeyboardHide` of type `reply_markup` which can be sent optionally in other functions such as `sendMessage()`.
- Forces to hide the custom `keyboard` on the receivers device.
- `hide_keyboard` can be left out, as it is always `true`.

```lua
generateForceReply([force_reply] [,selective])
```
- Generates a `ForceReply` of type `reply_markup` which can be sent optionally in other functions such as `sendMessage()`.
- Forces to reply to the corresponding message from the receivers device.
- `force_reply` can be left out, as it is always `true`.

## Library Extension

The Library extension was added to help developers focus on the things that actually matter in a bot: It's logic.
It offers serveral callback functions which can be overridden to provide the wanted logic.

### Available Functions

To use the extension, simply add another table variable to the initial `require` call like so:

```lua
local bot, extension = require("lua-bot-api").configure(token)
```

The `extension` Table now stores the following functions:

```lua
run()
```
- Provides an update handler which automatically fetches new updates from the server and calls the respective callback functions.

```lua
onUpdateReceive(update)
```
- Is called every time an update, no matter of what type, is received.

```lua
onMessageReceive(message)
```
- Is called every time a text message is received.

```lua
onPhotoReceive(message)
```
- Is called every time a photo is received.

```lua
onAudioReceive(message)
```
- Is called every time audio is received.

```lua
onDocumentReceive(message)
```
- Is called every time a document is received.

```lua
onStickerReceive(message)
```
- Is called every time a sticker is received.

```lua
onVideoReceive(message)
```
- Is called every time a video is received.

```lua
onVoiceReceive(message)
```
- Is called every time a voice message is received.

```lua
onContactReceive(message)
```
- Is called every time a contact is received.

```lua
onLocationReceive(message)
```
- Is called every time a location is received.

```lua
onLeftChatParticipant(message)
```
- Is called every time a member or the bot itself leaves the chat.

```lua
onNewChatParticipant(message)
```
- Is called when a member joins a chat or the bot itself is added.

```lua
onNewChatTitle(message)
```
- Is called every time the chat title is changed.

```lua
onNewChatPhoto(message)
```
- Is called every time the chat photo is changed.

```lua
onDeleteChatPhoto(message)
```
- Is called every time the chat photo is deleted.

```lua
onGroupChatCreated(message)
```
- Is called every time a group chat is created directly with the bot.

```lua
onSupergroupChatCreated(message)
```

```lua
onChannelChatCreated(message)
```

```lua
onMigrateToChatId(message)
```
- Is called every time a group is upgraded to a supergroup.

```lua
onMigrateFromChatId(message)
```

```lua
onInlineQueryReceive(inlineQuery)
```
- Is called every time an inline query is received.

```lua
onChosenInlineQueryReceive(chosenInlineQuery)
```
- Is called every time a chosen inline query result is received.

```lua
onUnknownTypeReceive(unknownType)
```
- Is called every time when an unknown type is received.

### Using extension functions

In order to provide your own desired behaviour to these callback functions, you need to override them, like so, for example:

```lua
local bot, extension = require("lua-bot-api").configure(token)

extension.onMessageReceive = function (message)
	-- Your own desired behaviour here
end

extension.run(limit, timeout)

```

You can now use `extension.run()` to use the internal update handler to fetch new updates from the server and call the representive functions.
It lets you pass the same `limit` and `timeout` parameters as in `getUpdates()` to control the handlers behaviour without rewriting it.

You can even override `extension.run()` with your own update handler.

See bot-example.lua for some examples on how to use extension functions.
