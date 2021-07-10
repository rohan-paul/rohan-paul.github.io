# Atom Editor — some common settings, packages and solutions to common issues for my Web Development…

Imp Note — Secret vs Public Gists

![](https://cdn-images-1.medium.com/max/1200/1*yk0NFrlnuqf5sbi4c61i1A.jpeg)

### Sync Atom Packages and settings for multiple computer

**This in my opinion one of the most important and incredibly helpful package, saving me days of work in setting up my IDE infrastructure in a new machine.** And I also use this similar package and similar synching-strategy for my Sublime and VsCode as well.

Follow official guide from the package — [sync-settings](https://github.com/atom-community/sync-settings "https://github.com/atom-community/sync-settings")

### Gist Setup

1.  Open Sync Settings configuration in Atom Settings.
2.  Create a [new personal access token](https://github.com/settings/tokens/new?scopes=gist "https://github.com/settings/tokens/new?scopes=gist") which has the `gist` scope and be sure to activate permissions: Gist -> create gists.
3.  Copy the access token to Sync Settings configuration or set it as an environmental variable GITHUB\_TOKEN.
4.  Create a [new gist](https://gist.github.com/ "https://gist.github.com/"):

*   The description can be left empty. It will be set when invoking the `backup` command the first time.
*   Use `packages.json` as the filename.
*   Put some arbitrary non-empty content into the file. It will be overwritten by the first invocation of the `backup`command
*   Save the gist.
*   Now backup from local to Gist — The below will backup my already created Gist from local Atom.

`sync-settings:backup`

1.  Copy the gist id (last part of url after the username) to Sync Settings configuration or set it as an environmental variable GIST\_ID.

Given GitHub Gists are by default public, its recommended that if you don’t want other people to easily find your gist (i.e. if you use certain packages, storing auth-tokens, a malicious party could abuse them), you should make sure to create a secret gist.

#### Imp Note — Secret vs Public Gists

Secret gist is visible for anyone who has a link — The Personal Access Token is ofcourse NOT visible to others

But if somebody goes to my Gist profile and sorts all my gist by ‘Recetnly Create’ — NONE of my Secret gist will show up.

Further from github [official page](https://docs.github.com/en/enterprise/2.13/user/articles/about-gists#public-gists "https://docs.github.com/en/enterprise/2.13/user/articles/about-gists#public-gists")

Public gists

Public gists show up in Discover, http(s)://\[hostname\]/gist/discover or http(s)://gist.\[hostname\]/discover if subdomains are enabled, where people can browse new gists as they’re created. They’re also searchable, so you can use them if you’d like other people to find and see your work. After creating a gist, you cannot convert it from public to secret.

Secret gists

Secret gists don’t show up in Discover, http(s)://\[hostname\]/gist/discover or http(s)://gist.\[hostname\]/discover if subdomains are enabled, and are not searchable. Use them to jot down an idea that came to you in a dream, create a to-do list, or prepare some code or prose that’s not ready to be shared with the world. After creating a gist, you cannot convert it from public to secret.

Secret gists aren’t private. If you send the URL of a secret gist to a friend, they’ll be able to see it. However, if someone you don’t know discovers the URL, they’ll also be able to see your gist. If you need to keep your code away from prying eyes, you may want to create a private repository instead.

#### Immediately After synching as above

Now my Edit > Config.. to edit Atom’s config.cson.

So my **config.cson** will look like below. The ids below are only for example (they are not real gistId or personal\_access\_token )

```
"sync-settings":    gistId: "kk8131jvm6gfgm64j6k461nm6k4696hjk46h"    hiddenSettings:      _lastBackupTime: "2020-07-30T16:34:25Z"    personalAccessToken: "hhhh56543c1n6h46h4f63h4hh54"
```

#### Regular Usage / Operation for package Synchronize Settings

Open the Atom Command Palette where you can search for the following list of commands.

Backup or restore all settings from the Packages menu or use one of the following commands:

*   `sync-settings:backup` - Here backup means uploading to Github the congiguraion of my current setting
*   `sync-settings:restore` - Here restore means downloading the whole configurations from Github and then downloading all the packages from Atom Package Repo

OR Packages > Synchronize Settings > Backup OR Restore

The first time I do Package > Backup and then to view it, do Package > View backup, I will see that gist has been updated with a huge lot of details of my Atom package.

#### Regular Steps to sync when installing Atom into a new machine

Install “sync-settings” package normally > It will ask me to input gist id (and sometime also the Personal Access Token) > Input them and that it.

### Assigning custom keyboard-shortcut to Atom

#### First, to see all the existing keybindings > In Pallette start typing “Keybindings” and select “Settings: Show Keybindings”

![](https://cdn-images-1.medium.com/max/800/1*-COuEfuoowJ3rxkoWf_xJQ.png)
undefined

Then for example, search for ‘block-comment’ and get below

![](https://cdn-images-1.medium.com/max/800/1*dD3KNrC2AXmBHjHvdmUdcw.png)
undefined

Now just as it says, you can override these keybindings by copying and pasting them into your keymap file > **So copy it and paste it in keymap.cson file** (you can open the keymap file by clicking on the link on the same screen, its towards the top).

Here’s is my full keymap.cson file at the moment (including the block-comment keybinding I discussed above)

![](https://cdn-images-1.medium.com/max/800/1*t6khHxUohD-WQxf0h8peJw.png)
undefined

[**Structure of a Keymap File**](https://flight-manual.atom.io/behind-atom/sections/keymaps-in-depth/#structure-of-a-keymap-file "https://flight-manual.atom.io/behind-atom/sections/keymaps-in-depth/#structure-of-a-keymap-file")

While updating the above keymap.cson file its important to not the syntax ans structure of the Keymap file

```
key:  key: value  key: value  key: [value, value]
```

> Objects are the backbone of any CSON file, and are delineated by indentation (as in the above example). A key’s value can either be a String, a Number, an Object, a Boolean, null, or an Array of any of these data types.

> Keymap files are encoded as JSON or CSON files containing nested hashes. They work much like style sheets, but instead of applying style properties to elements matching the selector, they specify the meaning of keystrokes on elements matching the selector. Here is an example of some bindings that apply when keystrokes pass through atom-text-editor elements:

```
'atom-text-editor':  'ctrl-left': 'editor:move-to-beginning-of-word'  'ctrl-right': 'editor:move-to-end-of-word'  'ctrl-shift-left': 'editor:select-to-beginning-of-word'  'ctrl-shift-right': 'editor:select-to-end-of-word'  'ctrl-backspace': 'editor:delete-to-beginning-of-word'  'ctrl-delete': 'editor:delete-to-end-of-word'
```

```
'atom-text-editor:not([mini])':  'ctrl-alt-[': 'editor:fold-current-row'  'ctrl-alt-]': 'editor:unfold-current-row'
```

> Beneath the first selector are several keybindings, mapping specific key combinations to commands. When an element with the atom-text-editor class is focused and Ctrl+Backspace is pressed, a custom DOM event called editor:delete-to-beginning-of-word is emitted on the atom-text-editor element.

> The second selector group also targets editors, but only if they don’t have the mini attribute. In this example, the commands for code folding don’t really make sense on mini-editors, so the selector restricts them to regular editors.

#### A common issue you may face in Atom while setting up your custom Key binding — which is that ‘ctrl-shift-,’ not working —

(This is also important for debuggin Keybinding issues)

So at first, I was configuring the keymap.cson file as below

```
'body':  'ctrl-\\': 'pane:split-left-and-copy-active-item'  'ctrl-shift-\\' 'pane:close'
```

Note from the above the first line was working fine — and so I just added the ‘shift’ for the second like for the `pane:close`

But that was not working — i.e. when I prest ‘Ctrl + Shift + \\’ key combination the paen was not closing.

And the underlying cause was Atom was not recognizing my physical key-combination (‘Ctrl + Shift + \\’) that I was doing to be indeed ‘Ctrl + Shift + \\’ for Atom.

So, to debug it I opened the `Keybinding Resolver Tooggle` from the command pallette - to see what keys Atom thinks you’re pressing when you type `Ctrl-Shift-\`

And I indeed saw that the key recognized by Atom was “|” and NOT “\\”

![](https://cdn-images-1.medium.com/max/1200/1*YBQexP1uRFqf7edje0wdcg.gif)
undefined

So now I re-configured my keymap.cson file as below

```
'body':  'ctrl-\\': 'pane:split-left-and-copy-active-item'  'ctrl-|': 'pane:close'
```

[**Keybinding Priority Order**](https://flight-manual.atom.io/behind-atom/sections/keymaps-in-depth/#specificity-and-cascade-order "https://flight-manual.atom.io/behind-atom/sections/keymaps-in-depth/#specificity-and-cascade-order")

> As is the case with CSS applying styles, when multiple bindings match for a single element, the conflict is resolved by choosing the most specific selector. If two matching selectors have the same specificity, the binding for the selector appearing later in the cascade takes precedence.

> Currently, there’s no way to specify selector ordering within a single keymap, because JSON objects do not preserve order. We handle cases where selector ordering is critical by breaking the keymap into separate files, such as snippets-1.cson and snippets-2.cson.

### Custom snippet

The idea is that you can type something like habtm and then press the Tab key and it will expand into has\_and\_belongs\_to\_many.

There is a text file in your ~/.atom directory called snippets.cson that contains all your custom snippets that are loaded when you launch Atom. To open that file, Edit > Snippets… menu in the top bar, and Atom will open the snippets.cson file to which you can add your own custom snippets.

You will need four things in order to add your custom snippet:

The name of the scope, the name of the snippet, prefix that will function as the handle of the snippet, the body of the snippet. name, prefix, and body of the snippet depend on you and however, you find the name of the scope before adding your custom snippets.

To find the scope just go to atom setting by pressing Ctrl +, and find the packages tab then search the scope that you need in the search box. For example, if you want to add the snippet for PHP language then type PHP in the search box

**Snippet Format**

So let’s look at how to write a snippet. The basic snippet format looks like this:

```
'.source.js':  'console.log':    'prefix': 'log'    'body': 'console.log(${1:"crash"});$2'
```

#### Meaning of $1

These are for Tab stops. Tab stops indicate the spots where the user can navigate by using the Tab key. With tab stops you can save the time that in-text navigation requires.

You can add tab stops using the $1, $2, $3, … syntax. Atom will position the cursor to the place it finds $1, then you can jump to $2 with the Tab key, then to $3, and so on.

The leftmost keys are the selectors where these snippets should be active. The easiest way to determine what this should be is to go to the language package of the language you want to add a snippet for and look for the “Scope” string.

For example, if we wanted to add a snippet that would work for JavaScript files, we would look up the `language-javascript` package in our Settings view > Click on that package `language-javascript` and we can see the Scope is `source.js`. Then the top level snippet key would be that prepended by a period (like a CSS class selector would do).

![](https://cdn-images-1.medium.com/max/800/1*41j469P6XaMJ6gFLV6Ji6g.png)
undefined

Here are some scopes you may want to use in your Atom projects:

```
Plain Text: .text.plain
```

```
HTML: .text.html.basic
```

```
CSS: .source.css
```

```
Sass: .source.sass
```

```
LESS: .source.css.less
```

```
JavaScript: .source.js
```

```
PHP: .text.html.php
```

```
Python: .source.python
```

```
Java: .source.java
```

Don’t forget that you will need to add a dot (.) in front of the name of the scope in order to use it in the snippets.  
cson file.

So below is an example of a working snippet which will activate after typing “cl” and then hitting tab

```
# For Language Javascript'.source.js':  'Single-line Snippet':    'prefix': 'cl'    'body': 'console.log($1);$2'
```

### [Set Custom keybinding for markdown writer](https://github.com/zhuochun/md-writer/wiki/Settings-for-Keymaps "https://github.com/zhuochun/md-writer/wiki/Settings-for-Keymaps")

Execute command `Markdown Writer: Create Default keymaps` to add the recommended keymaps to your configs. It will open your keymap configuration file and append the list of default Markdown Writer keymaps in it. You can modify the keymaps as needed.

Alternatively, you can manually edit in your keymap configuration file (Atom -> Open Your Keymap). These are the default set of keymaps provided (OSX): — Meaning the default keymaps that were addded to my main `keymap.cson` file immediately after I installed markdown writer

```
# Default Keymaps for Markdown Writer# https://atom.io/packages/markdown-writer## Wiki: https://github.com/zhuochun/md-writer/wiki/Settings-for-Keymaps#".platform-linux atom-text-editor:not([mini])":  "shift-ctrl-K": "markdown-writer:insert-link"  "shift-ctrl-I": "markdown-writer:insert-image"  "shift-ctrl-X": "markdown-writer:toggle-taskdone"  "ctrl-i":       "markdown-writer:toggle-italic-text"  "ctrl-b":       "markdown-writer:toggle-bold-text"  "ctrl-'":       "markdown-writer:toggle-code-text"  "ctrl-h":       "markdown-writer:toggle-strikethrough-text"  "ctrl-1":       "markdown-writer:toggle-h1"  "ctrl-2":       "markdown-writer:toggle-h2"  "ctrl-3":       "markdown-writer:toggle-h3"  "ctrl-4":       "markdown-writer:toggle-h4"  "ctrl-5":       "markdown-writer:toggle-h5"
```

**Side by Side Preview of Markdown file**

So to the above I added below to open Markdown preview side-by side

```
'alt-m': 'markdown-preview:toggle'
```

**Custom keybingings for inline code and code block text**

```
'ctrl-shift-w': 'markdown-writer:toggle-code-text''ctrl-shift-z': 'markdown-writer:toggle-codeblock-text'
```

By [Rohan Paul](https://medium.com/@paulrohan) on [July 31, 2020](https://medium.com/p/281bdfba4698).

[Canonical link](https://medium.com/@paulrohan/atom-editor-some-common-settings-packages-and-solutions-to-common-issues-for-my-web-development-281bdfba4698)

Exported from [Medium](https://medium.com) on December 12, 2020.