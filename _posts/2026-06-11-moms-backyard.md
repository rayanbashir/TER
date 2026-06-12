---
title: "Creating an RNG ore mining game using Processing"
date: 2026-06-11
categories:
tags:
---

This project is an RNG‑based ore mining game programmed in Processing by Kaysan and I. Over 5 days we added new features, improved the UI, and expanded the game systems by using important code concepts like selection structures, loops, functions, etc.. These logs show how the game developed step by step.

NOTE: These logs only contain four days because I missed one for cricket.

---

<h3>Day 1</h3>

```ruby
boolean inventoryOpen = false;

void setup() {
  size(800, 600);
}

void draw() {
  if (!inventoryOpen) {
    fill(0, 255, 0);
    rect(100, 100, 200, 100);
    fill(0);
    text("Inventory", 150, 150, 200, 100);
    if (checkObjectPressed(100, 300, 100, 200)) {
      drawInventory();
    }
  }
}

boolean checkObjectPressed(int x1, int x2, int y1, int y2) {
  if (mouseX >= x1 && mouseX <= x2 && mouseY >= y1 && mouseY <= y2) {
    fill(255, 126);
    rect(x1, y1, x2 - x1, y2 - y1);
    noTint();
    if (mousePressed) {
      return true;
    }
  }
  return false;
}

void drawInventory() {
  inventoryOpen = true;
  fill(160);
  rect(300, 20, 480, 540);

  for (int i = 0; i<25; i++) {
    if (i < 5) rect(300+(i%5)*(480/5), 20, 480/5, 560/5);
    else if (i < 10) rect(300+(i%5)*(480/5), 20+(560/5), 480/5, 560/5);
    else if (i < 15) rect(300+(i%5)*(480/5), 20+(2*560/5), 480/5, 560/5);
    else if (i < 20) rect(300+(i%5)*(480/5), 20+(3*560/5), 480/5, 560/5);
    else if (i < 25) rect(300+(i%5)*(480/5), 20+(4*560/5), 480/5, 560/5);
  }

  rect(20, 20, 280, 560);
}
```
For day 1, I started by creating a function for object clicks, because I knew that that would be one of the most re-used and important functions of the game. Every button in the game relies on the checkObjctPressed function, so I had to make it adaptable and reusable for each button. The function takes 4 parameters: x1, x2, y1, and y2. This way, the function can be called for any button in any position or size. I also made the function return a boolean value. The boolean it returns acts as a trigger to activate whatever a button is supposed to do. Each button will have its own action, so by making the trigger simple, it allows me to easily call the correct action for the clicked button.

I also used a selection structure within the function to:
1. Know if the mouse is in the right spot when it clicks
2. Create a transparent box on the button to show that the user is hovering over a button.

In the drawInventory function, I used a for loop to position all 25 inventory slots automatically. This saved me so much time and made the code look cleaner.

---

<h3>Day 2</h3>

```ruby
//CODE IS SHORTENED TO HIGHLIGHT CERTAIN CODE

void draw(){
  if (start){
    image(background, 0, 0, 800, 600);
    startMenu();
  } else{
    if (!inventoryOpen && !shopOpen) {
      gameMenu();
    } else if (inventoryOpen) {
      drawInventory();
    } else if (shopOpen) {
      drawShop();
    }
  }
}

void drawShop() {
  fill(160);
  rect(50, 100, 700, 400);


  rect(75, 125, 200, 250);
  rect(300, 125, 200, 250);
  rect(525, 125, 200, 250);


  textAlign(CENTER);
  fill(0);
  textSize(25);
  text("Tool Upgrade", 75, 135, 200, 250);
  text("Tip Upgrade", 300, 135, 200, 250);
  text("Depth Upgrade", 525, 135, 200, 250);

  fill(255, 0, 0);
  rect(40, 90, 35, 35);
  fill(255);
  textSize(30);
  textAlign(CENTER, CENTER);
  text("X", 40, 90, 35, 35);
  if (checkObjectPressed(40, 90, 35, 35)) {
    shopOpen = false;
  }
}

void inventoryButton(int x, int y, int button_width, int button_height) {
  imageMode(CORNER);
  image(inventoryButton, x, y, button_width, button_height); //button
  if (checkObjectPressed(x, y, button_width, button_height)) {
    inventoryOpen = true;
  }
}

void shopButton(int x, int y, int button_width, int button_height) {
  imageMode(CORNER);
  image(shopButton, x, y, button_width, button_height);
  if (checkObjectPressed(x, y, button_width, button_height)) {
    shopOpen = true;
  }
}

void mineButton(int x, int y, int button_width, int button_height) {
  imageMode(CORNER);
  image(mineButton, x, y, button_width, button_height);
  if (checkObjectPressed(x, y, button_width, button_height)) {
    mineOre();
  }
}
```
In day 2, I combined my UI code with Kaysan's to start implementing real functionality to the inventory.

I added some if statements in the draw function to check what was currently opened (i.e. shop, inventory). With this check, I was able to prevent two UI screens being opened at the same time, which would mess with the player experience.

I also worked on adding more functions such as inventoryButton, drawShop, mineButton, and shopButton. Although the code within the functions isn't going to be reused, the functions make it easy to organize our code and to logically decide when we should use each.

---

<h3>Day 3</h3>

```ruby
//CODE IS SHORTENED AGAIN


void drawInventoryOreSlot(int x, int y, int oreID) {
      fill(160);
      rect(x, y, 480/5, 560/5);

      if (oreInv.getInt(oreID, "isFound") == 0){
        fill(0);
        textAlign(CENTER);
        textSize(15);
        text("???", x, y+10, 480/5, 560/5);
        tint(0);
        image(oreImages[oreID], x+10, y+40, 480/5-10, 560/5-60);
        return;
      }

      if (checkObjectPressed(x, y, 480/5, 560/5)) {
        selectedOreID = oreID;
      }
      fill(0);
      textAlign(CENTER);
      textSize(15);
      text(oreTable.getString(oreID, "name"), x, y+10, 480/5, 560/5);
      imageMode(CORNER);
      tint(255);
      image(oreImages[oreID], x+10, y+40, 480/5-10, 560/5-60);

}

void drawInventory() {
  fill(160);
  rect(300, 20, 480, 540);

  for (int i = 0; i<25; i++) {
    if (i < 5) {
      drawInventoryOreSlot(300+(i%5)*(480/5), 20, i);
    } else if (i < 10) {
      drawInventoryOreSlot(300+(i%5)*(480/5), 20+(560/5), i);
    } else if (i < 15) {
      drawInventoryOreSlot(300+(i%5)*(480/5), 20+(2*560/5), i);
    } else if (i < 20) {
      drawInventoryOreSlot(300+(i%5)*(480/5), 20+(3*560/5), i);
    } else if (i < 25) {
      drawInventoryOreSlot(300+(i%5)*(480/5), 20+(4*560/5), i);
    }

    if (selectedOreID == -1){
      fill(160);
      rect(20, 20, 280, 560); //blank side bar
    } else {
      fill(160);
      rect(20, 20, 280, 560);
      textSize(40);
      fill(0);
      textAlign(CENTER);
      text(oreTable.getString(selectedOreID, "name"), 20, 80, 280, 100);

      tint(255);
      image(oreImages[selectedOreID], 20, 150, 280, 200);

      textSize(22);
      text(oreTable.getString(selectedOreID, "odds"), 20, 350, 280, 400);

      text(oreTable.getString(selectedOreID, "description"), 30, 400, 270, 550);

      textAlign(LEFT);
      textSize(20);
      text("Quantity: " + oreInv.getInt(selectedOreID, "quantity"), 30, 535, 270, 560);

      button(30, 500, 100, 30, "Sell");
      if (checkObjectPressed(30, 500, 100, 30)) {
        sellOre(selectedOreID);
      }
   
   
    }
    fill(255, 0, 0);
    rect(30, 30, 35, 35);
    fill(255);
    textSize(30);
    textAlign(CENTER, CENTER);
    text("X", 30, 30, 35, 35);
    if (checkObjectPressed(30, 30, 35, 35)) {
      inventoryOpen = false;
    }

    button(150, 30, 100, 30, "Sell All");
    if (checkObjectPressed(150, 30, 100, 30)) {
      sellAllOre();
    }
  }
}

void button(int x, int y, int button_width, int button_height, String text) {
  fill(200, 0, 0);
  rect(x, y, button_width, button_height);
  fill(0);
  textAlign(CENTER, CENTER);
  textSize(30);
  text(text, x, y, button_width, button_height);
}

```

On day 3, I focused on making our inventory function. Previously, I only programmed the opening and closing of the inventory as well as the position of each slot. Today, I added each ore's name and image to every slot and programmed a way to click a slot and look at the ore's details in the side bar.

Since I already has the for loop set up to make things easy, I was able to set up the ore name and image quite easily with the help of Kaysan's csv file/table. I purposely set the for loop to start at 0 and end at 24 to match all the ore IDs we set up. This made is super easy to call the correct variable for each slot. 

I also reused the checkObjectPressed function for the slots. Clicking a slot selects that ore for the side panel, which contains a bigger image and more details about the selected ore.

I set up a variable to keep track of the currently selected ore. I set the clicked slot's oreID to the selectedOreID variable. Now that we have the ID, once again, it becomes really easy to call and display the correct information on the side panel since we have it all in the csv/table.

I coded a generic button function for the "sell" and "sell all" buttons in the inventory. This made it quick to repeat the button making process. Kaysan programmed the sell and sell all functions already, so it was simple linking the functions to the button triggers since we already had the logic set up.

---

<h3>Day 4</h3>

```ruby

String[] toolUpgradeNames = new String[]{"p", "Wood Pickaxe", "Stone Pickaxe", "Iron Pickaxe", "Diamond Pickaxe"};
int[] toolUpgradePrices = new int[]{0, 100, 500, 1000, 1500, 0};
PImage[] toolUpgradeImages = new PImage[4];

String[] tipUpgradeNames = new String[]{"p", "Tier 1", "Tier 2", "Tier 3", "Tier 4", "Tier 5", "Tier 6"};
int[] tipUpgradePrices = new int[]{0, 150, 300, 500, 600, 0};

String[] depthUpgradeNames = new String[]{"Underground", "Caverns", "Depths", "Deep Dark", "Underworld", "Earth's Core"};
int[] depthUpgradePrices = new int[]{0, 400, 600, 1000, 1200, 0};
int[] depthUpgradeColors = new int[]{#ffffff, #ffffff, #3d2b15, #3f4a61, #1f1f1f, #d10808, #6b5f4f};

void setup(){
  toolUpgradeImages[0] = loadImage("data/Images/Tools/woodPick.png");
  toolUpgradeImages[1] = loadImage("data/Images/Tools/stonePick.png");
  toolUpgradeImages[2] = loadImage("data/Images/Tools/ironPick.png");
  toolUpgradeImages[3] = loadImage("data/Images/Tools/diamondPick.png");
}

void draw(){
  if (start){
    tint(depthUpgradeColors[currentDepth]);
    image(background, 0, 0, 800, 600);
    tint(255);
    startMenu();
    if (oreAnim){
      if (pickY <= 240){
        pickY += 3;
        pickX += 1;
        delay(int(5/mineSpeed));
       
      }
      imageMode(CENTER);
      image(glow, width/2, oreY, 75, 75);
      image(oreImages[currentOreID], width/2, oreY, 75, 75);
      imageMode(CORNER);
      image(rock, 250, 200, 300, 200);
      oreY -= mineSpeed*3;
      if (oreY <= maxOreHeight) {
          delay((int)(2000/mineSpeed));
          oreY = height/2;
          pickY = 175;
          pickX = 145;
          oreAnim = false;
      }
    }
  }
}


}

void drawShop() {
  fill(160);
  rectMode(CORNER);
  rect(50, 100, 700, 400);


  rect(75, 125, 200, 250);
  rect(300, 125, 200, 250);
  rect(525, 125, 200, 250);


  textAlign(CENTER);
  fill(0);
  textSize(25);
  text("Tool Upgrade", 75, 135, 200, 250);
  text("Tip Upgrade", 300, 135, 200, 250);
  text("Depth Upgrade", 525, 135, 200, 250);

  fill(255, 0, 0);
  rect(40, 90, 35, 35);
  fill(255);
  textSize(30);
  textAlign(CENTER, CENTER);
  text("X", 40, 90, 35, 35);

  fill(0);
  text("Level: " + playerData.getInt(1, "value"), 75, 200, 200, 250);
  text("Level: " + playerData.getInt(2, "value"), 300, 200, 200, 250);
  text("Level: " + playerData.getInt(3, "value"), 525, 200, 200, 250);

  fill(0);
  text("Tier: " + toolUpgradeNames[playerData.getInt(1, "value")], 75, 300, 200, 250);
  text("Tier: " + tipUpgradeNames[playerData.getInt(2, "value")], 300, 300, 200, 250);
  text("Tier: " + depthUpgradeNames[playerData.getInt(3, "value")], 525, 300, 200, 250);

  rectMode(CORNER);
  fill(180, 20, 20);
  rect(100, 200, 150, 100);
  fill(255);
  text("Price: " + toolUpgradePrices[playerData.getInt(1, "value")] + "\n Buy", 75, 125, 200, 250);

  fill(180, 20, 20);
  rect(325, 200, 150, 100);
  fill(255);
  text("Price: " + tipUpgradePrices[playerData.getInt(2, "value")] + "\n Buy", 300, 125, 200, 250);
 
  fill(180, 20, 20);
  rect(550, 200, 150, 100);
  fill(255);
  text("Price: " + depthUpgradePrices[playerData.getInt(3, "value")] + "\n Buy", 525, 125, 200, 250);
 
  if (checkObjectPressed(100, 200, 150, 100)){
    if (cash >= toolUpgradePrices[playerData.getInt(1, "value")]) {
      cash -= toolUpgradePrices[playerData.getInt(1, "value")];
      playerData.setInt(1, "value", playerData.getInt(1, "value")+1);
      mineSpeed = playerData.getInt(1, "value");
    }
  }

  if (checkObjectPressed(325, 200, 150, 100)){
    if (cash >= tipUpgradePrices[playerData.getInt(2, "value")]) {
      cash -= tipUpgradePrices[playerData.getInt(2, "value")];
      playerData.setInt(2, "value", playerData.getInt(2, "value")+1);
    }
    luckMultiplier = playerData.getInt(2, "value");
  }

  if (checkObjectPressed(550, 200, 150, 100)){
    if (cash >= depthUpgradePrices[playerData.getInt(3, "value")]) {
      cash -= depthUpgradePrices[playerData.getInt(3, "value")];
      playerData.setInt(3, "value", playerData.getInt(3, "value")+1);
    }
    currentDepth = playerData.getInt(3, "value");
  }

  if (checkObjectPressed(40, 90, 35, 35)) {
    shopOpen = false;
  }
}
```

On day 4, I took over Kaysan's ore and pickaxe animation seen in void draw(). We used the pickY and pickX variables to set the position of the pickaxe. This way, instead of writing another image function in another selection branch, we could take the much simpler route to just change the variables, and the pickaxe would move automatically. I wanted to make the animation look better and have some acceleration in the swing, but at this point we were running out of time and had to prioritize more important things.

To make our shops, we decided to create arrays for each type of upgrade containing the names, prices, and any other important parts for each tier of upgrade. We decided to do it this way because it would make finding the info for each upgrade tier a lot easier since we could just use the current tier in the array to find the current value.

Kaysan used this arrays to display the upgrades in the shop.

---

By the end of the project, we had a full mining, inventory, and upgrade system working together. This game gave me a greater understanding of how to create reusable functions to make adding new content easy and I learned how to use tables and arrays to making finding information easy. To put it simply, this game helped me find ways to make good systems that make the development process much easier.
