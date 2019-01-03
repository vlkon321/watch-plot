# watch-plot
If you want a graph from your "watch" output.

### Some limitations
  - Listed command can output only whitespace separated number(s) - and it has to be ended with newline character (because every n interval it is saved to a file). It is not foolproof - if you feed it with a command that doesn't return numbers it will misbehave.
  - There are no labels which line is which. But they are at least ordered by columns (separate colours) sou you can make an educated guess
  - If you specify too large area of the graph (bigger than you terminal window) then the graphic will be messed up.
  - It is not a standalone application - it requires gnuplot (among others).

```
Usage: ./watch-plot [-hn:c:b:x:y:m:M:k]
	-h ..... this help
	-n ..... update interval (s)
	-c ..... command to run - has to return only number(s) - whitespace separated
	-b ..... buffer size (how many datapoints to store)
	-x ..... width of graph
	-y ..... height of graph
	-m ..... y-scale minimal value
	-M ..... y-scale maximal value
	-k ..... no cleanup of temp file with recorded data
	
	use Ctrl-C to exit
```

**Example 1** - monitor temperatures of your hardware
```
watch-plot -c "cat /sys/class/thermal/thermal_zone*/temp | tr '\n' '\t' | sed 's/\t$/\n/'" -n1.5 -m0 -b35
```

**Result 1** (it's not as cool here without the colours)
```
Every 1.5s:   cat /sys/class/thermal/thermal_zone*/temp | tr '\n' '\t' | sed 's/\t$/\n/'

                                                                               
  60000 +---------------------------------------------------------+            
        |  +      +       +      +  +-+  +      +       +      +  |   +-----+  
        |                -+         |  +                          |   +-----+  
  50000 |-+            ++ |         |  |                        +-|   +-----+  
        |              |  |         |   |                         |   +-----+  
        |             |   |         |   |                         |            
        |   +-++-++-++-++-++-++-++-++-++-++-++-++- +-++- +-++-++  |            
  40000 |-+           |    |       |    |         +     +       +-|            
        |             |    |    +  |     |                        |            
        |            |     |   + | |     |                        |            
  30000 |-+ +-++-++-++-++-++-++-++-++-++-++-++-++-++-++-++-++-+++-|            
        |   +-++-++-++-++-++-++-++-++-++-++-++-++-++-++-++-++-++  |            
        |                                                         |            
  20000 |-+                                                     +-|            
        |                                                         |            
        |                                                         |            
        |                                                         |            
  10000 |-+                                                     +-|            
        |                                                         |            
        |  +      +       +      +       +      +       +      +  |            
      0 +---------------------------------------------------------+            
           0      5       10     15      20     25      30     35              
```

**Example 2** - monitor wifi signal of wlp6s0 interface
```
watch-plot -c 'iwconfig wlp6s0 | sed -ne "s/^.*Signal level=//p" | sed -ne "s/ .*$//p"' -m -90 -M -60
```
**Result 2**
```
Every 1s:   iwconfig wlp6s0 | sed -ne "s/^.*Signal level=//p" | sed -ne "s/ .*$//p"

                                                                               
 -60 +------------------------------------------------------------+            
     |  +        +        +         +        +        +        +  |   +-----+  
     |                                                            |            
 -65 |-+                                                        +-|            
     |                                                            |            
     |                                                            |            
     |                                                            |            
 -70 |-+                                           -+-+-+-+-++-++-|            
     |    ++-+-+-+-+                  -+   +-+-++-+               |            
     |              |           +-+-++  + +                       |            
 -75 |-+            |           |        +                      +-|            
     |               +-+       |                                  |            
     |                  |      |                                  |            
 -80 |-+                +-+-+-+                                 +-|            
     |                                                            |            
     |                                                            |            
     |                                                            |            
 -85 |-+                                                        +-|            
     |                                                            |            
     |  +        +        +         +        +        +        +  |            
 -90 +------------------------------------------------------------+            
        0        5        10        15       20       25       30              
 ```
