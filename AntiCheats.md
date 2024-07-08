Project Zomboid Anticheats:

There are 24 Anti-Cheat Types.


Type 1: Hitting Players/Vehicles
This check makes sure you don't hurt someone that you shouldn't be able to, specifically, it will fail if any of the following is correct:
- The victim is in god-mode.
- PVP is disabled in Server Options.
- Either the victim or the attacker have safety enabled (when the safety system is enabled in Server Options).
- Either the victim or the attacker are in a non-PVP zone.
- The victim or the attacker are in the same faction, and faction PVP is disabled.

Type 2: Speed Check
A simple, raw speed check that verifies that the player or their vehicle is not going too fast. For vehicles, it uses the speed limit in the server options, and for players, it uses an hardcoded value of 10.

Type 3: Distance Checks
Only runs when a player hits another player, a vehicle or a zombie. Makes sure you don't try to hit something from too far away.

Type 4: Damage Checks
Verifies that the player does not cause damage above a defined level.

Type 5: Zombie Owner Check
In the `ZombieHitPlayerPacket` packet, verifies that the player sending the packet is the network owner of the zombie, as long as the network owner of the zombie has not changed in 2000ms. This check also only kicks the player if the player is marked as suspicious, and only adds an infraction point otherwise.

Type 6: Zombie Target Check
In the `ZombieHitPlayerPacket` packet, verifies that the player sending the packet is the target of the packet. The player is kicked otherwise.

Type 7: Safehouse Authorization Check
In the `ZombieHitPlayerPacket` packet, kicks the player if none of the following conditions apply:
- The safehouse has no owner (set to null or empty string).
- The player is the owner of the safehouse.
- The player is a moderator.

Type 8: Packet Authorization Check
Checks if a player is "authorized" to send a certain packet based on the action that they represent, unless the player is in debug mode or privileged.
Individual packets can be set to one of three policies: log/kick/ban. However, at the current time, it appears that all packet types are set to kick. This may change in the future.

Type 9: XP Growth Check
A check that runs every-time update is called and checks if the player's XP growth since the last update exceeds the following threshold:

Type 10: Unknown Packet Check
Simply kicks the player if UdpEngine receives an unknown packet from the player.

Type 11: Unused
This isn't used anywhere in the entirety of the game's code-base, unless I'm missing something major.

Type 12: Access-Level Abuse
Kicks the player if they are spoofing/abusing their accessLevel.

Type 13: Invalid PacketType Check
Kicks the player if the game receives a packet with an invalid PacketType that gets parsed to null.
If this happens to you, you're probably sending malformed packets.

Type 14: Invalid player in AddXp
Closes the connection sending the player if they don't have a valid player in the AddXp packet. Check UdpConnection.havePlayer for details.

Type 15: XP Limit in AddXp
Kicks the player if they try sending an AddXp packet with an amount above a threshold

Type 16: Starting Fires when Disabled
One of the checks in the StartFire packet, kicking the player if they send the packet while ServerOptions.NoFire is set to true.
Note that this check does not run if the smoke field on the packet is set to true.

Type 17: Valid Fire Squares
Inside of the StartFire packet, this check verifies that the square specified is fit for a fire (e.g. not water). For details, see IsoFire.CanAddFire, which includes a complete list of the checks on a specific square. The player is kicked if this check fails.
This check also does not run if the smoke field on the packet is set to true.

Type 18: Valid Smoke Squares
Another check in the StartFire packet, verifying that the square specified is fit for smoke (somewhat similar checks). For details, see IsoFire.CanAddSmoke. The player is kicked if this check fails.
This check only runs when the smoke field on the packet is set to true.

Type 19: Valid Safehouse for Syncing
A check in the SyncSafehousePacket packet, that assures that the player sending the packet has a safehouse in the first place when they send it. The player is kicked otherwise.

Type 20: Invalid Safehouse Details
A check in the SyncSafehousePacket packet, that only runs if the player sending the packet is a user (unprivileged) and either the weight or height exceed the threshold:

Type 21: Checksum
Kicks the player if they fail the checksum check in ValidatePacket.

Type 22: Checksum Time-out
Kicks the player if they don't respond to the checksum check in time. The timeout for the checksum check is: (in milliseconds)

Type 23: Time Multiplier Check
Kicks the player if their time multiplier is different than the one on the server when GameTime.receiveTimeSync is called.

Type 24: Time Multiplier Time-out
Kicks the player if they don't respond to the time multiplier check in time. The timeout for the time multiplier check is (in milliseconds)
