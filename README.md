# nvim-spider
Use the `w`, `e`, `b` motions like a spider. Considers camelCase and skips insignificant punctuation.

> __Warning__  
> This plugin is still WIP and subject to change without warning.

<!--toc:start-->
- [Features](#features)
	- [CamelCaseMotion](#camelcasemotion)
	- [Skipping Insignificant Punctuation](#skipping-insignificant-punctuation)
	- [Text Object](#text-object)
- [Installation](#installation)
- [Roadmap](#roadmap)
- [Credits](#credits)
<!--toc:end-->

## Features
The `w`, `e`, `b` (and `ge`) motions work the same as the default ones by vim, except for two differences:

### CamelCaseMotion
The movements happen by subwords, meaning it stops at the sub-parts of an CamelCase (or SCREAMING_SNAKE_CASE or kebab-case) variable are considered words.

```lua
-- positions vim's `w` will move to
local myVariableName = FOO_BAR_BAZ
--    ^              ^ ^

-- positions spider's `w` will move to
local myVariableName = FOO_BAR_BAZ
--    ^ ^       ^    ^ ^   ^   ^
```

### Skipping Insignificant Punctuation
A sequence of one or more punctuation characters is considered significant if it is surrounded by whitespace and does not includes any non-punctuation characters. This speeds up the movement across the line by reducing the number of mostly unnecessary stops.

```lua
foo == bar .. "baz"
--  ^      ^    significant punctuation

foo:find("a")
-- ^    ^  ^  insignificant punctuation
```

```lua
-- positions vim's `w` will move to
if foo:find("%d") and foo == bar then print("[foo] has" .. bar) end
-- ^  ^^   ^  ^^  ^   ^   ^  ^   ^    ^    ^  ^  ^ ^  ^ ^  ^  ^ ^  -> 21

-- positions spider's `w` will move to
if foo:find("%d") and foo == bar then print("[foo] has" .. bar) end
-- ^   ^      ^   ^   ^   ^  ^   ^    ^       ^    ^    ^  ^    ^  -> 14
```

> __Note__  
> vim's `iskeyword` option is ignored by this plugin.

### Text Object
For an alternative `iw` text object that considers CamelCase word parts, check out the "subword" text object of [nvim-various-textobjs](https://github.com/chrisgrieser/nvim-various-textobjs).

## Installation

```lua
-- packer
use { "chrisgrieser/nvim-spider" }

-- lazy.nvim
{ "chrisgrieser/nvim-spider" },
```

No `.setup()` function is required, and no keybindings are created by default. Below are the mappings to replace the default `w`, `e`, and `b` motions with this plugin's version of them.

```lua
-- Keymaps
vim.keymap.set({"n", "o", "x"}, "w", function() require("spider").motion("w") end, { desc = "Spider-w" })
vim.keymap.set({"n", "o", "x"}, "e", function() require("spider").motion("e") end, { desc = "Spider-e" })
vim.keymap.set({"n", "o", "x"}, "b", function() require("spider").motion("b") end, { desc = "Spider-b" })
vim.keymap.set({"n", "o", "x"}, "ge", function() require("spider").motion("ge") end, { desc = "Spider-ge" })
```


## Roadmap
- [ ] Counts
- [ ] Movements to the next/previous line
- [ ] Dot repeats ([Dot-repeat *for text objects* seems a bit more tricky, help is welcome.](https://github.com/chrisgrieser/nvim-various-textobjs/issues/7#issuecomment-1374861900))

## Credits
__Thanks__  
This is basically a port of the [CamelCaseMotion](https://github.com/bkad/CamelCaseMotion), written in lua, and with some minor improvements.

<!-- vale Google.FirstPerson = NO -->
__About Me__  
In my day job, I am a sociologist studying the social mechanisms underlying the digital economy. For my PhD project, I investigate the governance of the app economy and how software ecosystems manage the tension between innovation and compatibility. If you are interested in this subject, feel free to get in touch.

__Profiles__  
- [Discord](https://discordapp.com/users/462774483044794368/)
- [Academic Website](https://chris-grieser.de/)
- [GitHub](https://github.com/chrisgrieser/)
- [Twitter](https://twitter.com/pseudo_meta)
- [ResearchGate](https://www.researchgate.net/profile/Christopher-Grieser)
- [LinkedIn](https://www.linkedin.com/in/christopher-grieser-ba693b17a/)

__Buy Me a Coffee__  
<br>
<a href='https://ko-fi.com/Y8Y86SQ91' target='_blank'><img height='36' style='border:0px;height:36px;' src='https://cdn.ko-fi.com/cdn/kofi1.png?v=3' border='0' alt='Buy Me a Coffee at ko-fi.com' /></a>
