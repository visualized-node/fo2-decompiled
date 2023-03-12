# fo2-decompiled
Full Lua Data Decompilation of the game "Flatout 2"
Made using bfs2unpacker

You can repack this using bfs2packer

here's the documentation for all functions that you can find in the .bed files

# Flatout 2 LUA API Documentation

# 1. Preliminaries

Let's start off by saying scripts are the major part of the game and the executable probably only instanciates and calls methods of classes.

For modding purposes I've made this huge list of API functions, ... to ease out development of mods

# 2. Documentation

## GLOBAL CONSTRUCTOR FUNCTIONS 

### enum(enumcontent: table) [CURRENTLY UNKNOWN]

```lua
function enum(enumcontent: table)
    -- there really isn't any documentation or any actual functionality known for the enum function, but presumably it just returns enumcontent

    return enumcontent
end
```

---

## GUI FUNCTIONS (windowfunctions.bed ~ Source, used all across bed scripts)

### SAFEPOS(x: number, y: number)

```lua
function SAFEPOS(x, y)
    local titlesafe_x = TITLESAFE_X or 0
    local titlesafe_y = TITLESAFE_Y or 0

    return { titlesafe_x + x, titlesafe_y + y}
end
```

A function that returns a `table` containing the provided x and y POS values with their corresponding titlesafe values summed to them

### POS(x: number, y: number)

```lua
function POS(x, y)
    return {x, y}
end
```

A constructor function to a `table` of 2d positional values (used in ui)
they could've used a class pfft

### SIZE(x: number, y: number)

```lua
function SIZE(x, y)
    return {x, y}
end
```

Function that returns exactly the same thing as POS() but uses the values for size

### SYSTEMFONT()

```lua
function SYSTEMFONT()
    return wm.GetResource("SystemFont")
end
```

Returns the system font of **FO2**

### GETFONT(name: string)

```lua
function GETFONT(name)
    return wm.GetResource(name)
end
```

Returns the selected font or nil if it didn't find it

### AddPos(p0: POSTYPE, p1: POSTYPE)

```lua
function AddPos(p0, p1)
    return POS(p0[1] + p1[1], p0[2] + p1[2])
end
```

Returns the sum of 2 POS values

### AddPosAndMakeSafe(p0: POSTYPE, p1: POSTYPE)

```lua
function AddPosAndMakeSafe(p0, p1)
    local AddPos = POS(p0[1] + p1[1], p0[2] + p1[2])

    return SAFEPOS(AddPos[1], AddPos[2])
end
```

Returns the SAFEPOS of two 2 POS values added together

### MakeSafePos(Pos: POSTYPE)

```lua
function MakeSafePos(Pos)
    return SAFEPOS(Pos[1], Pos[2])
end
```
## GUI ENUMS

### DIRECTION (enum)

```lua
DIRECTION = enum({
    UPDOWN,
    LEFTRIGHT
})
```

### WM (WINDOW MANAGER?) STUFF

```
wm._Menu = {}
wm._ActiveMenu{}


-- setmetatable(wm._Menus, { __mode = "k" })
```

these tables most likely contain (menu) all the menus and for activemenu it probably like expresses which menu is showing at that moment

let's go through all the module functions of wm

### LoadMenu(menuname: string, filename: string)

```lua
function wm.LoadMenu(menuname, filename)
    local MenuSandbox = Sandbox.GetSandbox(menuname)
    local SandboxEnv = MenuSandbox:GetEnvironment()

    SandboxEnv.menuself = menuname

    wm._Menu[meuname] = SandboxEnv

    return MenuSandbox:DoFile(filename)
end
```

A function that loads a menu using a `menuname` and returns what DoFile returns using the filename

### GetMenu(menuname: string)

```lua
function wm.GetMenu(menuname)
    local Menu = wm._Menu[menuname] or ERROR("wm.GetMenu: menu, 27h, %s27h, not found", menuname)

    return Menu
end
```

Returns a Menu or nil and throws an error if the menu doesn't exist
