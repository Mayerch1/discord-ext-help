### discord-ext-help

An extension to create an interaction based Help Menu.

**There are no front-facing docs for this and it's not on PyPI.**

## Installing

Installing is done purely via git:

```py
python -m pip install -U git+https://github.com/Mayerch1/discord-ext-help
```

## Getting Started

To setup a basic help-page, the extension needs to be loaded as cog.
Execute the following before starting the bot.

To automatically detect slash commands, the extension should be loaded at last.
Before starting the bot, `init_help` should be called.

```py
import discord
from discord.ext.help import Help

bot = discord.Bot(...)
# ...
bot.load_extension('discord.ext.help.help')

Help.init_help(bot, auto_detect_commands=True)
bot.run(TOKEN)
```

To offer some more options, certain attributes can be set.
These attributes unlock more help buttons.

```py
Help.set_default_footer(custom_footer)
Help.set_feedback(FEEDBACK_CHANNEL, FEEDBACK_MENTION)
Help.invite_permissions(
    discord.Permissions(attach_files=True)
)
Help.support_invite('<invite url>')
Help.set_tos_file('legal/tos.md')
Help.set_privacy_file('legal/privacy.md')
```

If an error happens then an exception of type `HelpException` is raised.

This second example shows how to create a custom help page:
This can be used with or without automatic command detection.

The extension will add pagination once more than one field is shown

```py
from discord.ext import menus

elements = [
    HelpElement(cmd_name='/help', description='show this message'),
    HelpElement(cmd_name='/fuel time', description='use this for time limited races'),
    HelpElement(cmd_name='/fuel laps', description='use this for lap limited races')
]
page = HelpPage(
    name='parameters',
    title='QuoteBot Help',
    description='Explain command parameters',
    elements=elements,
    emoji='✏️'
)
Help.add_page(page)
```


## List of settings

```py
Help.init_help(bot, auto_detect_commands=True) # must be called first, but after .load_extension
```
Initialize the Bot, must be called after `.load_extension` but before `bot.run(...)`.
On `auto_detect_commands`, the bot will try to index existing slash commands. For this to work, 
the module is best loaded at last.


```py
Help.add_page(page: HelpPage, make_default=False)
```
Add a new page to the Help Menu, optionally make it the new default page.


```py
Help.set_default_footer(default_footer:str)
```
Set the default footer at the bottom of each help page. Can be override in custom pages


```py
Help.set_feedback(channel_id: int, role_id: int)
```
Set the `channel_id` of a feedback channel, direct feedback is redirected to.
The bot must have access to `channel_id`. 

If `role_id` is specified, it is used as mentionwith every feedback.



```py
Help.invite_permissions(permissions: discord.Permissions)
```
Set the permissions required by this bot. This is used to generate the invite url


```py
Help.support_invite(link: str)
```
Invite link for your bot support server.


```
Help.set_tos_text(text: str)
```
The text which is shown when the `ToS` Button is presesd


```
Help.set_privacy_text(text: str)
```
The text which is shown when the `Privacy` Button is presesd

```
Help.set_tos_file(file: str)
```
Identical to `set_tos_text`, but the text is read from a file.

```
Help.set_privacy_file(file: str)
```
Identical to `set_privacy_text`, but the text is read from a file.

```
Help.set_github_url(github_url:str)
```
Set the repository url for the bot. This is used as a secondary support link
(in addition to the `support_invite`)

## Create custom pages


Each Page is initialized as follows
```py
class HelpPage:
    def __init__(self, name: str, title: str, emoji=None, description:str = None, elements:list[HelpElement] = [], override_footer:str = None):
        pass
```
The name is used as reference in the pagination and must be unique. The emoji can optionally be specified for prettier pagination.

All other attributes correspond to the same embed attributes.


```py
class HelpElement:
    def __init__(self, cmd_name: str, description: str):
        pass
```
Each subelement is represented as field in an embed.
The content must not necessarily describe a command.
