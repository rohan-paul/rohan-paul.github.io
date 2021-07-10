# VS Code Settings, Extensions, tips and tricks and solutions to common issues for my Web…

To install any extensions from Pallette

![](https://cdn-images-1.medium.com/max/1200/1*VZU_lDgPngNZ_Kog1fSKpQ.jpeg)

### To install any extensions from Pallette

If, for example, we want to install the `html-css-class-completion` extension, launch VS Code Quick Open (Ctrl+P), paste the following command, and press enter.

`ext install html-css-class-completion`

### Apply permanent global word wrap

First open settings by File > Preference > Settings >OR

Directly open the Pallette by pressing `Ctrl + Shift + P` start typing "Settings" then from the options select "Preferences: Open Settings'

So here just add the below line

`"editor.wordWrap": "on",`

### Simulating “alert” in VS-Code by console-logging it

“alert” is not part of JavaScript, it’s part of the window object provided by web browsers. So it doesn’t exist in the vs-code context.

Running ‘alert’ directly inside a script in vscode like below

`alert`("Hello World")

Wil output `ReferenceError: alert is not defined`

To simulate ‘alert’ need have the below at the beginning of the script

```
if (typeof alert === "undefined") {  global.alert = function (message) {    console.log(message)  }}
```

```
alert("Hello World")
```

### Syncing All VS-Code Extensions, themes and cofigurations across multiple computers

I used this super-popular extension -

[marketplace.visualstudio.com/items?itemName=Shan.code-settings-sync](https://marketplace.visualstudio.com/items?itemName=Shan.code-settings-sync)

Follow the step metioned in their official doc, you need to create a gist and get you Personal Access Token.

For first time upload and also for regular Upload/Update, just type Type “>Sync” In Command Palette > Select **Sync: Update / Upload settings** > Enter the GitHub token in the window and click enter.

For First time Download of your Settings in after you install vscode in a machine or after completely removng VS Code and re-installing

Type “>Sync” In Command Palette >> Select **Sync download**

It will open its home page within VS Code > Choose Configure Sync > Enter gist ID and Github Token >

I most probably will take around 10 minutes and then you can verify that the extensions were all downloaded into your **~/.vscode/extensions** directory (for Linux / Ubuntu machines)

If for some reason it does not work first time reset the token in VSCode by **selecting form Command pallatte > Sync (Reset Setting) — It will clear the cached token from VSCode**

### Possible Issues you may face while syncing

I got something like, **“Error — Toggle Developer tools”**

This seems to be an open issue in github. So, I pretty much had to reset synching, and then create a new Access Token and new gist, and upload the whole thing again.

That is I had to do the whole sync-setting up process again.

### Issue in vscode — “Experimental support for decorators is a feature that is subject to change in a future release”

I had to resort to following steps to resolve this issue.

**Step-A**

Go to File/preference/user settings in vscode, put this code

{ “typescript.tsdk”: “node\_modules/typescript/lib” }

**Step-B**

Create `tsconfig.json` file in the root directory of your project and include the following options.

```
{
```

```
  "compilerOptions": {
```

```
    "experimentalDecorators": true,
```

```
    "allowJs": true }
```

```
}
```

**Step-C**

File/preference/user settings in vscode,

On the left are default settings, on the right you can enter USER settings.

Scroll down through the (white color) TOP LEVEL headings on the left until you come to TYPESCRIPT.

Scroll down through the TYPESCRIPT settings until you find the following entry:

**“javascript.implicitProjectConfig.experimentalDecorators”: false**

COPY this entry into USER SETTINGS on the right, and change “false” to “true” — so your USER SETTINGS now include the following entry:

`"javascript.implicitProjectConfig.experimentalDecorators": true`

And now you may the restart VSCode

### Set tab-space-style for 2-spaces

**STEP-1**

Take a look at the bottom right-hand side of the screen i.e. the extreme bottom blue color horizontal border of VSCode. You should see something that says Spaces or Tab-Size.

Mine shows spaces, →

Click on the Spaces (or Tab-Size) Choose Indent Using Spaces or Indent using Tabs Select the amount of spaces or tabs you like.

just need to right click on the file then choose format code to do the conversion

**STEP-2**

File > Preferences > User Settings > Include the following 2 lines in User Settings

`"editor.tabSize": 2,`

`"editor.detectIndentation": false`

### Delete Trailing Spaces on Save

Install the extension > [Trailing Space](https://marketplace.visualstudio.com/items?itemName=shardulm94.trailing-spaces)

File > Preference > Settings > User Settings > Put the below

`"trailing-spaces.saveAfterTrim": true`

### Change the global font-size

File -> Preferences -> User Settings

`"editor.fontSize": 18`

### Managing Multiple Terminals — Integrated Terminal

You can create multiple terminals open to different locations and easily navigate between them. Terminal instances can be added by hitting the plus icon on the top-right of the TERMINAL panel or by triggering the “Ctrl+Shift+ \`” command (thats the tilda key). This creates another entry in the dropdown list that can be used to switch between them.

### Install typings

Install typings to bring in the .d.ts files which power javascript intellisense.

`npm install typings --global`

### Change Keymap / Keyboard shortcuts so I can assign custom keymap / custom keyboard shortcuts / custom keybinding

File > Preference > Keyboard Shortcuts > Search in the search-box to find the action (for example “toggle” ) > double click on the default and press the key combination that I want > Hit Enter

I was facing problem binding to Ctrl+Shift+e => which is default for showing the File Explorer — for an open github issue — Solution –

Preference > Open Keyboard Shortcuts (keybindings.json) > Open ‘Default Keyboard Shortcuts JSON’ (which is the long list of json of all the keybindings) and then find for this keybinding (Ctrl+Shift+e) and then copy-paste the configuration to

`{ "key": "ctrl+shift+e", "command": "workbench.view.explorer" }`

### multi line-editing-in-vscode

Keep the cursor on the first line > Shift + Alt + ↑ or ↓

### Ignore / Exclude some folders (like e.g. node\_modules ) when searching across project — search.exclude\` setting

Exclude folder option to remove unwanted folders like node\_modules or others you don’t want to open in Visual Studio Code.

To exclude a folder, go to File > Preferences, and search for file.exclude in the search settings. You can add the pattern of the folder you don’t want Visual Studio Code to open.

OR you can directly edit in the settings.json file (below is my settings for it)

First > File > Preferences > Settings > Add the following

```
"files.exclude": {    "*/**/node_modules/": true,    "wp-includes/": true,    "wp-admin/": true,    "wp-*.*": true,    "wp-config.*": false,    "xmlrpc.php": true,    "*/**/cache/": true,    "*/**/uploads/": true,    "*/**/infinitewp/": true,    "*/**/.github/": true,    "*/**/coverage/": true,    "*.pyc": true,    "**/*.min.js": true,    "*.min.css": true,    "*.pdf": true,    "*.min.js.map": true,    "*.pyo": true,    "*.exe": true,    "*.dll": true,    "*.obj": true,    "*.o": true,    "*.a": true,    "*.lib": true,    "*.so": true,    "*.dylib": true,    "*.ncb": true,    "*.sdf": true,    "*.suo": true,    "*.pdb": true,    "*.idb": true,    "**/.DS_Store": true,    "*.class": true,    "*.psd": true,    "*.db": true,    "*.sublime-workspace": true,    "yarn.lock": true,    ".gitignore": false  },
```

After doing Ctrl + Shift + F in the left side Search-Panel >> Expand the 3 horizontal dots to get to Toggle-Search-Detais > Check the **‘use exclude settings’** option is activated. If its activated the gear icon will be have a border of blue.

### Emmet with JS / JSX for React project so after entering div and tab it properly expands like it does in .html file

**Step A**\> From the bottom of VS code select JavaScript React as the language.

**Step B** >For setting it globally → After some experimentation following the above link the below setup worked for me > File > Preference > Settings > Put the below in User Settings

```
"emmet.syntaxProfiles": { "javascript": "jsx" },    "emmet.includeLanguages": {        "javascript": "javascriptreact",        "jsx-sublime-babel-tags": "javascriptreact"    }
```

However the above many not be enough — As I have to set the default .js language mode to javascriptreact — otherwise it will keep reverting back to javascript(babel) — So I had set the below — and immediately I saw the language mode changing to javascrtiptreact.

```
"files.associations": {    "*.js": "javascriptreact"}
```

### To replace multiple word simultaneously — like when working with React replace all “class” with “className”

select the first “class” and then press Ctrol + D and the next “class” will be selected > press Ctrol + D another time and the next “class” will be selected. After all are selected, just delete “class” and type “className” and all of them will be changed.

### To manually terminate a task in Visual Studio Code? — e.g. when a non-ending while loop keeps on running or a bad recursion is accidentally executed

As of now, in Unix there’s no key shortcut to terminate a running task. So you need to create your own shortcut for it. Go to File->Preferences->Keyboard Shortcuts and add your shortcut to the keybindings.json.

Here’s example code:

{  
 “key”: “ctrl+shift+alt+t”,  
 “command”: “workbench.action.tasks.terminate”  
 }

And now for terminating the task just press **ctrl+shift+alt+t**

**However this extension** [**Task-Kill**](https://github.com/shy2net/vscode-taskkill "https://github.com/shy2net/vscode-taskkill") **allows you to easily kill active network processes on a specific port.**

*   Kill all interesting active network processes (such as 8000, 4200, 3000 for NodeJS and Angular) automatically
*   Kill a specific network process by specifying a port number (`Ctrl + Del`, on mac `Cmd + Del`)
*   Show and kill specific active network processes manually (`Ctrl + Shift + Del`, on mac `Cmd + Shift + Del`).

![](https://cdn-images-1.medium.com/max/800/1*Dx3T5FyxpaOFQ3zqdXidJQ.gif)
undefined

**And from the same extension killing a specific active network process**

![](https://cdn-images-1.medium.com/max/800/1*Wsy3Tgj0iPrc9OMo1LMMAQ.gif)
undefined

### To highlight current line ? Very important while searching variable in a long file

Try this in your settings.json:

```
"workbench.colorCustomizations": {
```

```
  "editor.lineHighlightBackground": "#254117"}
```

This assumes that you have the below settings set up as well

`"editor.renderLineHighlight": "line",`

And in the above, I can change the hex-code from HTML’s official doc to suit my eyes.

### To set different color schemes with multiuple vscode windows open

Need to be done each time I want to apply this setting on different windows

Instead of whole VS-Code wide settings, here I have to use Workspace settings (Ctrl+Shift+P and type workspace settings) and edit the color to your choice. Now it should get changed only to the opened instance. Like below

```
{    "workbench.colorTheme": "Material Theme Darker High Contrast"}
```

And it will persistent for that Project as the settins will be saved in .vscode/settings.json file of that project.

### To git push using VS-Code’s built in Source Control tab

1> View > SCM > Ctrl + Shift + G G

2> Just type the message > and press Ctrl + Enter . This will commit everything.

3> Then click on the three horizontal bar > Push to > just type ‘origin’ and it will push to the branch that VS-Code is currently in.

### Add open with vscode to context menu in Ubuntu, by right clicking

A> First install FileManager Actions

```
sudo add-apt-repository ppa:daniel-marynicz/filemanager-actionssudo apt updatesudo apt install filemanager-actions-nautilus-extension
```

The above 3 are no more applicabel in Ubunt 20.04 instead run below directly

`sudo apt-get install filemanager-actions`

After installation you can launch

`fma-config-tool`

B> Add new action (the + icon) > Command tab >

In the Path field put > `**code**`

And in Parameters: **%B**

File > Save

You may need to restart the Machine

### Clean Uninstall and Reinstall

`sudo apt-get purge –auto-remove`

I very importantly delete (manually or with `rm -rf`) the below 2 folders

```
~/.vscode
```

```
and
```

```
~/.config/Code
```

### Issue — High CPU usage in vscode

Suddenly one day my vscode taking 35% CPU. Found out this great feature.

`"Developer: Open Process Explorer"`

command from command prompt

It will tell exactly which processes or extension taking so much CPU or Memory. Then just uninstall that extension. Today I had to uninstall Angular Language Service VS code extension and then restart VSCode.

Without uninstalling this extension, dven restarting the whole machine did not work.

### Edit multiple lines simultaneously

Just copying and pasting code is a bit old fashioned. In VS Code, you can edit multiple lines by adding cursors to different locations. This is very useful if you are going to use the same code on different lines. Instead of copying and pasting the code in all places, you can hold Alt and click to add a cursor in the places you want to type or edit the code. After adding cursors, edit the code to see the changes in the places where the cursors were added.

This also is quite useful when I want to paste the same word to multiple places. Hold `Alt` at multiple places and then `Ctrl + V`

### Reopen your latest closed file

Just press CTL+SHIFT+T. This will reopen the latest closed window. You can press this is multiple times to open multiple windows. This also works in your browser if you closed a tab that you don’t want to

### Make VS Code change language to JavaScript React when open a JSX file

Within VS Code, in the bottom right of the window you will see a smiley face — to the left of that is the language the currently visible file is associated with (e.g. JavaScript). Ensure your currently opened file is a .jsx file.

Clicking this will reveal a menu at the top. Click the `Configure File Association for '.jsx'...`, and then choose "JavaScript React".

To permanently set this as default

Press `cntrl + ,` and add this to user settings `JSON` file:

```
"files.associations": {    "*.js": "javascriptreact"}
```

like

![](https://cdn-images-1.medium.com/max/800/1*lnurJbGUNvwrrVTKSIW-dg.png)
undefined

And to keep the default to plain Javascript

```
"files.associations": {    "*.js": "javascript",    "*.ejs": "html"  }
```

#### Some shortcuts I use all the time in a regular day

1.  CTRL+, = Open user settings
2.  CTRL+K CTRL+S = Show shortcuts
3.  CTRL+G = Go to line
4.  CTRL+P = Go to file
5.  SHIFT+ALT+I = Insert cursor at end of each line selected
6.  CTRL+SHIFT+L = Select all occurrences of current selection
7.  CTRL+L = Select current line
8.  CTRL+F2 = Select all occurrences of current word

#### And some other super useful Markdown related shortcuts

**Following are from the very useful extension**

[**markdown-shortcuts**](https://marketplace.visualstudio.com/items?itemName=mdickin.markdown-shortcuts "https://marketplace.visualstudio.com/items?itemName=mdickin.markdown-shortcuts")

**Make** `**a code block**` ctrl+M ctrl+C

**Make** `**inline code**` ctrl+M ctrl+I

**Toggle Checkboxes** `Ctrl + M Ctrl X`

**Toggle Bullet** `Ctrl + M Ctrl B`

**Make an image** `Ctrl + Shift + L`

**Paste Screenshot Image directly in markdown**

Use this is superbly useful extension

[https://marketplace.visualstudio.com/items?itemName=mushan.vscode-paste-image](https://marketplace.visualstudio.com/items?itemName=mushan.vscode-paste-image "https://marketplace.visualstudio.com/items?itemName=mushan.vscode-paste-image")

Just take the screenshot and then > Ctrl + Alt + V

**Make a numbered list — from command pallette type > “Toggle Number list” and select “Markdown Shortcuts: Toggle Number List”**

**Paste link on selected text (extremely useful)**

Copy a Link > select the word and then **ctrl + v**

![](https://cdn-images-1.medium.com/max/800/1*vKgJDNvZAMqtFO4TgoTpIA.gif)
undefined

#### **Markdown preview side by side**

[https://marketplace.visualstudio.com/items?itemName=shd101wyy.markdown-preview-enhanced](https://marketplace.visualstudio.com/items?itemName=shd101wyy.markdown-preview-enhanced "https://marketplace.visualstudio.com/items?itemName=shd101wyy.markdown-preview-enhanced")

*   `cmd-k v or ctrl-k v` - Open preview to the Side

### Making Custom snippets — Super time saving

First make one for Console.log(“”) in all .js file

Ctrl + Shift + P to open pallette > Type ‘snippet’ > Select ‘Configure User Snippets’ >

You’ll be presented with a list to choose the type of snippet you want to create. Either a global one (for all projects or just one specific project) or one that is specific to a language.

![](https://cdn-images-1.medium.com/max/800/1*jVpTWlHiDAy-0EzF-fvRGA.png)
undefined

Third: Then write any code snippet you want, following this simple template provided by VS Code, where: So for example, from that long doropdown list, choose **“JavaScript React”** for your React files and **‘Babel JavaScript’** for regular JS files.

*   `scope` defines which language or project this code snippet will be used in.
*   `prefix` is the shorthand you will use to call on the code snippet.
*   `body` is the code snippet that will be injected.
*   and well `description` describes what the code snippet does.
*   `$1` and `$2` are placeholders for the code you will add to complete the snippet (you can move from one placeholder to another using the tab key).

Below an example, for React codebase (i.e. choosing **“JavaScript React”**) and here for my current ‘cl’ snippet I will see the below for `.js` file

```
"console": {		"prefix": "cl",		"body": [			"console.log('$1');",			"$2"		],		"description": "Log output to console"  }
```

### Custom snippets for React’s .js files to include className={“”}

`Ctrl + Shift + P` to open pallette > Type 'snippet' > Select 'Configure User Snippets' >

Then from that long doropdown list, choose “JavaScript React” for my React files and ‘Babel JavaScript’ for regular JS files. And here for my current ‘cssm’ snippet I will see the below.

```
{  "react-css-modules": {    "prefix": "cssm",    "body": [      "className={styles[\"\"]}"    ],    "description": "react-css-modules"  }}
```

**If above simple step to set custom-snippet is NOT working then Launch vscode with below command**

`code --disable-extensions`

to check. → if this issue occur when all extensions are disabled. And in my case I saw that it was indeed working with all extension disabled but will extenstion enabled, this was not working.

**This is how to resolve the above**

[**(Taken from my own ans in StackOverflow)**](https://stackoverflow.com/questions/59810935/vscode-custom-react-snippets-not-working-in-javascriptreact-json-file-and-also)

Got this strange behaviour, in a React Component.js file when I type the snippet (‘cssm’ in this case) — before the React Component File’s return() statement, I get the expected behaviour. But if I type the snippet (‘cssm’) inside the return() statement, I dont get anything. i.e. the snippet does not give me the code completion option inside the return () statment.

The source of the problem was that VSCode was not recognizing this file to be a “javascriptreact” language type.

So this what needed to be done.

> **In a React Component’s .js file, try the tokens inspector via “F1 > Inspect TM Scopes” and position the cursor at those insert position. That will tell you the detected, embedded language. Depending on your grammar it might be a different language**

And below is what I am getting.

![](https://cdn-images-1.medium.com/max/800/0*6pxuIX-mFUws9iXX.png)
undefined

**So as you can see, this not JavaScript nor JavaScript React but the jsx-attr language.** So now I had to target that type with vscode snippet

So now, I put the whole snippet in the jsx-attr type target file `~/.config/Code/User/snippets/jsx-attr.json`

And now the custom snippets worked as expected.

#### List out or Print the full List of Extenstions that you are currently using

In case you need to send all your installed extensions to a colleague.

Run below command in Linux / Unix:

```
code --list-extensions | xargs -L 1 echo code --install-extension
```

In my case it printed the below

code --install-extension 2gua.rainbow-brackets  
code --install-extension aaron-bond.better-comments  
code --install-extension adpyke.codesnap  
code --install-extension alefragnani.Bookmarks  
code --install-extension alefragnani.numbered-bookmarks  
code --install-extension alexeyvax.vscode-open-native-terminal  
code --install-extension bceskavich.theme-dracula-at-night  
code --install-extension bierner.markdown-preview-github-styles  
code --install-extension ChakrounAnas.turbo-console-log  
code --install-extension chrisdias.vscode-opennewinstance  
code --install-extension christian-kohler.npm-intellisense  
code --install-extension christian-kohler.path-intellisense  
code --install-extension CoenraadS.bracket-pair-colorizer  
code --install-extension Compulim.compulim-vscode-closetag  
code --install-extension dbaeumer.vscode-eslint  
code --install-extension dzannotti.vscode-babel-coloring  
code --install-extension ecmel.vscode-html-css  
code --install-extension eg2.tslint  
code --install-extension eg2.vscode-npm-script  
code --install-extension emilast.LogFileHighlighter  
code --install-extension emmanuelbeziat.vscode-great-icons  
code --install-extension Equinusocio.vsc-community-material-theme  
code --install-extension Equinusocio.vsc-material-theme  
code --install-extension equinusocio.vsc-material-theme-icons  
code --install-extension eriklynd.json-tools  
code --install-extension esbenp.prettier-vscode  
code --install-extension fabiospampinato.vscode-open-in-terminal  
code --install-extension faceair.ayu-one-dark  
code --install-extension fms-cat.theme-monokaisharp  
code --install-extension formulahendry.auto-close-tag  
code --install-extension formulahendry.auto-complete-tag  
code --install-extension formulahendry.auto-rename-tag  
code --install-extension formulahendry.code-runner  
code --install-extension formulahendry.terminal  
code --install-extension gencer.html-slim-scss-css-class-completion  
code --install-extension gerane.Theme-Sunburst  
code --install-extension ghgofort.neon-vommit  
code --install-extension goessner.mdmath  
code --install-extension HookyQR.beautify  
code --install-extension httpsterio.henna  
code --install-extension ivanzusko.theme-jo-light  
code --install-extension James-Yu.latex-workshop  
code --install-extension jasonnutter.search-node-modules  
code --install-extension jdinhlife.gruvbox  
code --install-extension jolaleye.horizon-theme-vscode  
code --install-extension josef.rouge-theme  
code --install-extension kamikillerto.vscode-colorize  
code --install-extension kube.theme-kay  
code --install-extension leizongmin.node-module-intellisense  
code --install-extension MaxfieldWalker.vscode-color-theme-spirited-away  
code --install-extension mdickin.markdown-shortcuts  
code --install-extension mgmcdermott.vscode-language-babel  
code --install-extension michelemelluso.code-beautifier  
code --install-extension mikestead.dotenv  
code --install-extension mohsen1.prettify-json  
code --install-extension monokai.theme-monokai-pro-vscode  
code --install-extension mrmlnc.vscode-scss  
code --install-extension ms-azuretools.vscode-docker  
code --install-extension ms-python.anaconda-extension-pack  
code --install-extension ms-python.python  
code --install-extension ms-vscode.node-debug2  
code --install-extension ms-vscode.Theme-MaterialKit  
code --install-extension ms-vsliveshare.vsliveshare  
code --install-extension msjsdiag.debugger-for-chrome  
code --install-extension msjsdiag.vscode-react-native  
code --install-extension mushan.vscode-paste-image  
code --install-extension naumovs.color-highlight  
code --install-extension nickdemayo.vscode-json-editor  
code --install-extension Nimda.deepdark-material  
code --install-extension njpwerner.autodocstring  
code --install-extension Nuuf.theme-hackershaze  
code --install-extension P-de-Jong.vscode-html-scss  
code --install-extension philsinatra.nested-comments  
code --install-extension PKief.material-icon-theme  
code --install-extension pranaygp.vscode-css-peek  
code --install-extension rafamel.subtle-brackets  
code --install-extension redhat.vscode-yaml  
code --install-extension rsbondi.highlight-words  
code --install-extension Shan.code-settings-sync  
code --install-extension shardulm94.trailing-spaces  
code --install-extension shd101wyy.markdown-preview-enhanced  
code --install-extension shubham-saudolla.lilac  
code --install-extension sibiraj-s.vscode-scss-formatter  
code --install-extension sldobri.daily  
code --install-extension sleistner.vscode-fileutils  
code --install-extension SmukkeKim.theme-setimonokai  
code --install-extension sourcegraph.sourcegraph  
code --install-extension sourcegraph.vscode-sourcegraph-theme  
code --install-extension spywhere.guides  
code --install-extension steoates.autoimport  
code --install-extension stevencl.addDocComments  
code --install-extension streetsidesoftware.code-spell-checker  
code --install-extension teabyii.ayu  
code --install-extension thebarkman.vscode-djaneiro  
code --install-extension tht13.python  
code --install-extension torn4dom4n.latex-support  
code --install-extension trybick.terminal-zoom  
code --install-extension ueno.react-native-code-styles  
code --install-extension vincaslt.highlight-matching-tag  
code --install-extension voldemortensen.rainbow-tags  
code --install-extension vscode-icons-team.vscode-icons  
code --install-extension wmaurer.change-case  
code --install-extension yzhang.markdown-all-in-one  
code --install-extension ZainChen.json  
code --install-extension Zignd.html-css-class-completion

By [Rohan Paul](https://medium.com/@paulrohan) on [July 29, 2020](https://medium.com/p/ecad6bda5aa3).

[Canonical link](https://medium.com/@paulrohan/vs-code-settings-extensions-tips-and-tricks-and-solutions-to-common-issues-for-my-web-ecad6bda5aa3)

Exported from [Medium](https://medium.com) on December 12, 2020.