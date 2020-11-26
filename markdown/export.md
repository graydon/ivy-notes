[[keywords|Keyword]]: `export`

Exporting an [[action]] from an [[isolate]] marks it as a candidate for being called from its [[environment]].

Only exported actions (and their callees) are verified. If an action is not called (transitively) from an exported action, it is ignored.
