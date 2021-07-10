# Sublime- assign custom-keyboard shortcut for code-block while editing markdown file

(Very short blog on adding a custom keybinding to multi-line code block)

(Very short blog on adding a custom keybinding to multi-line code block)

![](https://cdn-images-1.medium.com/max/1200/1*dd-a-yHqAeTRMKStptl1hQ.jpeg)

A very quick post for a small time-saving trick in Sublime.

While editing a markdown file having an option to direclty invoke a code block like below with a keyboard shortcut is immensely time saving.

\`\`\`js  
\`\`\`

And as of August-2020 in Sublime, I could not find a keyboard shortcut for adding code-blocks either natively from Sublime’s in-built keybinding or an option to add one from in-built commads.

So I am taking help of a third party package [**Markdown Code Blocks**](https://packagecontrol.io/packages/Markdown%20Code%20Blocks) and will add a keyboard shortcut of **‘Ctrl + Shift + z’** for multi-line code block (you can ofcourse assign any other Keyboard combinations to it).

**First install the package (**[**Markdown Code Blocks**](https://packagecontrol.io/packages/Markdown%20Code%20Blocks)**) from Command palette using** `**Package Control**`

Ctrl + Shift + P to open palette > Type ‘Install Package’ and choose the one you see below selected and then search for search for `Markdown Code Blocks`

![](https://cdn-images-1.medium.com/max/800/1*4VVUPMSkyuJjqBaSaGEqgw.png)
undefined

#### Usage

A command will appear in your command palette.  
Search for the `Markdown Code Blocks: Add Block` command, and type in your desired language name in lowercase letters. And you will see something like below for a .js code formatting

\`\`\`js  
\`\`\`

**Now time to add a keybinding to invoke the above.**

First to know which is the relevant Sublime command (which I have to invoke with a keybinding), after installing `[Markdown Code Blocks](https://packagecontrol.io/packages/Markdown%20Code%20Blocks)` open Command Palette > start typeing **‘code blocks’** like below and you will the see the relevant command is this ‘**Markdown Code Blocks: Add Block’**

![](https://cdn-images-1.medium.com/max/800/1*7C9Dc9ZucnQwjzruWkW1gg.png)
undefined

**So all I had to do is adding a keybinding to that.**

Now nevigate to the Github source code of the `[Markdown Code Blocks](https://packagecontrol.io/packages/Markdown%20Code%20Blocks)` package which is [https://github.com/molnarmark/markdowncodeblocks](https://github.com/molnarmark/markdowncodeblocks)

And search for the string **‘Code block’ in this repo.** And I get the below result.

![](https://cdn-images-1.medium.com/max/800/1*4JZy1kGCrsOITXHapgBtow.png)
undefined

From the above I can see that the command that I am interested is the below one.

![](https://cdn-images-1.medium.com/max/800/1*esmHfRS-QlCjHRAkAfsXiw.png)
undefined

So now from Sublime > Command Palette > Key Bindings > Open my .sublime-keymap file and include the below command

```
{ "keys": ["ctrl+shift+z"], "caption": "Markdown Code Blocks: Add Block", "command": "markdown_add_code_block" },
```

And that’s it, now I can invoke the `"Markdown Code Blocks: Add Block"` command with `"ctrl+shift+z"`

By [Rohan Paul](https://medium.com/@paulrohan) on [August 2, 2020](https://medium.com/p/53cf96be20d3).

[Canonical link](https://medium.com/@paulrohan/sublime-assign-custom-keyboard-shortcut-for-code-block-while-editing-markdown-file-53cf96be20d3)

Exported from [Medium](https://medium.com) on December 12, 2020.