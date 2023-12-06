# fontresizer.nvim

A plugin for Neovim to resize the guifont.

### Demo

https://github.com/abcdefg233/fontresizer.nvim/assets/32760059/7da96868-019f-49da-ae33-ca40f7155b53

## Installation

With [Lazy](https://github.com/folke/lazy.nvim)

```lua
{
 "abcdefg233/fontresizer.nvim",
 cmd={"FontResizer"},
 opts={},
 dependencies={
  "abcdefg233/hcutil.nvim"
 },
}
```

Configuration

```lua
{
 "abcdefg233/fontresizer.nvim",
 cmd={"FontResizer"},
 opts={
   -- These are the default settings, not quite necessary.
  default_size=10,
   -- Affects command :FontResizer Set Default.
  change_up=1,
   -- Affects command :FontResizer Change Up.
  change_down=-1,
   -- Affects command :FontResizer Change Down.
  maximum=30,
   -- Font size will not sets higher than <maximum>.
  minimum=2,
   -- Font size will not sets lower than <minimum>.
   -- Please don't set the font size lower than 1, it seems broken.
  others={
   create_cmd=true,
    -- Create command "FontResizer" after setup.
    -- If use API, cmd may not necessary.
   create_var=true,
    -- Create Lua global variable "_G.FontResizer" after setup.
   create_api=true,
    -- Create a Module API after setup.
    -- require("fontresizr").API,
    -- If it sets false, you can still use
    -- require("fontresizr.api").
  },
 },
 dependencies={
  "abcdefg233/hcutil.nvim"
 },
}
```

## Usage

Lua API

```lua
-- You can get the API by these following ways.
-- If you do not like Lua way, you can disable API
-- by setting options create_var and create_api to false,
-- and use command instead.

require("fontresizr.api")
-- Works but not elegant.

require("fontresizr").API
-- Needs *others.create_api==true.

_G.FontResizer.API
-- Needs *others.create_api==true.
-- Needs *others.create_var==true.
```

```lua
_G.FontResizer.API.Change_Up()
 -- Change font size by change_up
_G.FontResizer.API.Change_Down()
 -- Change font size by change_down
_G.FontResizer.API.Change(<num>)
 -- Change font size by <num>
 -- - Positive <num> increases font size
 -- - Negative <num> decreases font size
_G.FontResizer.API.Set_Default()
 -- Set font size to default_size
_G.FontResizer.API.Set(<num>)
 -- Set font size to <num>
```

Cmd

If `*others.create_cmd==true`, you will get a command `FontResizer` after setup.

You can use it in these following terms

```lua
:FontResizer Change Up
 -- Change font size by change_up
:FontResizer Change Down
 -- Change font size by change_down
:FontResizer Change <num>
 -- Change font size by <num>
 -- - Positive <num> increases font size
 -- - Negative <num> decreases font size
:FontResizer Set Default
 -- Set font size to default_size
:FontResizer Set <num>
 -- Set font size to <num>
```

Use by shortcuts

Normal Keys

```lua
local keys={
 [{"n"}]={
  {"<A-Up>",  _G.RainbowCursor.API.Change_Up,  "RainbowCursor Change Up"},
  {"<A-Down>",_G.RainbowCursor.API.Change_Down,"RainbowCursor Change Down"},
  {"<A-0>",   _G.RainbowCursor.API.Set_Default,"RainbowCursor Set Default"},
 },
 [{"n","s","x","o","i","c","t"}]={
  {"<C-ScrollWheelUp>",  _G.RainbowCursor.API.Change_Up,  "RainbowCursor Change Up"},
  {"<C-ScrollWheelDown>",_G.RainbowCursor.API.Change_Down,"RainbowCursor Change Down"},
  {"<C-MiddleMouse>",    _G.RainbowCursor.API.Set_Default,"RainbowCursor Set Default"},
 },
}
local default_opts={noremap=true,silent=true}
for modes,key in ipairs(keys) do
 for _,mode in ipairs(modes) do
  local opts=default_opts
  opts.desc=key[3]
  vim.keymap.set(mode,key[1],key[2],opts)
 end
end
```

Lazy keys

```lua
{
 {"<A-Up>",             function() _G.FontResizer.API.Change_Up() end,  desc="FontResizer Change Up"},
 {"<A-Down>",           function() _G.FontResizer.API.Change_Down() end,desc="FontResizer Change Down"},
 {"<A-0>",              function() _G.FontResizer.API.Set_Default() end,desc="FontResizer Set Default"},
 {"<C-ScrollWheelUp>",  function() _G.FontResizer.API.Change_Up() end,  mode={"n","s","x","o","i","c","t"},desc="FontResizer Change Up"},
 {"<C-ScrollWheelDown>",function() _G.FontResizer.API.Change_Down() end,mode={"n","s","x","o","i","c","t"},desc="FontResizer Change Down"},
 {"<C-MiddleMouse>",    function() _G.FontResizer.API.Set_Default() end,mode={"n","s","x","o","i","c","t"},desc="FontResizer Set Default"},
}
```

## Example Configuration

Convenient setup for lazy.nvim users

```lua
{
 "abcdefg233/fontresizer.nvim",
 cmd={"FontResizer"},
 keys={
  {"<A-Up>",             function() _G.FontResizer.API.Change_Up() end,  desc="FontResizer Change Up"},
  {"<A-Down>",           function() _G.FontResizer.API.Change_Down() end,desc="FontResizer Change Down"},
  {"<A-0>",              function() _G.FontResizer.API.Set_Default() end,desc="FontResizer Set Default"},
  {"<C-ScrollWheelUp>",  function() _G.FontResizer.API.Change_Up() end,  mode={"n","s","x","o","i","c","t"},desc="FontResizer Change Up"},
  {"<C-ScrollWheelDown>",function() _G.FontResizer.API.Change_Down() end,mode={"n","s","x","o","i","c","t"},desc="FontResizer Change Down"},
  {"<C-MiddleMouse>",    function() _G.FontResizer.API.Set_Default() end,mode={"n","s","x","o","i","c","t"},desc="FontResizer Set Default"},
 },
 opts={
  -- These are the default settings, not quite necessary.
  default_size=10,
   -- Affects command :FontResizer Set Default.
  change_up=1,
   -- Affects command :FontResizer Change Up.
  change_down=-1,
   -- Affects command :FontResizer Change Down.
  maximum=30,
   -- Font size will not sets higher than <maximum>.
  minimum=2,
   -- Font size will not sets lower than <minimum>.
   -- Please don't set the font size lower than 1, it seems broken.
  others={
   create_cmd=true,
    -- Create command "FontResizer" after setup.
    -- If use API, cmd may not necessary.
   create_var=true,
    -- Create Lua global variable "_G.FontResizer" after setup.
   create_api=true,
    -- Create a Module API after setup.
    -- require("fontresizr").API,
    -- If it sets false, you can still use
    -- require("fontresizr.api").
  },
 },
 dependencies={
  "abcdefg233/hcutil.nvim"
 },
}
```

Modularize

```lua
---@class LazySpec
local M={}
M[1]="abcdefg233/fontresizer.nvim"
-- Event
M.cmd={"FontResizer"}
M.keys={
 {"<A-Up>",             function() _G.FontResizer.API.Change_Up() end,  desc="FontResizer Change Up"},
 {"<A-Down>",           function() _G.FontResizer.API.Change_Down() end,desc="FontResizer Change Down"},
 {"<A-0>",              function() _G.FontResizer.API.Set_Default() end,desc="FontResizer Set Default"},
 {"<C-ScrollWheelUp>",  function() _G.FontResizer.API.Change_Up() end,  mode={"n","s","x","o","i","c","t"},desc="FontResizer Change Up"},
 {"<C-ScrollWheelDown>",function() _G.FontResizer.API.Change_Down() end,mode={"n","s","x","o","i","c","t"},desc="FontResizer Change Down"},
 {"<C-MiddleMouse>",    function() _G.FontResizer.API.Set_Default() end,mode={"n","s","x","o","i","c","t"},desc="FontResizer Set Default"},
}
-- Setup
M.opts={
  -- These are the default settings, not quite necessary.
 default_size=10,
  -- Affects command :FontResizer Set Default.
 change_up=1,
  -- Affects command :FontResizer Change Up.
 change_down=-1,
  -- Affects command :FontResizer Change Down.
 maximum=30,
  -- Font size will not sets higher than <maximum>.
 minimum=2,
  -- Font size will not sets lower than <minimum>.
  -- Please don't set the font size lower than 1, it seems broken.
 others={
  create_cmd=true,
   -- Create command "FontResizer" after setup.
   -- If use API, cmd may not necessary.
  create_var=true,
   -- Create Lua global variable "_G.FontResizer" after setup.
  create_api=true,
   -- Create a Module API after setup.
   -- require("fontresizr").API,
   -- If it sets false, you can still use
   -- require("fontresizr.api").
 },
}
M.dependencies={
 "abcdefg233/hcutil.nvim"
}
return M
```