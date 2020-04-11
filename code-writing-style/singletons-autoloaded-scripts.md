# Singletons / Autoloaded scripts

Singleton of events \(Signals\)

```text
Events.gd

signal party_walk_started(msg)
signal party_walk_finished(msg)

...

signal dialog_system_proceed_pressed(msg)
signal dialog_system_cancel_pressed(msg)

...

signal battle_started(msg)
signal battle_finished(msg)
```

This singleton’s only purpose is to lists signals that can be emitted and connected to. A “deeply” nested node like `$Game/Party/Godette/Walk` could then emit the appropriate signal directly using the global `Events` node:

`Events.emit_signal("party_walk_started", {destination = destination})`. Other nested nodes could connect to these signals: `Events.connect("party_walk_started", self, "_on_Party_walk_started")` etc.

This way, we can encapsulate signal connections in the related nodes instead of managing them in the code of some parent script like `Game.`

\`\`

\`\`

