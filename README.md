# LibCompat-1.0

At first, I made this library as I was working on [Skada](https://github.com/bkader/Skada-WoTLK), but because I used it on few other addons as it helps me speed up the backporting process, it decided to make it a standalone library.

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

### tLength, tCopy, tAppendAll

Simple table helpers that do exactly what their names mean.

## WeakTable

If you are familiar with [LUA Weak Tables](https://www.lua.org/pil/17.html), this function helps you create weak tables on-the-go:

```lua
local t = MyAddOn.WeakTable() -- create a new table
local s = MyAddOn.WeakTable(t) -- reuse the table t
```

## newTable & delTable

Another [LUA Weak Tables](https://www.lua.org/pil/17.html) functions that use a table pool and allow you to recycle your tables.

```lua
local newTable, delTable = MyAddOn.newTable, MyAddOn.delTable
local t = newTable() -- that's all.
delTable(t) -- unless you return the table, once done with it recycle it.

```

### Math: Round, Square, Clamp, WithinRange, WithinRangeExclusive

- *Round*: rounds up a value.
- *Square*: returns the square of the value.
- *Clamp**: makes sure the returned value is always between the min and the max
- *WithinRange*: returns true if a value is less/greater than or equal the min/max value.
- *WithinRangeExclusive*: returns true if a value is less/greater than the min/max value.

## Group Utilities

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

## Unit Utilities

- *GetUnitIdFromGUID*: attempts to return a unitId from the given GUID, useful if you want to run functions that require a valid unitId.
- *GetClassFromGUID*: returns the class of the given GUID.
- *GetCreatureId*: returns the creature ID from the given GUID.
- *GetUnitCreatureId*: returns the creature id of the gived unitId.
- *UnitHealthInfo*: returns data about the unit health: percent, current health and maximum health.
- *UnitPowerInfo*: returns data about the unit power: percent, current power and maximum power.
- *UnitFullName*: returns the unit full name, with the realm name if found.
- *UnitIsGroupLeader*: returns true if the given unit is the party or raid leader.
- *UnitIsGroupAssistant*: returns true if the given unit has raid assist.

## Class Color Utilities

- *GetClassColorsTable*: returns the table of class colors.
- *GetClassColorObj*: returns the color table for the given class, as a table.
- *GetClassColor*: returns the color table for the give class, unpacked.

## C_Timer Mimic

- *After*: simply executes something once after the give delay, returns nothing.
- *NewTimer*: creates a timer that's executed only once, returns the timer object.
- *NewTicker*: creates a repeating timer and returns its object.
- *CancelAllTimers*: cancels all scheduled timers.

## Spell Utilities

- *GetSpellInfo*: does like the default api function, except it changes Auto Shot and Melee icons and provide extra environmental damage spells (3, 4, 5, 6, 7, 8).
- *GetSpellLink*: same as the default api function, except that it ignores custom spells (environmental).

## Specialization Utilities

- *GetSpecialization*: returns the spec id, spec name and talents points.
- *GetInspectSpecialization*: returns the given unit's specialization ID.
- *GetSpecializationRole*: returns the given unit's specialization role.
- *UnitGroupRolesAssigned*: returns the role assigned to the given unit.
- *GetGUIDRole*: returns the role assigned to the given GUID.

## C_PvP

A really basic object that comes with few methods:

- *C_PvP.IsPvPMap*: returns true if the player is in a battleground or an arena.
- *C_PvP.IsBattleground*: returns true if the player is in a battleground.
- *C_PvP.IsArena*: returns true if the player is in an arena.