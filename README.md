YOU ARE USING THIS DEVICE AT YOUR OWN RISK, HIH VOLTAGES ARE DANGEROUS
I TAKE NO RESPONSIBILITY FOR ANYTHING THAT HAPPENS TO YOU.

# DESCRIPTION
Time to get electrocuted !
BzztMachen shocks you with high voltage square wave every time you take damage in game.
For this to work you'll need:
[BzztMachen Software](https://github.com/Dankeatermidir/bzztmachen)
[BzztMachen Hardware](https://github.com/Dankeatermidir/BzztMachenHardware)
[BzztMachen Tables](https://github.com/Dankeatermidir/BzztTables) for [Cheat engine](https://github.com/cheat-engine/cheat-engine), or make yourself a plugin idk

BzztTables are basicly what allows Bzztmachen to know when you're taking damage.

They calculate how much hp have you lost and based on that they do HTTP POST in format {player}:{frequency} to http://bzztmachen.local. The higher frequency means less power, as project uses 50Hz transofers frequency is in range of 50 to 1000Hz.

Currently only Souls series Tables are available, but I will add more.

# How to use them

1. Start cheat engine
2. Select game process
3. Open Bzzt table
4. Select corresponding player
5. Press START !

If you're a linux user you can use [protonhax](https://github.com/jcnils/protonhax) to run cheat engine.

# How o make a BzztTable

You'll have to edit code so it finds right pointer for current health, max health and deaths values (change "ded" to 0 if there is do death counter) and relpace HPpointer, DEATHpointer, MAXHPpointer with right values.
I found souls pointers in [Grand Archaives CheatTables](https://github.com/The-Grand-Archives).
I'll make a detailed guide on how to make then If there is need.
