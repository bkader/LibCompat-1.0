# LibCompat-1.0 (r30)

At first, I made this library as I was working on [Skada](https://github.com/bkader/Skada-WoTLK), but because I used it on few other addons as it helps me speed up the backporting process, i decided to share it as a standalone library.

## What is it?

Once embedded to your addon, it provdes some useful functions that are either not available for **WoTLK** or useful for other things.

## How to use it?

```lua
-- Embed is just like you do with Ace3 libraries for example
local MyAddOn = LibStub("AceAddon-3.0"):New("AddOnName", "LibCompat-1.0")

-- Then, you can use the available functions. For example:
local r, g, b, str = MyAddOn.GetClassColor("PALADIN")

-- or like I prefer to do
local GetClassColor = MyAddOn.GetClassColor
local r, g, b, str = GetClassColor("PALADIN")

```

## Available Mixins

### SafePack & SafeUnpack

Help you safely pack and unpack tables.

### tLength, tCopy, tAppendAll, tInvert, tIndexOf

Simple table helpers that do exactly what their names mean.

### WeakTable

If you are familiar with [LUA Weak Tables](https://www.lua.org/pil/17.html), this function helps you create weak tables on-the-go:

```lua
local t = MyAddOn.WeakTable() -- create a new table
local s = MyAddOn.WeakTable(t) -- reuse the table t
```

### Table & TablePool

the "Table" class allows you the reuse your tables instead of creating new ones. it has 2 functions `get` and `free`, using it is pretty easy:

```lua
local T = MyAddOn.Table
local myTable = T.get("MyTable") -- name your table.
-- after you finished working with your table, free it.
T.free("MyTable")
```

As for "TablePool", it creates a table pool system allowing you to not only reuse tables but also allows the GC to claim unused ones. It returns 2 functions: the first is used to create or get a table and the second is used to free/delete it.

```lua
local new, del = MyAddOn.TablePool()
-- to create a new table or reuse a deleted one:
local t = new()
-- after you're done, through it:
del(t)
-- note that "del" func returns "nil" just in case you want to unset the table
t = del(t)
```

### Math: Round, Square, Clamp, WithinRange, WithinRangeExclusive

- *Round*: rounds up a value.
- *Square*: returns the square of the value.
- *Clamp**: makes sure the returned value is always between the min and the max
- *WithinRange*: returns true if a value is less/greater than or equal the min/max value.
- *WithinRangeExclusive*: returns true if a value is less/greater than the min/max value.

### Group Utilities

- *IsInRaid*: returns true if you are in a raid group.
- *IsInGroup*: returns true if you are in a party or raid group.
- *GetNumGroupMembers*: returns the party or raid members count.
- *GetNumSubgroupMembers*: returns the part members count.
- *GetGroupTypeAndCount*: returns 3 values: group type, starting index and ending index.
- *UnitIterator*: allows you to loop through your group where the first arg is the unit and the second is the owner unit if the unit is a pet.
- *IsGroupDead*: returns true if all group members are dead.
- *IsGroupInCombat*: returns true if at least one of the group members is in combat.
- *GroupIterator*: allows you to loop your group members and to execute a callback for each.

```lua
-- GetGroupTypeAndCount is good if you want to loop through members
local prefix, minm, maxm = MyAddOn.GetGroupTypeAndCount()
if prefix then
	for i = minm, maxm do
		-- do the magic!
	end
end

-- this is the best way to loop through your group members
for unit, owner in MyAddOn.UnitIterator() do
	-- do the magic!
end

-- if however you want to execute a callback, better use GroupIterator:
MyAddOn.GroupIterator(function(unit, owner) end)
-- or
local function testCallback(arg1, arg2) return end
MyAddOn.GroupIterator(testCallback, "arg1", "arg2")
```

### Unit Utilities

- *GetUnitIdFromGUID*: attempts to return a unitId from the given GUID, useful if you want to run functions that require a valid unitId.
- *GetClassFromGUID*: returns the class of the given GUID.
- *GetCreatureId*: returns the creature ID from the given GUID.
- *GetUnitCreatureId*: returns the creature id of the gived unitId.
- *UnitHealthInfo*: returns data about the unit health: percent, current health and maximum health.
- *UnitPowerInfo*: returns data about the unit power: percent, current power and maximum power.
- *UnitFullName*: returns the unit full name, with the realm name if found.
- *UnitIsGroupLeader*: returns true if the given unit is the party or raid leader.
- *UnitIsGroupAssistant*: returns true if the given unit has raid assist.

### Class Color Utilities

- *GetClassColorsTable*: returns the table of class colors.
- *GetClassColorObj*: returns the color table for the given class, as a table.
- *GetClassColor*: returns the color table for the give class, unpacked.

### C_Timer Mimic

- *After*: simply executes something once after the give delay, returns nothing.
- *NewTimer*: creates a timer that's executed only once, returns the timer object.
- *NewTicker*: creates a repeating timer and returns its object.
- *CancelTimer*: cancels the give timer and returns nil.
- Timers created using **NewTicker** or **NewTicker** are cancellable timers using `timer:Cancel()` function.

### Specialization Utilities

- *GetSpecialization*: returns the spec id, spec name and talents points.
- *GetInspectSpecialization*: returns the given unit's specialization ID.
- *GetSpecializationRole*: returns the given unit's specialization role.
- *GetSpecializationInfo*: returns the given unit's specialization role.
- *GetSpecializationInfoByID*: returns information about the given specializations ID.
- *UnitGroupRolesAssigned*: returns the role assigned to the given unit.
- *GetGUIDRole*: returns the role assigned to the given GUID.
- *GetNumSpecializations*.
- *GetNumSpecGroups*.
- *GetNumUnspentTalents*.
- *GetActiveSpecGroup*.
- *SetActiveSpecGroup*.

### C_PvP

A really basic object that comes with few methods:

- *C_PvP.IsPvPMap*: returns true if the player is in a battleground or an arena.
- *C_PvP.IsBattleground*: returns true if the player is in a battleground.
- *C_PvP.IsArena*: returns true if the player is in an arena.

### Extra

- *PassClickToParent*
- *Mixin*
- *CreateFromMixins*
- *CreateAndInitFromMixin*
- *CreateObjectPool*
- *ColorMixin*
- *CreateColor*
- *WrapTextInColorCode*
- *IsPlayerSpell*
- *GetNumClasses*
- *GetClassInfo*
- *FramePoolMixin*
- *FramePool_Hide*
- *FramePool_HideAndClearAnchors*
- *CreateFramePool*
- *TexturePoolMixin*
- *TexturePool_Hide*
- *TexturePool_HideAndClearAnchors*
- *CreateTexturePool*
- *GetPhysicalScreenSize*