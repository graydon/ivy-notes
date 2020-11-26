IVy assumes that every program is embedded within some **environment** with certain characteristics:
  - It may spontaneously make calls to [[export|exported]] [[action|actions]], in any order.
  - It will [[guarantee]] any [[precondition]] of those actions, but not by any particular sequence of calls or manipulation of state variables -- it is simply assumed within called actions, like any [[precondition]], that the environment **did** meet the precondition.
  - It provides any [[parameter|parameters]] and [[import|imported]] [[action|actions]].