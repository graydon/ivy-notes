A [[statement]] is a single step within an [[action]].

Multiple statements can be composed into a sequence by separating them with a semicolon (`;`). The semicolon is a sequential composition operator in Ivy, not a statement terminator. However, an extra semicolon is allowed before an ending brace (`}`) to make editing sequences of statements easier.

A statement may be one of the following:

  - [[call|calls]]
  - [[conditional|conditionals]]
  - [[loop|loops]]
  - [[assignment|assignments]] to [[state variable|state variables]]
  - local [[declared variable|declared variables]]
  - logical statements:
    - [[precondition|preconditions]]
    - [[postcondition|postconditions]]
    - [[assumption|assumptions]] 
    - [[assertion|assertions]]
