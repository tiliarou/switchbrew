Notes from an outside-of-the-scene developer:

FS Access Control is all one giant ulong flag. Byte 1 says the
application is allowed to access its own data, set it to zero to watch
everything break. The last byte means full file permission. The byte
before that means it can access debug devices, this would allow (in
theory) access to debug gamecards/hardware from retail units. System
updates have the 17th and 21st bit set, this may mean that's providing
full control for modifying system related files. Some updates just use
the full permission bit (last bit).

For the sake of homebrew, we should just ignore this and flag the first
bit and the last two bits of the flag. I don't think anything else could
possibly matter. Maybe even flagging the first is pointless but I
haven't fuzzed this enough to check what breaks.
--[FrasierCrane](User:FrasierCrane "wikilink")
([talk](User%20talk:FrasierCrane.md "wikilink")) 12:08, 10 May 2018
(CDT)
