This page references the known limitations of NanosWorldTS, if there is a behavior that seems strange this might be helpful for you.

This page lists all the workarounds and the planned resolutions

## Using any and dealing with table return types that contains functions
### Problem
Most of the time during developement, when importing Lua packages you will want to type it as any like this:
```ts
Package.RequirePackage("nanos-world-weapons");

declare const NanosWorldWeapons: any;
[...]
char.PickUp(NanosWorldWeapons.AK47(new Vector(0, 0, 0)))
```

While this works great in most cases you will experience issues when trying to call functions from tables and typing as any.
For example the above code will generate this Lua
```lua
    char:PickUp(NanosWorldWeapons:AK47(Vector(0, 0, 0)))
```

This is not good because NanosWorldWeapons is a Table, not a class instance ! (and should be called with a `.` not a `:`)

This limitation comes from the [compiler itself](https://typescripttolua.github.io/docs/the-self-parameter#noimplicitself), using  `any` the compiler has no way to know what is the type and assume it is an Object.

### Workaround
You need to give a bit more context to the compiler so he knows what he deals with, using the `Quick&Dirty` approach we can narrow down a bit the type like this:
```ts
Package.RequirePackage("nanos-world-weapons");
declare const NanosWorldWeapons: {[k: string]: (...args: any) => any};
[...]
char.PickUp(NanosWorldWeapons.AK47(new Vector(0, 0, 0)))
```
As we explicitly told the compiler that the return type from `nanos-world-weapons` is an Table, indexed by a string (the weapon name) that returns a function, it does work:

Will correctly generate
```lua
    char:PickUp(NanosWorldWeapons.AK47(Vector(0, 0, 0)))
```

Of course, this is a quick and dirty approach, the more durable solution is this one, just for your knowledge:
```ts
declare const NanosWorldWeapons: {
    [weaponName: string]: (position?: Vector, rotation?: Rotator) => Pickable
};
```

### Planned resolution
There is no good solution for this limitation. Fixing it for this case will break it for the orther one (Lua package returning a Class instance).
As such, we think that the default behavior of `tstl` is the good choice, but we are open for debate if you think ortherwise !

## Extending NanosWorld Classes
### Problem
When you try to extend a NanosWorld class like this:
```ts
export class MyNPC extends Character {
     private position;
}
```

You will encounter a runtime error when trying the code
```
[2022-09-10 23:10:15] [error]    Lua Error on loading file 'nanos-ts-starter/Server/../dist/Server/./NPCs.lua':
        - [0] [nanos-ts-starter/Server/../dist/Server/./NPCs...]:24: attempt to index a nil value (field 'prototype')
        - [1] [nanos-ts-starter/Server/../dist/Server/./NPCs.lua]:24: in local '__TS__ClassExtends'
```

Here is the reason we think this happends:
- NanosWorld protects the metatables of the game Classes, however the compiler tries to rewrite the metatable.

### Workaround
Even if you cannot extend the base class, you can still use it as a member of your class and use it

```ts
export class NPC {
    private character: Character;
}
```

This solution is of course not ideal but you can implement most desired behaviors this way

### Planned resolution
Yes, this we want to solve this issue. We still have to talk about it with Syed. As such, there is still no planned resolution date because we have to figure out if it will be fixed in the game scripting API or on our end.

