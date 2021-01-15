# unezQuake

unezQuake is a fork of the ezQuake client that aims to bring more permissive standards and features to Quakeworld to help the game evolve in a positive way.


## Context

ezQuake already offers an exceptional Quakeworld experience that unezQuake builds upon. unezQuake includes features meant to help inexperienced players learn better teamplay techniques as well as features meant to improve the gameplay experience for its users. unezQuake also includes a modern ruleset to explore and refine what is allowable for competitive play.

Quake servers and clients saw numerous changes for over a decade, but in recent years many changes or even experimentation have been frowned upon and even ridiculed within the broader community. unezQuake offers an alternative experience to those players and regions that are still interested in exploring and refining their gameplay experience. unezQuake remains compatible with ezQuake where practical and frequently tracks upstream changes in order to stay current.

While it shouldn't have to be said, we will say it anyways. Cheating in Quake is easy, so it is obviously not the intent of unezQuake to faciliate cheating or get away with small advantages. Quite the opposite, as unezQuake is developed in open source and makes public versioned releases.


## Features

Below is a list of the differences between unezQuake and ezQuake. This list may be out of date as unezQuake changes are often proposed upstream to ezQuake once tried, tested, and discussed.


### Teamplay 

The following features make the most sense in the context of teamplay gameplay modes, for example 2on2, 4on4, and clan arena.

---

* `cl_autoshownick`
  - [Video Preview](https://www.twitch.tv/bogojoker/clip/BlueSincereSmoothieKAPOW)
  - **Description:** Trigger shownick when teammate in crosshairs.
      - `0` - off
      - `1` - triggers `shownick`
      - `2` - triggers `shownick 1`
  - **Rationale:** Using shownick is beneficial to improve player communication and decision making in team games. Inexperienced players that do not know about or trigger shownick (after all it is a server command and not a client command) are the ones that would benefit from it the most. Making shownick easier to use will benefit all players.
  - **Solution:** Trigger shownick by looking at teammates instead of by triggering a bind. This still requires the player to use intentionality (look at a teammate) in order to benefit from the feature, it simply avoids a keypress.
  - **Implementation:** Approximately 20 times a second check if a teammate is in the crosshairs (e.g. `tp_msgpoint`), and if so trigger a shownick.
  - **Rulesets:** Allowed by all

---

* teaminlay family of commands (`teaminlay`, `teaminlay_*`, `scr_teaminlay_*`)
  - [Video Preview](https://www.twitch.tv/bogojoker/clip/ConsiderateIgnorantAirGuitarGingerPower)
  - **Description:** Client side teamoverlay with additional features.
      - `0` - off
      - `1` - to use inlay if overlay is not available
      - `2` - to force enabled
  - **Rationale:** Team overlay (teaminfo HUD) has been the norm for many regions of the world. However, teamoverlay is a feature of KTX that is not available in other server mods commonly played in those regions, namely Clan Arena. Additionally for users already using teamoverlay it can act as a better / more central point in the HUD for text based team play communication.
  - **Solution:** Provide a teamoverlay implementation independent of the server mod. Also consolidate and reward players using teamplay binds (`tp_msg_*`) by displaying the status in the inlay alongside player status.
  - **Implementation:** Formatted teamplay messages are sent via `say_team` and update the inlay instead of displaying like normal. Only for players with a team set. Many customization options are available.
  - **Rulesets:** `default` or `modern2020`


### General

The following features matter for all gameplay modes, not just teamplay, for example duel and ffa.

---

* `cl_smartspawn`
  - **Description:** When dead, `+attack` will spawn and immediately `-attack` to prevent weapon firing when spawning.
  - **Rationale:** A best practice is to spawn without shooting. Spawning by shooting has numerous disadvantages:
      - wastes ammo
      - prevents an aimed shot for at least one reload cycle
      - can accidentally hurt or kill a teammate (Murphy's Law)
      - may reveal your spawn location via bullet impacts on a far wall to an enemy that otherwise would not know
  - **Solution:** Add a command, like `cl_smartjump`, that better matches the player's intended behavior.
  - **Implementation:** When a player is dead, an attack command is treated as a jump instead of an attack.
  - **Rulesets:** Allowed by all

---

* `+pogo`/`-pogo`
  - **Description:** Unlimited jumping by holding down a jump key. Respects `cl_smartjump` just like `+jump`/`-jump`.
  - **Rationale:** Feedback received from new players in NA coming from other games. Gameplay testing did not show ways it could be exploited to achieve anything not already possible.
  - **Solution:** Add a new command to perform continuous jumping.
  - **Implementation:** Just like `+jump` with an implicit `-jump`/`+jump` after the player leaves the ground.
  - **Rulesets:** Allowed by all

---

* `scr_sbar_drawarmor666`
  - **Description:** Ability to draw true armor value when a player has pent instead of `666` if set to `0`.
  - **Rationale:** This is already possible in new hud (`hud_armor_pent_666`). Make it possible for old hud.
  - **Solution:** An equivalent command for old hud.
  - **Implementation:** When player has pent, do not change their armor to 666.
  - **Rulesets:** Allowed by all, matching `hud_armor_pent_666`.
  - **Note:** Accepted into ezQuake (as yet unreleased).


### Rulesets

Rulesets have always been a targeted list of excluded features. The list of excluded features in ezQuake's pre-existing rulesets has not been modified in unezQuake, and upstream changes are tracked. Put succiently, no features previously disallowed by an existing ruleset (qcon, smackdown, thunderdome, etc) are now allowed in unezQuake.

New uenzQuake features when developed are evaluated as to whether or not they should be disallowed by the older rulesets, as has been the case with features like inlay, to match the associated tournament conditions surrounding older rulesets. This matches with the historical and ongoing development in ezQuake, which often introduces new features and evaluates whether or not to exclude them from rulesets. For example the numerous feature differences between ezQuake 3.6 alpha and ezQuake 3.2.2 stable while both having ruleset "qcon"; unezQuake is the same.

---

* `modern2020` ruleset
  - Limit cl_rollangle to `10` to protect against exploitative usage
  - Allows `teaminlay` to encourage better teamplay
  - Allows `triggers` to encourage player creativity and new ideas
  - Currently evolving, may roll into a new ruleset like `modern2021`.

---

* ruleset changing
  - **Description:** Allow changing a ruleset without disconnecting / reconnecting.
  - **Rationale:** Having to disconnect to change rulesets is a waste of time.
  - **Solution:** Make changing rulesets work while connected.
  - **Implementation:** Changing rulesets has always been disallowed during a game. When a ruleset is changed a message is printed from the player for the new ruleset and a new `f_ruleset` is initiated by the player.

---

* ruleset reporting
  - **Description:** Ruleset reporting is now less cryptic.
  - **Rationale:** Players with offending settings did not know what rule they were breaking (what is `+msfi` or `-msfi`?)
  - **Solution:** Clearer output.
  - **Implementation:** Output words instead of cryptic letters.


### Other

* Remove `allow_f_system`
  - **Rationale:** Fake-nicking players commonly disallow `f_system` and attempt to ruin games.
  - **Solution:** Remove the command that helps trolls.


## Closing Thoughts

Why not contribute to ezQuake?

If you are asking this question then you likely mis-understand how open source works. We are contributing to ezQuake, you probably just don't see it as such. ezQuake has an incredible, but over-worked, over-burdened, and physically very distant maintainer! Bug fixes and smaller features that we've implemented can (and have) been cherry-picked to ezQuake/ktx/mvdsv in a matter of minutes. However, sometimes accepting changes can take much longer. As I write this there has been one of my changes sitting in the ezQuake queue for over a month with no feedback or resolution.

In some ways this is a chicken and egg problem. Some of the teamplay features we have worked on require multiple players to be using a client. As I write this the last official stable ezQuake release was Sept 2020 (which is before work on unezQuake ever even began). Waiting for inclusion into ezQuake and its accepted distribution channels amounts to effectively doing nothing at all. Instead we have opted to actually do something that will hopefully benefit both in the longer term. A fork, with more frequent versioned releases to test among a group of interested players.

The pressure of being the arbiter of what "is accepted into ezQuake" comes with undo stress on its maintainer. If we suggest some features and a vocal group is skeptical and pushes back nobody is happy (the maintainer, the contributors, or the skeptics). In fact, the concept of teaminlay was inspired by a comment on quakeworld.nu by the maintainer of ezQuake trying to think of ways to bridge the gap between skeptics and players interested in new features. The path we have chosen is to implement features, see what works, and contribute them when they are ready and iterate on feedback. It is at that point, which has not yet been reached, that we will see if this experiment works or not.

Further, as unezQuake's developers are more interested players than game developers our first take at feature implementations have not been perfect, and the features have been refined through use and play-testing. This is the normal development cycle for feature software development. I'd suggest that rather than being skeptical of the process, I'd urge you to be more understanding of how development works.

As stated at the beginning, we are interested in improving the gameplay experience and evolve the game in a positive way. If you think otherwise, perhaps you have pre-conceived ideas or emotions that may be clouding your judgement.


## Contact

Always consider contributing to ezQuake first. If you have an unezQuake specific concern you can contact @Dusty or @BogoJoker in [http://discord.usquake.world](). We welcome interesting client ideas you'd like to explore!
