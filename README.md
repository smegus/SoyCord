# SoyCord
SoyCord Standalone portable encrypted chat, for privacy.

SoyCord is a lightweight encrypted chat client built on top of ntfy topics. It is meant to feel like a small, classic desktop messenger: simple rooms, pinned topics, profile pictures, inline Soymoticons, encrypted uploads, and a normal Windows installer.

It started as an experiment, but has slowly turned into a real little Discord-like chat app.

## What it does

SoyCord lets people join the same chat by using the same:

```text
ntfy server
topic name
encryption key
```

Messages are encrypted before they are sent to ntfy. Other SoyCord clients using the same topic and key can decrypt and show them.

## Features

- Encrypted chat over ntfy topics
- Shared topic plus shared encryption key system
- Standalone C# WinForms desktop app
- Self-contained Windows installer EXE
- Installer can clean old SoyCord program files while keeping saved data
- Pinned topic list
- Unread badges for inactive pinned topics
- Slow background checking for inactive topics
- Local message history/cache
- Welcome room before joining a topic
- Username with set-once lock
- Profile pictures shared through encrypted metadata
- Encrypted image and file uploads through ntfy attachments
- Inline Soymoticons using bundled local PNG files
- Soymoticon autocomplete when typing `*`
- Soymoticon dropdown that inserts at the cursor
- Image, GIF, video, and YouTube embeds
- YouTube preview cards with in-app play option
- Ping highlights using `@username`, `@username#id`, `username#id`, or `@id`
- Custom ping chime
- Theme/color customization
- Reset encryption key to default button
- ntfy rate-limit cooldown handling
- Compatibility mode for older PowerShell SoyCord text messages

## Screenshots

Add screenshots here later:

```md
![SoyCord chat window](screenshots/chat.png)
![Inline Soymoticons](screenshots/soymoticons.png)
```

## How chats work

A SoyCord room is just an ntfy topic.

Example:

```text
Server: https://ntfy.sh
Topic: soycord
Encryption key: your shared key
```

Everyone who uses the same server, topic, and key joins the same encrypted chat.

## Encryption

SoyCord encrypts messages before sending them to ntfy.

The current message format uses:

- AES-CBC for encryption
- PBKDF2 key derivation
- HMAC verification
- `NDL2:` encrypted packet format

The app is a personal/experimental project and has not been professionally audited. Do not treat it like a hardened security product for extremely sensitive information.

## Soymoticons

Soymoticons are local bundled PNGs. They are not downloaded from Discord or any CDN.

You can type inline Soymoticons like this:

```text
hello *soymoticon-fat* soy world
```

Supported token styles:

```text
*soymoticon-glad*
/soymoticon-glad
:soymoticon-glad:
```

Bundled Soymoticons:

```text
meh
mad
glad
upset
fat
unimpressed
greed
over
```

Typing `*` in the message bar opens the Soymoticon autocomplete list.

Controls:

```text
Up/Down = move through suggestions
Enter/Tab = insert selected Soymoticon
Escape = close suggestions
```

## Pings

SoyCord can highlight messages and play a chime when you are mentioned.

Supported ping formats:

```text
@username
@username#12345
username#12345
@12345
```

Your own messages should not ping you.

## Commands

Useful commands:

```text
/help
/keycheck
/resync
/syncdebug
/upload
/file
/pfp <image url or local file path>
/panic
/panic full
```

Command meanings:

```text
/help        shows help
/keycheck    shows the encryption key fingerprint
/resync      reloads local history and checks ntfy cache/backlog
/syncdebug   shows sync/debug information
/upload      opens file upload
/file        opens file upload
/pfp         sets profile picture
/panic       wipes local history
/panic full  wipes local history, cache, and logs while keeping config safe
```

## Background unread checking

SoyCord can slowly check inactive pinned topics while the app is open.

It does this carefully to avoid hammering ntfy:

```text
active topic = live sync
inactive pinned topics = slow cache checks
```

Unread badges show beside pinned topics, for example:

```text
# friends (3)
# soycord
# airoom (1)
```

Opening a topic clears its unread badge.

Important limit: SoyCord can only fetch old messages that still exist in the ntfy server cache, or messages that were already saved locally. If the ntfy server has deleted old cached messages, SoyCord cannot recover them.

## File uploads

SoyCord supports encrypted image/file uploads through ntfy attachments.

Images and GIFs can embed directly in chat. Other files show as downloadable encrypted file placeholders.

Large files may fail depending on ntfy server limits.

## YouTube and media embeds

SoyCord supports:

```text
images
GIFs
mp4/webm/mov/m4v videos
YouTube links
encrypted local uploads
```

YouTube links show a preview card first, then can be opened or played in-app when WebView2 is available.

## Building from source

Requirements:

```text
Windows
.NET 8 SDK
```

Build:

```bat
build.bat
```

The build script creates a standalone installer EXE:

```text
SoyCordCSharp\bin\Release\net8.0-windows\win-x64\publish\SoyCord-Installer-v2.0.30.exe
```

That EXE is the one you run or share.

## Installing

Run the installer EXE.

The setup wizard installs SoyCord into the user's AppData folder and creates shortcuts.

The loose installer EXE can delete itself after setup, while the installed app stays in AppData.

Saved data is kept unless you manually wipe it.

## Saved data

SoyCord stores config and local history under the user's AppData folder.

It keeps things like:

```text
username
user ID
topics
encryption key
profile picture settings
local history
cache/logs
```

The installer cleanup options are designed to remove old program files/shortcuts without deleting saved user data.

## Compatibility

SoyCord C# can still read older PowerShell SoyCord text messages.

Normal text messages are sent in a legacy-compatible format so mixed old/new clients can still talk.

Some newer features only work between newer C# clients:

```text
inline Soymoticons
encrypted attachments
profile picture metadata
unread badges
background cache watching
YouTube preview cards
```

## Known limitations

- Public ntfy servers can rate-limit clients.
- Old messages only exist while the ntfy server still caches them, unless SoyCord already saved them locally.
- Attachments may expire or fail depending on the ntfy server.
- This is an experimental personal project, not a professionally audited secure messenger.
- WebView2 may be needed for the best in-app media playback.

## Recommended use

For best results:

```text
Use a unique topic name
Use a shared custom encryption key
Keep SoyCord open if you want background unread checking
Avoid spamming huge uploads on public ntfy servers
Use /syncdebug if sync seems weird
Use /resync if a topic looks stale
```

## License

Add a license before publishing publicly.

Good simple choices:

```text
MIT License
Apache-2.0
GPL-3.0
```

If you are not sure, MIT is usually the easiest for a small open-source app.

## Credits

Built by Philip.

Powered by:

```text
C#
WinForms
ntfy
WebView2
Soymoticons
```
