---
draft: false
title: 'Implementation Details'
weight: 4
ShowReadingTime: true
---
**IMPORTANT NOTE:** Since I incorporated a few assets from the original game, it is important to clarify upfront that all rights to the assets and also any images used in this project are reserved by Nintendo. This project was created solely for study and learning purposes and will not be used for any commercial activities.

# **1. Locomotion Implementation** 

The initial idea for the locomotion technique was to implement the **Hover Nozzle** as the first movement ability. However, for this to work, the user first needed a way to gain height before activating the hover. To achieve this, I decided to implement a classic Mario-style jump.

To execute this, the code tracks the position of either controller. If one of the controllers moves above the position of the head-mounted display (HMD), a positive velocity is applied to the player's Rigidbody, propelling them into the air - mimicking the Mario jump gesture. Unity’s physics engine then naturally counteracts this movement with gravity, causing the player to slow down and eventually fall back to their initial position as a negative velocity is applied. 

{{< figure src="/images/code/jump.png" width="500px" >}} 

If the controller positions are then registered to be under the HMD again the ability to jump is then reset to prevent the player from spamming the jump action and gaining infinite height.

After implementing the jumping gesture, the next step was to develop the hovering mechanic. The code first checks if the player's Y-position is above zero - if so, hovering becomes available. if the player now presses both of the triggers on the controllers he is able to activate the hovering. 

For the hovering logic itself, the player's downward velocity is nullified. This effectively counteracts gravity, allowing the player to stay in the air. To control the direction of movement while hovering, the user can tilt both controllers. The code then calculates the average tilt of both controllers and applies the resulting directional force to the player's transform, adjusting movement relative to the HMD’s orientation. This allows the user to smoothly navigate the 3D world while hovering.

### **Turbo Nozzle**

Since the Hover Nozzle is primarily designed for hovering and does not provide significant forward momentum, the Turbo Nozzle was implemented as a way to move quickly while on the ground.

{{< figure src="/images/code/boost.png" width="500px" >}} 

The Turbo Nozzle operates similarly to the hovering mechanic but with some key differences. It only considers the left and right tilt of the controllers to determine the movement direction. Additionally, the Turbo Nozzle applies a higher movement speed to the player's transform, resulting in significantly faster and more dynamic movement across the ground.

### **Rocket Nozzle**

The Rocket Jump functions similarly to the jumping gesture, but without requiring a physical gesture. Instead, when the player holds both triggers, a timer starts counting. If the threshold value is surpassed, a powerful upward force is applied to the player's Rigidbody.

{{< figure src="/images/code/rocketjump.png" width="500px" >}} 

To differentiate the Rocket Jump from the standard jump, it applies significantly more force, launching the player much higher into the air.

### **Water Tank**

The water tank mechanic was implemented using an internal float variable, which is initially set to 100. Each time the user activates an ability, this value decreases accordingly. For all nozzles, the water level gradually decreases while in use. However, for the Rocket Jump, water consumption occurs in a burst, reducing the tank by 20 units per use. Before executing a Rocket Jump, the code checks whether the current water level is at least 20 to ensure the action can be performed.

Once the water level reaches 0, the player is unable to use any abilities until the tank is refilled. To replenish the water supply, the player must stand in a water source, restoring their ability to use the FLUDD nozzles. To implement this, a water pond prefab was created, consisting of a cube and a water surface prefab. This object was assigned the "waterSource" tag, allowing the code to recognize if the player is in a refillable area.

{{< figure src="/images/code/waterrefilling.png" width="350px" >}}

Using the OnTriggerStay function, the player's collider checks for contact with an object tagged as "waterSource". If the player remains within the water source, the water tank value gradually increases, allowing them to refill and continue using FLUDD’s abilities.

Several water ponds have been strategically placed around the course, allowing players to refill their water tank when needed. In case a player runs out of water and gets stuck, pressing the reset button will automatically respawn them in a water source, ensuring their water tank is refilled and they can continue playing without frustration.

After implementing the logic for all movement nozzles the ability to cycle through them has been added. Players can switch between nozzles by pressing the A-button on the right controller, allowing them to seamlessly swap abilities as needed.

{{< figure src="/images/code/cyclenozzles.png" width="600px" >}}

# **2. Interaction Implementation**

### **Spraying Nozzle**

For the interaction technique, I opted for a more fun and dynamic approach rather than a precise object manipulation implementation. Instead of allowing the player to precisely adjust an object's position and orientation, I focused on replicating the spraying nozzle from Super Mario Sunshine.

The spraying nozzle mechanic was implemented using a Line Renderer to simulate the arc of sprayed water. The parabolic trajectory of the water stream was achieved through interpolation and a parabola formula.

The function CalculateArcPoint determines the position of each point along the arc by linearly interpolating between the start and end positions while applying a parabolic offset to create the arch effect.
{{< figure src="/images/code/calculatearcpoint.png" width="500px" >}} 
The arc visualization is then generated using a for loop that iterates through a set number of points, calculating each one using CalculateArcPoint, and updating the Line Renderer.
{{< figure src="/images/code/generatearcpoints.png" width="500px" >}} 
To actually move an object the water spray applies force to the object it hits. If the raycast detects a collision with an object that has a Rigidbody, force is applied in the direction of the spray, resulting in the object being pushed.
{{< figure src="/images/code/spray_hit.png" width="500px" >}} 

### **Interaction Tasks**

To create fun and engaging interaction tasks, the original tasks on the map were redesigned. Using ProBuilder, I modeled various structures within Unity to serve as interactive elements creating small mini games the player has to solve to progress through the course. The first task features a giant cube marked with a red cross, indicating that the player must push it using the spraying nozzle. The cube was placed inside a tunnel structure, blocking the player’s path. By successfully pushing the cube forward, the tunnel opens up, allowing the player to proceed through the course.
{{< figure align=center src="/images/implementation/interaction1.png" width="500px" >}} 

The second interaction task involves a giant wooden board positioned near a large gap. To progress, the player must use the spraying nozzle to push the board over the gap, creating a makeshift bridge that allows them to cross safely.
{{< figure align=center src="/images/implementation/interaction2.png" width="500px" >}}
For the third task I designed a small obstacle course for the player to push a ball through using the spraying nozzle. At the end of the course, there is a pit - if the player successfully guides the ball into the pit, the wall blocking the final checkpoint disappears, allowing them to pass through and complete the parkour course.
{{< figure align=center src="/images/implementation/interaction3.png" width="500px" >}} 

# **3. UI Redesign**

To further mimic the look and feel of Super Mario Sunshine, I also redesigned the UI. For this, I downloaded UI sprites from the [Mario Wiki](https://www.mariowiki.com/Super_Mario_Sunshine), incorporating them into the project to create a more authentic visual experience. Specifically, I used the coin and red coin sprites, tilting them slightly along with the timer to match their appearance in the original game. Additionally, I incorporated sprites for the different nozzles, providing users with visual feedback when cycling through them, making it clear which nozzle is currently equipped. I also included the water tank sprite, pairing it with a visual representation of the internal float value. This ensures that players can easily monitor their remaining water supply and know when they need to refill.
{{< figure align=center src="/images/implementation/ui.png" width="500px" >}} 
# **4. Miscellaneous**

Using ProBuilder, I recreated the iconic green pipes seen in almost every Mario game. These pipes serve as obstacles that players must overcome by utilizing the various FLUDD abilities, adding a classic platforming challenge to the experience.
{{< figure align=center src="/images/implementation/obstacle1.png" width="500px" >}}
To further emphasize the platforming aspect, several map adjustments were made. Some platforms were elevated, requiring players to use different FLUDD nozzles to navigate the environment. This design choice encourages varied gameplay and ensures that players engage with all available movement mechanics.

{{< figure align=center src="/images/implementation/obstacle2.png" width="500px" >}}


Since every Mario level features eight red coins that can be collected to earn an extra Star or Shine, I added red coins throughout the course as a fun side objective for the player. To implement this, I duplicated the existing coin prefab and modified it by changing the material color of both the coin itself and the glowing effect surrounding it to red.
{{< figure align=center src="/images/implementation/obstacle3.png" width="500px" >}}
Additionally, to further enhance the authentic Mario atmosphere, I replaced the background music with the Super Mario Sunshine OST, making the experience feel even more like a real Mario level.