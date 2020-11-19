A transition relation is the "logical" view of an [[action]] (or at least a single [[statement]] within an action) that modifies [[state variable|state variables]].

Each state variable `V1, ..., Vn` representing the state *before* the action is duplicated into a "primed" variable `V1', ..., Vn'` representing the state *after* the action.

The transition relation is then a conjunction of equations defining each post-state `Vi'` in terms of the pre-state variables `V1,...,Vn`.

An action's verification condition is are formulated in terms of its transition relation.