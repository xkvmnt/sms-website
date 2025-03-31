---
draft: false
title: 'Problem Statement & Solution'
weight: 3
ShowReadingTime: true
---
# **Problem Statement**

As part of the lecture, we were tasked with implementing both a locomotion and an object interaction technique using the virtual reality headsets.
The locomotion technique needed to allow users to traverse a parkour course, passing through several checkpoints while attempting to complete it in the shortest time possible. A good locomotion technique is considered intuitive for the user, causes little to no cybersickness, and provides an efficient means of travel. The object interaction technique, on the other hand, was required to enable users to select objects and manipulate their position and rotation along all axes within a 3D space.

# **My Solution**

Since we discussed in the lecture that most efficient locomotion techniques have already been developed, and as a game developer, I wanted to take a more creative and fun approach to the task. My goal was to implement a movement and interaction system inspired by a game while ensuring an engaging and enjoyable experience for the user. While searching for a suitable game to draw inspiration from, I recalled the Super Mario series by Nintendo, which I grew up playing a lot. After reviewing various titles in the series, I decided to recreate the movement mechanics from *Super Mario Sunshine*.
{{< figure align=center src="/images/general/SI_GCN_SuperMarioSunshine.jpg" width="500px" >}}

What sets Super Mario Sunshine apart from other games in the franchise is its introduction of the FLUDD (Flash Liquidizer Ultra Dousing Device), a water-powered jetpack-like device that enhances Mario’s basic movement. In addition to the classic abilities like jumping, double jumping, side flipping, and long jumping, the FLUDD allows Mario to perform new and unique moves. This moves are enabled by swapping between different nozzles which can by found in Boxes located in various parts of the world. These boxes contain upgrades like the Rocket Nozzle or Turbo Nozzle, which remain equipped until Mario picks up another nozzle or leaves the area. Some areas are designed around specific nozzles, requiring players to experiment with movement strategies to progress.

**Standard Spray Nozzle:** The default nozzle, allowing Mario to spray water in front of him to clean up goop, push objects, or interact with the environment.
{{< figure align=center src="/images/general/Mario_and_FLUDD_SMS.png" width="300px" >}}
**Hover Nozzle:** Enables Mario to hover for a short duration, giving him better air control and allowing for precise landings.
{{< figure align=center src="/images/general/Mario_and_Hover_Nozzle_SMS_2.png" width="300px" >}}
**Rocket Nozzle:** Charges up and launches Mario high into the air, reaching platforms that would otherwise be inaccessible.
{{< figure align=center src="/images/general/Mario_and_Rocket_Nozzle_SMS.png" width="300px" >}}
**Turbo Nozzle:** Grants Mario an instant speed boost, allowing him to dash across land and even skim over water.
{{< figure align=center src="/images/general/Mario_and_Turbo_Nozzle_SMS.png" width="300px" >}}
Since FLUDD is powered by water, using its abilities consumes water from a limited tank displayed on the HUD. If the tank runs out, FLUDD becomes nonfunctional until it is refilled. Water can be replenished by:

**1.** Standing in a body of water (e.g., a lake, ocean, or fountain) and holding the refill button.

**2.** Jumping into a deep water source, which automatically refills the tank.

**3.** Collecting water bottles or specific interactive objects scattered throughout levels.


    
{{< figure align=center src="/images/general/FLUDD_refill_Super_Mario_Sunshine_tutorial.webp" width="300px" >}}
The core idea behind my locomotion technique is to allow the user to switch between the Rocket, Turbo, and Hover Nozzles, mimicking their unique abilities to traverse the parkour. To complement this system, the course would be modified to require users to swap between the different nozzles strategically, ensuring they utilize all of them throughout the challenge.

For the interaction technique, a spraying nozzle would be implemented, enabling the user to interact with specific objects by spraying them. This mechanic allows for manipulation of an object's position and rotation, adding an interactive layer to the experience.


