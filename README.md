YOU ARE USING THIS DEVICE AT YOUR OWN RISK, HIH VOLTAGES ARE DANGEROUS
I TAKE NO RESPONSIBILITY FOR ANYTHING THAT HAPPENS TO YOU.

# DESCRIPTION
Time to get electrocuted !
BzztMachen shocks you with high voltage square wave every time you take damage in game.
For this to work you'll need:

1. [BzztMachen Software](https://github.com/Dankeatermidir/bzztmachen)
2. [BzztMachen Hardware](https://github.com/Dankeatermidir/BzztMachenHardware)
3. [BzztMachen Tables](https://github.com/Dankeatermidir/BzztTables) for [Cheat engine](https://github.com/cheat-engine/cheat-engine), or make yourself a plugin idk


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

# API
To trigger electric shock at given player, you need to specify **player_number** and **frequency** and make POST request to */machen* uri with given format:
"player_number,frequency", for example:
```
curl -d "0,50" http://bzztmachen.local:80/machen
```

**player_nymber** starts iterating from 0.

**frequency** is capped between 50 and 1000 by default. The higher frequency the lower power cuz impedance and stuff. 

**address** - bzztmachen uses mdns to avoid checking IP address every time, but it might be not resolved by mobile phones. On PC *bzztmachen.local* should be resolved without a problem.

### Return codes
*OK* - POST request succeded, electric shock was triggered.

*TOO SOON* - Given player was being shocked while request was made. No action taken.

*ERROR* - Request was probably in wrong format. No action taken.

### Light test
You can check if server is accessible an running without triggering shock by GET /version. It will return plain text answer with version and http methods.
```
curl bzztmachen.local/version
```




## Shock strength
As mentioned before the hiher frequency the lower is power. Writting right scaling function is crutial for immersive experience. Here are some exampes:

```
-- lua function I use for calculation of frequency in dark souls tables.
-- take previous hp state, current hp, maximum hp, player number
function calc_freq(p,c,m,player)
    --calculate hp loss
    x = (p-c)/m
    --if loss is higher then 10% calculate further
    if (x &gt; 0.01) then
    		--function lineary decreases frequency as hp loss rises
    		--at 50% it reaches 50 and doesn't increase further
        if (x &lt; 0.5) then
           x = math.floor((1030 - (1958 * x)))
           return player..","..tostring(x)
        else
           return player..",50"
	      end
	  --if hp loss is smaller then 10% give VERY light shock
    else
        return player..",2000"       
    end
end
```

```
// kotlin function to calculate frequency from shock level in percent
fun lvlFromPercent(percent: Int): Int {
    var lvl = percent % 100
    lvl = 100000 - 950 * lvl
    lvl /= 100
    return lvl
}
```
