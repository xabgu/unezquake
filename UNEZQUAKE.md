# unezQuake

unezQuake is a fork of the ezQuake client that aims to bring more permissive standards and features to Quakeworld to help the game evolve in a positive way.


## Context

ezQuake already offers an exceptional Quakeworld experience that unezQuake builds upon. unezQuake includes features meant to help inexperienced players learn better teamplay techniques as well as features meant to improve the gameplay experience for its users. unezQuake also includes a modern ruleset to explore and refine what is allowable for competitive play.

Quake servers and clients saw numerous changes for over a decade, but in recent years many changes or even experimentation have been frowned upon and even ridiculed within the broader community. unezQuake offers an alternative experience to those players and regions that are still interested in exploring and refining their gameplay experience. unezQuake remains compatible with ezQuake where practical and remains open source.

While it shouldn't have to be said, we will say it anyways. Cheating in Quake is easy, so it is obviously not the intent of unezQuake to faciliate cheating or get away with small advantages.


## Features

Below is a list of the differences between unezQuake and ezQuake. This list may be out of date changes are often proposed upstream to ezQuake once tried, tested, and discussed.


### Teamplay 

* `cl_autoshownick`
  - [Video Preview](https://www.twitch.tv/bogojoker/clip/BlueSincereSmoothieKAPOW)
  - **Description:** Trigger shownick when teammate in crosshairs.
      - `0` - off
      - `1` - triggers `shownick`
      - `2` - triggers `shownick 1`
  - **Rationale:** Using shownick is beneficial to improve player communication and decision making in team games. Inexperienced players that do not know about or trigger shownick (after all it is a server command and not a client command) are the ones that would benefit from it the most. Making shownick easier to use will benefit all players.
  - **Solution:** Trigger shownick by looking at teammates instead of by triggering a bind. This still requires the player to use intentionality (look at a teammate) in order to benefit from the feature, it simply avoids a keypress.
  - **Implementation:** Approximately 4 times a second check if a teammate is in the crosshairs (e.g. tp_msgpoint), and if so trigger a shownick.
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

---

* `scr_sbar_drawarmor666`
  - **Description:** Ability to draw true armor value when a player has pent instead of `666` if set to `0`.
  - **Rationale:** This is already possible in new hud (`hud_armor_pent_666`). Make it possible for old hud.
  - **Solution:** An equivalent command for old hud.
  - **Implementation:** When player has pent, do not change their armor to 666.
  - **Rulesets:** Allowed by all, matching `hud_armor_pent_666`.


### General

* `cl_smartspawn`
  - **Description:** When dead, `+attack` will spawn and immediately `-attack` to prevent weapon firing when spawning.
  - **Rationale:** A best practice is to spawn without shooting. Spawning by shooting has numerous disadvantages:
      - wastes ammo
      - prevents an aimed shot for at least one reload cycle
      - can accidentally hurt or kill a teammate (Murphy's Law)
      - may reveal your spawn location via bullet impacts on a far wall to an enemy that otherwise would not know
  - **Solution:** Add a command, that like `cl_smartjump`, that better matches the player's intented behavior.
  - **Implementation:** When a player is dead, an attack command is treated as a jump instead of an attack.
  - **Rulesets:** Allowed by all

---

* `+pogo`/`-pogo`
  - **Description:** Unlimited jumping by holding down a jump key. Respects `cl_smartjump` just like `+jump`/`-jump`.
  - **Rationale:** Feedback received from new players in NA coming from other games. Gameplay testing did not show ways it could be exploited to achieve anything not already possible.
  - **Solution:** Add a new command to perform continuous jumping.
  - **Implementation:** Just like `+jump` with an implicit `-jump`/`+jump` after the player leaves the ground.
  - **Rulesets:** Allowed by all


### Rulesets

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
  - **Rationale:** Players with offending settings did not know what rule they were (what is `+msfi` or `-msfi`?)
  - **Solution:** Clearer output.
  - **Implementation:** Output words instead of cryptic letters.


### Other

* Remove `allow_f_system`
  - **Rationale:** Fake-nicking players commonly disallow `f_system` and attempt to ruin games.
  - **Solution:** Remove the command that helps trolls.


## Contact

Always consider contributing to ezQuake first. If you have an unezQuake specific concern or idea you can contact @Dusty or @BogoJoker in [http://discord.usquake.world]().
