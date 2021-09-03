# sys-netcheat

## Don't use this for online games! It'll ruin the experience for others and will probably get your switch banned in the process!

## Warning:

For some reason this may result in installed games no longer launching if this is used in combination with sigpatches.  
If that happens running the 'Delete Common Ticket' option from Tinfoil and then (force) rebooting fixes it **sometimes**.

In at least one case running this with ReiNX (with sigpatches) resulted in all games on the storage being permanently corrupted!  
Be **very** careful when trying out weird configurations. In any case you should (not just for this) make a nand-backup so you can restore from somewhere if things go south.

I am in no way responsible for any damage that may or may not happen to your switch!

---

The original version of this seems to no longer be in development, so I forked it to get it working again and add more features.

This is an open-source cheat-engine for the nintendo switch.

It requires a hacked switch (recommended configuration is current Atmosphere, not tested on other CFWs.).

To use this, download the ZIP from the latest [release](https://github.com/therealmck/sys-netcheat/releases/) and extract it to the root of your SD Card.

You can also download the NSP and install it manually. Place the NSP in /atmosphere/contents/430000000000000A and rename it to exefs.nsp. Then, make a new folder called "flags", and create a file in there called "boot2.flag".

After installing simply boot your switch, start a game/homebrew and run

```
nc IP_OF_YOUR_SWITCH 5555
```

in the terminal on your computer. If you don't have netcat installed, install it with `sudo apt-get install netcat`. If you're on Windows, use cygwin or WSL.

You can run "help" to see a list of commands. Here's an example of how to use it:

I'll show you how to change the number of bananas in botw (the values will be different for you!)

Right now I have `353` Mighty Bananas
```
> ssearch u32 353
Got a hit at c7581d9cc!
Got a hit at c758240a8!
Got a hit at c758279b8!
... (about 500 lines)
Got a hit at 345d1b2a84!
Got a hit at 346d9b9070!
Got a hit at 346dcfb138!
```
This isn't very helpful. I'll eat one banana in order to see which of those hits is the one I want.
```
> csearch 352
Got a hit at 33bb64a888!
Got a hit at 344d109b10!
Got a hit at 344dd0c050!
Got a hit at 3456a577d8!
```
This is a lot better. If there are stil too many hits you should repeat the `csearch` until you find what you want.

Now we'll poke the values to find the one that we actually want and check if any change occured (in zelda closing and reopening the inventory is necessary in order to see the change).

```
> pokesearch 500
```

This pokes every address in the last search. Be very careful with this if there's a lot of addresses in the search.

You can also poke addresses individually, e.g:

```
> poke 3456a577d8 u32 500
```

![screeshot](/screenshot.jpg?raw=true)

We want to make sure that we never suffer from a lack of bananas again! So we'll freeze the value!

```
> afreeze 3456a577d8 u32 500
> lfreeze
0) 3456a577d8 (u32) = 500
```

If we want to unfreeze the value we just need to run

```
> dfreeze 0
```

And the value will be unfrozen.

---

# Troubleshooting

## 'nc' is not recognized as an internal or external command, operable program or batch file.
You are trying to use netcat through the Windows command prompt. Install cygwin or WSL to use a Linux command line.

## Error code f401
This means something else is using the debugger. Disable any cheat programs use have installed on your Switch, and try launching the game while holding L to disable Atmosphere's built-in cheat system.

## Netcat fails silently
This means it couldn't connect. Make sure your syntax is correct, that sys-netcheat is properly installed, and that your PC and Switch are on the same network.