# Sublimetext Settings, packages, techniques and solutions to common issues for my Web Development…

All the setup below is for my Ubuntu Box ( mainly 18.04 and 20.04)

All the setup below is for my Ubuntu Box ( mainly 18.04 and 20.04)

![](https://cdn-images-1.medium.com/max/1200/1*mdBG1Egnfr0H0JMqLfKW5Q.jpeg)

### To sync Packages and Settings across multiple machines in Sublime

**This in my opinion one of the most important and incredibly helpful package, saving me days of work in setting up my IDE infrastructure in a new machine.** And I also use this similar package and similar synching-strategy for my Atom and VsCode as well.

Install the package [**Sync Settings**](https://packagecontrol.io/packages/Sync%20Settings) Sublime package-control.

*   A) Create a new personal access token in Github, which has the gist scope (i.e. in Github, before creating the token we need to check the “gist” scope).
*   B) Activate permissions: Gist -> create gists.
*   C) Copy the access token to Sync Settings configuration.
*   D) Create a new gist and save it. (i.e. just create a new regular gist with only content being this newly created token). Note, its preferable to make this a secret gist, as it may have more configuration’s info in my machine.
*   E) Copy the gist id (last part of url after the username in Github after I create the gist) and Paste it to Sync Settings configuration (i.e. in Preferences > Packages Settings > Sync Settings > Settings file).
*   So after fresh install of sublime my Preferences > Packages Settings > Sync Settings > Settings — User file was the below.

The below is example gist\_id and access\_token

```
{	"access_token": "100aff4ff446e4ettew4t64t4t4616ytry",	"auto_upgrade": false,	"gist_id": "t5646h4re6y4y6r4yr4yr6e"}
```

#### And the the regular Steps for package Synchronize Settings — For uploading or backing up from the existing installations

Preference > Package Settings > Sync Settings > Upload

**So afte installing Sublime to a new Computer for Downloading all packages**

First install sync-setting package. Then.

A) Preferences > Packages Settings > Sync Settings > Settings — User >> Copy-paste the above contents

```
{	"access_token": "100aff4ff446e4ettew4t64t4t4616ytry",	"auto_upgrade": false,	"gist_id": "t5646h4re6y4y6r4yr4yr6e"}
```

B) Preference > Package Settings > Sync Settings > Download

C) Restart Sublime > and wait. You will see all the packages getting installed one after another and the the size of your ~/.config/sublime-text-3/Packages directoy will be filled with those installed packages.

#### Imp Note — Secret vs Public Gists

Secret gist is visible for anyone who has a link — The Personal Access Token is ofcourse NOT visible to others

But if somebody goes to my Gist profile and sorts all my gist by ‘Recetnly Create’ — NONE of my Secret gist will show up.

Further from github [official page](https://docs.github.com/en/enterprise/2.13/user/articles/about-gists#public-gists "https://docs.github.com/en/enterprise/2.13/user/articles/about-gists#public-gists")

> **Public gists**

> Public gists show up in Discover, http(s)://\[hostname\]/gist/discover or http(s)://gist.\[hostname\]/discover if subdomains are enabled, where people can browse new gists as they’re created. They’re also searchable, so you can use them if you’d like other people to find and see your work. After creating a gist, you cannot convert it from public to secret.

> **Secret gists**

> Secret gists don’t show up in Discover, http(s)://\[hostname\]/gist/discover or http(s)://gist.\[hostname\]/discover if subdomains are enabled, and are not searchable. Use them to jot down an idea that came to you in a dream, create a to-do list, or prepare some code or prose that’s not ready to be shared with the world. After creating a gist, you cannot convert it from public to secret.

> Secret gists aren’t private. If you send the URL of a secret gist to a friend, they’ll be able to see it. However, if someone you don’t know discovers the URL, they’ll also be able to see your gist. If you need to keep your code away from prying eyes, you may want to create a private repository instead.

### Word-wrap globally

Open Preference > Settings

```
"word_wrap": "true","wrap_width": "auto"
```

**To implement above in a markdown file, open a markdown file and then Preference > Settings and put the above configuration.**

**“word\_wrap”: “auto” vs “true”**

`"word_wrap": "auto",`

Controls whether word wrap is always on, always off or auto-selects based on the type of file. You’ve set that one to true, it will enable word wrap everywhere.

**“wrap\_width”: 0,**

Controls what the wrap column is set to. The default value of 0 corresponds to wrapping at the width of the window , which is the Automatic setting that you want, while setting it to some other value wraps at that column specifically.

### Color Scheme / Theme installation

Open Command Pallette > Ctrl + Shift + P > Install Package > start typing the theme name

To activate or switch between themes >

Open Command Pallette > Ctrl + Shift + P > Start typing Theme > Select Themr: List themes

However this option Themr: List themes will NOT show the preview of the themes when navigating between them before selection one.

#### So how can I preview all sublime text 3 themes before selecting one ?

By installing [**sublime-text-theme-switcher-menu**](https://github.com/chmln/sublime-text-theme-switcher-menu "https://github.com/chmln/sublime-text-theme-switcher-menu") package

Preview and choose themes using the command palette — Ctrl+Shift+P and type:

*   `UI: Select Theme`
*   `UI: Select Color Scheme`

![](https://cdn-images-1.medium.com/max/800/1*qQFujG4lIPe1xCWXg7xoPQ.gif)
undefined

Or just navigate to the `Preferences -> Theme` menu.

![](https://cdn-images-1.medium.com/max/800/1*Ycwt1Qa2kFQ_xbFPT7j0YQ.png)
undefined

### Add “open with” Sublime to context menu in Ubuntu, so by right clicking on a file or folder you can open it in Sublime

You have to create a desktop launcher file.

Create the file named **sublime-text-3.desktop**, paste the content (example below) and update the path for the Exec key and Icon. Replace all Sublime text 2 by Sublime text 3. Move the file into **/usr/share/applications/**.

\[Desktop Entry\]  
Version=1.0  
Type=Application  
Name=Sublime Text  
GenericName=Text Editor  
Comment=Sophisticated text editor for code, markup and prose  
Exec=/opt/sublime\_text/sublime\_text %F  
Terminal=false  
MimeType=text/plain;  
Icon=/opt/sublime\_text/Icon/256x256/sublime-text.png  
Categories=TextEditor;Development;  
StartupNotify=true  
Actions=Window;Document;

\[Desktop Action Window\]  
Name=New Window  
Exec=/opt/sublime\_text/sublime\_text -n  
#OnlyShowIn=Unity;

\[Desktop Action Document\]  
Name=New File  
Exec=/opt/sublime\_text/sublime\_text --command new\_file  
#OnlyShowIn=Unity;

Now right click on your text file, click on Properties, Open with tab and you should able to choose your Sublime text 3 application.”

If you installed sublime text using ‘Ubuntu Make’(umake), then probably your .desktop file should be in the below location.

`cat ~/.local/share/applications/sublime-text.desktop`

And in my case this .desktop file was in

/snap/sublime-text/85/opt/sublime\_text/sublime\_text.desktop

**Now, note the following line in the file, which is the Exec key.**

Exec=/opt/sublime\_text/sublime\_text %F

Make sure you typed capital F, not small. “F” here is for list of files. Use for apps that can open several local files at once. Each file is passed as a separate argument to the executable program.

`sudo update-desktop-database`

You may need to restart your machine.

#### As a separate note the significance of the [Exec key](https://developer.gnome.org/desktop-entry-spec/#exec-variables "https://developer.gnome.org/desktop-entry-spec/#exec-variables")

> The Exec key must contain a command line. A command line consists of an executable program optionally followed by one or more arguments. The executable program can either be specified with its full path or with the name of the executable only. If no full path is provided the executable is looked up in the $PATH environment variable used by the desktop environment. The name or path of the executable program may not contain the equal sign (“=”). Arguments are separated by a space.

### To Create Custom Snippets

All snippets are saved in — `~/.config/sublime-text-3/Packages/User/Snippets`

*   A> Create the “Snippets” directory if already it does not exists.
*   B> Tools > Developer > New Snippet
*   B. The whole actual content (i.e. output of the snippet) that will need to come to the editor goes inside

<content><!\[CDATA\[  
  <p>  
  $1  
  </p>  
\]\]></content>

C. And the trigger, i.e. the partial word that will bring it in, goes inside

```
<tabTrigger>css</tabTrigger>
```

So in the above case after I type ‘css’ and then hit tab it will render a blank area which will take up css syntax.

\`\`\`css

\`\`\`

So the final content of my `css.sublime-snippet` file is

<snippet>  
 <content><!\[CDATA\[  
\`\`\`css  
  $1  
\`\`\`  
\]\]></content>  
 <!-- Optional: Set a tabTrigger to define how to trigger the snippet -->  
 <!-- <tabTrigger>hello</tabTrigger> -->  
 <!-- Optional: Set a scope to limit where the snippet will trigger -->  
 <!-- <scope>source.python</scope> -->

<tabTrigger>css</tabTrigger>  
</snippet>

**And now the next snippet — either,** just manually create another file in the above directory (~/.config/sublime-text-3/Packages/User/Snippets) with the same naming convention or Tools > Developer > New Snippet

For example for my file js.sublime-snippet, I have the below

<snippet>  
 <content><!\[CDATA\[  
\`\`\`js  
  $1  
\`\`\`  
\]\]></content>  
 <!-- Optional: Set a tabTrigger to define how to trigger the snippet -->  
 <!-- <tabTrigger>hello</tabTrigger> -->  
 <!-- Optional: Set a scope to limit where the snippet will trigger -->  
 <!-- <scope>source.python</scope> -->

<tabTrigger>js</tabTrigger>  
</snippet>

### Customize / Create the keybindings / Custom Keyboard Shortcut

Preferences > Key Bindings — User and copying over lines from Preferences > Key Bindings — Default, changing the keys value as needed. e.g.,

As an example, I made the following changes to the default by having the below lines on the right side User section.

```
[	{ "keys": ["ctrl+alt+n"], "command": "build" },	{ "keys": ["ctrl+b"], "command": "toggle_side_bar" },	{		"keys": ["ctrl+\\"],		"command": "set_layout",		"args":		{			"cols": [0.0, 0.5, 1.0],			"rows": [0.0, 1.0],			"cells": [[0, 0, 1, 1], [1, 0, 2, 1]]		}	},	{		"keys": ["ctrl+shift+\\"],		"command": "set_layout",		"args":		{			"cols": [0.0, 1.0],			"rows": [0.0, 1.0],			"cells": [[0, 0, 1, 1]]		}	}]
```

**Set Custom Keyboard shortcuts for Opening Integrated Terminal**

From Paletter open Keybindings and put below

```
{ "keys": ["ctrl+`"], "command": "toggle_terminus_panel" }
```

**An important note on how Sublime handles the priority among the key-bindings**

This is to understand how Sublime knows when a key binding needs to override another key binding.

Sublime will start loading all the key bindings located in Packages/Default; then, it will sort all the installed packages in an alphabetic order and load them one after another. The last one to be loaded will always be Packages/User. Each keymap file that is being loaded will override any other key bindings that have been loaded before it in case of a key conflict. This means that Packages/User will override all the key bindings because it is being loaded last.

### Different color-scheme for different Project and Workspace / project specific color-scheme

First Open any Folder regularly > Project > Save Project as.. > Give a name . It will create a `.sublime-project` file

And then Open up your .sublime-project file. (Top menu -> Project -> Edit project)

It will look similar to this:

```
{    "folders":    [        {            "follow_symlinks": true,            "path": "/home/user/some_path/project"        }    ]}
```

Simply add the color scheme setting, so now the below configurations will look like below for my “mern-image-uploading-boilerplate” project

```
{	"folders":	[		{		"follow_symlinks": true,		"path": "~/codeLap/React/MERN-Practice-Post-Bootcamp/DONE/mern-image-uploading-boilerplate"		}	],	"settings": {		"color_scheme": "Packages/Theme - Spacegray/base16-ocean.dark.tmTheme"	}}
```

For getting the name of the color theme, just open another window and > go the main (global) settings file >> Preference > Settings and check

I saw with the below minimum settings also it work in the .sublime-project

```
{	"folders":	[		{			"path": "."		}	],	"settings": {		"color_scheme": "Packages/Materialize/schemes/Material Cobalt.tmTheme",	}}
```

### Prettier Auto Format on Save

**The below steps are all you need to activate** `**auto_format_on_save**`

A> `npm i -g prettier`

B> Open Sublime Text and using Package Control, type Install Package and then JsPrettier

C> After JsPrettier is installed >>

Preferences > Package Settings > JsPrettier > Settings — User to edit JsPrettier.sublime-settings.

My config looks like this:

```
{    // macOS/Linux:    "prettier_cli_path":  "~/.nvm/versions/node/v14.4.0/bin/prettier",    "node_path": "~/.nvm/versions/node/v14.4.0/bin/node",     "auto_format_on_save_excludes": [        "*/node_modules/*"    ],     "auto_format_on_save": true,     "auto_format_on_save_excludes": [        "*/node_modules/*"    ],     "prettier_options": {                "printWidth": 80,                "tabWidth": 2,                "singleQuote": false,                "trailingComma": "none",                "bracketSpacing": true,                "jsxBracketSameLine": false,                "parser": "babel",                "semi": true,                "requirePragma": false,                "proseWrap": "preserve",                "arrowParens": "avoid",                "htmlWhitespaceSensitivity": "css"            }
```

```
}
```

#### Very Important in the Above configuration — The “node\_path” and “prettier\_cli\_path” need to be assigned correctly.

So for the above you can use `which node` and `which prettier` command

If you still face issues — Read more the official Doc

[https://github.com/jonlabelle/SublimeJsPrettier](https://github.com/jonlabelle/SublimeJsPrettier "https://github.com/jonlabelle/SublimeJsPrettier")

And an important Issue on this -

[https://stackoverflow.com/questions/46191137/jsprettier-not-working](https://stackoverflow.com/questions/46191137/jsprettier-not-working "https://stackoverflow.com/questions/46191137/jsprettier-not-working")

#### Important Prettier configurations notes [from documentation](https://github.com/jonlabelle/SublimeJsPrettier "https://github.com/jonlabelle/SublimeJsPrettier")

**prettier\_cli\_path (default: _empty_)** If Sublime Text has problems automatically resolving a path to \[Prettier\], you can set a custom path here.

When the setting is empty, the plug-in will attempt to find Prettier by:

1.Locally installed prettier relative to active view.  
2\. Locally installed prettier relative to the Sublime Text Project file’s root directory.  
3\. The current user home directory.  
4\. JsPrettier Sublime Text plug-in directory.  
5\. Globally installed prettier.

**Examples:**

```
{    // macOS and Linux examples:    "prettier_cli_path": "/usr/local/bin/prettier"    "prettier_cli_path": "/some/absolute/path/to/node_modules/.bin/prettier"    "prettier_cli_path": "./node_modules/.bin/prettier"    "prettier_cli_path": "~/bin/prettier"    "prettier_cli_path": "$HOME/bin/prettier"    "prettier_cli_path": "${project_path}/bin/prettier"    "prettier_cli_path": "$ENV/bin/prettier"    "prettier_cli_path": "$NVM_BIN/prettier"
```

```
    // Windows examples:    "prettier_cli_path": "C:/path/to/prettier.cmd"    "prettier_cli_path": "%USERPROFILE%\\bin\\prettier.cmd"}
```

*   **node\_path (default: _empty_)** If Sublime Text has problems resolving the absolute path to node, you can set a custom path here.

#### Examples:

{

// macOS/Linux:

"node\_path": "/usr/local/bin/node"

"node\_path": "/some/absolute/path/to/node"

"node\_path": "./node"

"node\_path": "~/bin/node"

"node\_path": "$HOME/bin/node"

"node\_path": "${project\_path}/bin/node"

"node\_path": "$ENV/bin/node"

"node\_path": "$NVM\_BIN/node"

// Windows:

"node\_path": "C:/path/to/node.exe"

"node\_path": "%USERPROFILE%\\\\bin\\\\node.exe"

}

*   **auto\_format\_on\_save (default: _false_)** Automatically format the file on save.
*   **auto\_format\_on\_save\_excludes (default: \[\])** File exclusion patterns to ignore when `auto_format_on_save` is enabled.
*   Example:

{  
_"auto\_format\_on\_save\_excludes"_: \["\*/node\_modules/\*", "\*/file.js", "\*.json"\]  
}

*   custom\_file\_extensions (default: \[\]) There’s built-in support already for `js`, `jsx`, `mjs`, `json`, `html`,`graphql/gql`, `ts`, `tsx`, `css`, `scss`, `less`, `md`, `mdx`, `yml`, `vue` and `component.html` (angular html) files.
*   Put additional file extensions here, and be sure not to include the leading dot in the file extension.

#### Issue — JsPrettier on Sublime text was giving error — no such file /home/.nvm/-verslions/node —

Solution → Preference > Package Settings > JsPrettier > User Settings and update the ‘node\_path’ as below

```
{    // macOS/Linux:    "prettier_cli_path": "~/.nvm/versions/node/v14.4.0/bin/prettier",    "node_path": "~/.nvm/versions/node/v14.4.0/bin/node",     "auto_format_on_save_excludes": [        "*/node_modules/*"    ],     "auto_format_on_save": true,     "auto_format_on_save_excludes": [        "*/node_modules/*"    ],     "prettier_options": {                "printWidth": 80,            }
```

```
}
```

### Markdown related shortcuts

Directly Paste image from Clipboard

One shortcomings of Sublime on the point of Paste Screenshot Image (from Clipboard) directly in markdown — There is nothing of this sort at the moment.

[This discussion](https://github.com/SublimeText-Markdown/MarkdownEditing/issues/429#issuecomment-359433391 "https://github.com/SublimeText-Markdown/MarkdownEditing/issues/429#issuecomment-359433391") — Clearly says that unlike, the ‘vscode-paste-image’ there’s NO equivalent in Sublime.

```
"There is this plugin which I cannot get to work, robinchenyu/imagepaste"
```

There’s also [this Reddit link](https://www.reddit.com/r/SublimeText/comments/ei3waj/how_to_paste_image_into_a_markdown_file/ "https://www.reddit.com/r/SublimeText/comments/ei3waj/how_to_paste_image_into_a_markdown_file/") — Which also asks for the same in Sublime, and at the moment nobody has replied.

**So the closest keyboard-shortcuts for Sublime for pasting image that we have currently is from the ‘**[**MarkdownEditing**](https://github.com/SublimeText-Markdown/MarkdownEditing/blob/master/README.md#key-bindings "https://github.com/SublimeText-Markdown/MarkdownEditing/blob/master/README.md#key-bindings")**’ package**

But in this case, I have separately create the image.png or image.jpeg etc file and the paste that within the square bracket

Ctrl + Alt + V — It Creates or pastes the contents of the clipboard as an inline link on selected.

**Open Markdown preview side-by-side with** [**MarkdownLivePreview package**](https://packagecontrol.io/packages/MarkdownLivePreview "https://packagecontrol.io/packages/MarkdownLivePreview")

Install the package from package control >Then open the preview, you can search up in the command palette (**ctrl+shift+p**)

MarkdownLivePreview: Open Preview

But if you prefer to have a shortcut, add this to your keybindings file (Open Command pallette with `Ctrl + Shift + P`and start typing "Key Bindings" and select `"Preferences: Key Bindings"`):

```
{    "keys": ["alt+m"],    "command": "open_markdown_preview"}
```

**Other keyboard-shortcuts for markdown editing in Sublime from the ‘**[**MarkdownEditing**](https://github.com/SublimeText-Markdown/MarkdownEditing/blob/master/README.md#key-bindings "https://github.com/SublimeText-Markdown/MarkdownEditing/blob/master/README.md#key-bindings")**’ package**

**Alt + B and Alt + I** — These are bound to bold and italic. They work both with and without selections. If there is no selection, they will just transform the word under the cursor. These keybindings will unbold/unitalicize selection if it is already bold/italic.

**Command + L** Select a line.

**Markdown keyboard shortcut for Javascript code block**

I could not find any package to do it, however I have my custom-snippet in below file

`~/.config/sublime-text-3/Packages/User/Snippets/js.sublime-snippet`

Which has the below content. So that the line `<tabTrigger>js</tabTrigger>` will make sure that in any markdown file as soon as I type 'js' and hit tab it will bring a .js code block

<snippet>  
 <content><!\[CDATA\[  
\`\`\`js  
  $1  
\`\`\`  
\]\]></content>  
 <!-- Optional: Set a tabTrigger to define how to trigger the snippet -->  
 <!-- <tabTrigger>hello</tabTrigger> -->  
 <!-- Optional: Set a scope to limit where the snippet will trigger -->  
 <!-- <scope>source.python</scope> -->

<tabTrigger>js</tabTrigger>  
</snippet>

**And also note the above custom-snippet will get transferred / synched with sync-settings package (explained at the top of this article), which is a great time-saver, as you dont have to build this custom snippet each time you install sublimetext.**

Meaning once I set up my Sublime custom snippet in a single Machine, I never have to do that again for a new machine or new Sublime installation, as it will be perfectly synched-up each time I have to freshly install Sublime.

### Run JavaScript code snippets in Sublime Text console inside the editor

Step — A) Navigate to Tools | Build System | New Build System. You’ll get a bare bones JSON configuration file called untitled.sublime-build with a single command in it.

```
{  "cmd": ["make"]}
```

Change this to

```
{  "cmd": ["node", "$file"],  "selector": "source.js"}
```

And save as node.sublime-build in the folder it suggests which in Linux is

`~/.config/sublime-text-3/Packages/User`

Step — B) After making the above changes in the build-system > Go to Tools > Build System > select node. And the the keyboard shortcut for running snippet is — **Ctrl + B**

**Issues even after doing the above steps by creating a node.sublime-build if it does not run the snippet, you may need to do some more troubleshooting**

Step — A) Type

which node

in terminal. It outputs the path of the node executable, which in most cases is /usr/local/bin/node. Now create a new build system (Tools > Build System > New Build System) with the following settings in Sublime Text. I had the following configuration in that file.

```
{  "cmd": ["~/.nvm/versions/node/v9.3.0/bin/node", "$file"],  "selector": "source.js"}
```

Step — B)

Set the default node path by Preferences → Package Settings → NodeJs → User Settings

modify

`"node_command": false,`

to

`"node_command": "/usr/local/bin/node",`

or whatever is the location of your node file — which you will know by running

which node

### Run Python code / snippets in Sublime Text console inside the editor

First run < `which python` > in terminal – it will give me the directory of my Python - `/usr/bin/python`

Then create a build system for sublime text 3 -

Go to Tools->Build System->New Build System.

Paste the following code in the file and Save.

By default it will select — `~/.config/sublime-text-3/Packages/User` – Save Here (Name the file anything like Pyhon-3 )

Then choose that new build system >> Tools Build System > Python-3

```
{   "shell_cmd": "/usr/bin/python -u \"$file\"",    "file_regex": "^[ ]*File \"(...*?)\", line ([0-9]*)",    "selector": "source.python"}
```

And then just Ctrl + Shift + B ( Which is Tools > Build with.. )

### Setting Indentation which determine the size of the tab stops

So e.g. each time I press tab I want to make sure, it takes 2 spaces (instead of 4) AND while coding in JS, each time I press ENTER after curly braces, the next line starts at 2 spaces (instead of 4)

Preference > Settings-User > a file will open > search for “tab\_size” and change the value to 2 from 4 (which is the default)

[https://www.sublimetext.com/docs/3/indentation.html](https://www.sublimetext.com/docs/3/indentation.html "https://www.sublimetext.com/docs/3/indentation.html") -

Settings files are consulted in this order:

*   Packages/Default/Preferences.sublime-settings
*   Packages/Default/Preferences ().sublime-settings
*   Packages/User/Preferences.sublime-settings
*   Packages//.sublime-settings
*   Packages/User/.sublime-settings

In general, you should place your settings in Packages/User/Preferences.sublime-settings. If you want to specify settings for a certain file type, for example, Python, you should place them in Packages/User/Python.sublime-settings. Example Settings File

Try saving this as Packages/User/Preferences.sublime-settings

```
{    "tab_size": 4,    "translate_tabs_to_spaces": false}
```

### To view Sublime’s crash log / session log / general operational log when launching Sublime

View > Show Console

This allows you to scroll through the history of your session and look for any errors. It is also the console to the internal version of Python 2.6 embedded in ST2, similar to the >>> that you see if you start Python from the command line, so you can use it to get all sorts of system information that way, if you know how to use the API and, of course, Python.

### Simulating “alert” in Sublime by console-logging it

“alert” is not part of JavaScript, it’s part of the window object provided by web browsers. So it doesn’t exist in the context you’re trying to use it in.

Running ‘alert’ directly like below

`alert`("Hello World")

Wil output `ReferenceError: alert is not defined`

To simulate ‘alert’ need have the below at the beginning of the script

```
if (typeof alert === "undefined") {  global.alert = function (message) {    console.log(message)  }}
```

```
alert("Hello World")
```

### Exclude node\_modules out of Sublime Text searches

Add the following to your preferences file I.e Preference > Settings

“folder\_exclude\_patterns”:\[“.git”,”node\_modules”, “_/client/coverage”, “_/client/build”\],

Exclude Files while searching — [https://coderwall.com/p/dza-mw/exclude-files-folders-in-sublime-text](https://coderwall.com/p/dza-mw/exclude-files-folders-in-sublime-text "https://coderwall.com/p/dza-mw/exclude-files-folders-in-sublime-text")

“file\_exclude\_patterns”: \[“_.pyc”, “_.pyo”, “_.exe”, “_.dll”, “_.obj”,”_.o”, “_.a”, “_.lib”, “_.so”, “_.dylib”, “_.ncb”, “_.sdf”, “_.suo”, “_.pdb”, “_.idb”, “.DS\_Store”, “_.class”, “_.psd”, “_.db”, “\*.sublime-workspace”, “\*package-lock.json”, “_docs.min.js”, “_.min.js”\]

### To see and check the full list of installed packages

Specially useful when you want to figure out if a particular package has been installed or not. e.g. if you want to see which “markdown” related packages are installed already.

Preferences -> Package Control -> List Packages > Then just type the word “markdown”

### Some other very useful shortcuts I use daily

*   **File Switching CTRL + P** — Just press ctrl + p and start typing the name of the file you want. Once it shows up, just press enter.
*   **Goto Symbols** (To find a method in a long file) CTRL + R — When you have a large file with a bunch of methods, pressing ctrl + r will list them all and make them easier to find. Just start typing the one you want and press enter.
*   **Multi edit** — ctrl + click
*   **Select the current word and the next same word ctrl + d**
*   **Find a word in your files and then select them all** — ctrl + shift + f AND alt + enter:
*   **From the sidebar to open the location of a particular file** — right click on the file > Reveal

By [Rohan Paul](https://medium.com/@paulrohan) on [July 30, 2020](https://medium.com/p/54522c9bc012).

[Canonical link](https://medium.com/@paulrohan/sublimetext-settings-packages-techniques-and-solutions-to-common-issues-for-my-web-development-54522c9bc012)

Exported from [Medium](https://medium.com) on December 12, 2020.