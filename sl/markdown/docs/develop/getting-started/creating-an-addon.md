---
title: Izdelava dodatka
---
Potrebna programska oprema: urejevalnik besedila, brskalnik Chrome.  
Preden nadaljujete, izključite morebitno že nameščeno razširitev Scratch Addons, da se izognete težavam.

## 1. korak: Preberite [članek o dodatkih](/docs/develop/getting-started/addon-basics/)
Najprej preberite članek na zgornji povezavi, da se seznanite z izrazi.

## 2. korak: Fork/podvojite kodo
Ali prenesite kot ZIP, če imate to raje. Skratka, prenesite kodo na svoj računalnik.

## 3. korak: Naložite razširitev v Chrome
*Note: Chrome is recommended for working on addons. Nevertheless, addons are expected to work on Firefox and Edge.*  
Now that you have the extension in your filesystem, go to `chrome://extensions` and toggle "developer mode".  
Click "load unpacked", then select the folder where Scratch Addons is located. If you're having issues with this, make sure to be selecting the folder where `manifest.json` is located.  
That's it, you loaded the extension! It should look similar to this:  
![image](https://user-images.githubusercontent.com/17484114/91502527-accfd580-e89e-11ea-9e16-7daa2b808379.png)  
Note: you can safely ignore it says "errors". That's just a warning for an unrecognized manifest key that's required by Firefox.

## 4. korak: Kaj bo vaš dodatek naredil?
Now comes the fun part!  
What will your addon do? Think of a self descriptive addon ID (no spaces or special characters, except hyphens).  
Got it?

## 5. korak: Ustvarite mapo za dodatek
V upravitelju datotek odprite mapo, v kateri je Scratch Addons. Poiščite mapo `addons`.  
V njej ustvarite mapo z imenom, ki ste ga pravkar izbrali.

## Step 6: Add an addon manifest
The addon manifest tells Scratch Addons how your addon works. Make sure to get this right to avoid headaches.  
Inside the folder you just created, create an `addon.json` file.  
This is a base you can use to start coding, make sure to change it in the future:
```json
{
  "name": "Epic placeholder text in place of addon name",
  "description": "Hello World! It would be really smart to replace this placeholder text with a description.",
  "tags": ["community"],
  "enabledByDefault": false
}
```
For more information on what you can declare in the manifest, check [this article](/docs/reference/addon-manifest/).


## Step 7: Tell Scratch Addons what your addon's ID is
Scratch Addons can't find new folders by itself, so you have to add the name to a special file.  
Go to `scratchAddonsFolder/addons/addons.json` and add the ID of your addon to the array.

## Step 8: Hello world
Your addon does nothing at the moment, so it's a good time to check if everything we made previously worked.  
Go to `chrome://extensions` and reload Scratch Addons by clicking the refresh symbol on its card.  
Now, right-click the Scratch Addons icon, and click "options".  
You should see your addon on the list! Once you find it, enable it, and set any settings that you may have.

## Step 9: The fun part, code!
*Before proceeding, make sure you read the wiki article linked in step 1.*  

Here comes the fun part: create your own JS or CSS files!  
Protip: after making any change to your addon, make sure to refresh the Scratch Addons extension like you did in step 8.  

Depending on what you want your addon to do, you should now check these wiki pages:
- [Userscripts](/docs/develop/addon-types/userscripts)
- [Userstyles](/docs/develop/addon-types/userstyles)

## Step 10: Make your addon customizable
If you want, you can make your addon customizable!  
Users of your addon will be able to toggle settings, enter numbers, and more!  
To get started, see [how to declare settings in the addon manifest](/docs/reference/addon-manifest/#settings-object).  
Then, head to the [addon.settings documentation](/docs/reference/addon-api/addon.settings) to learn how to access user choices from userscripts.

## Step 11: Before publishing your addon
Now that your addon works, let's make sure we can add it to the addon library.  
Make sure your addon's manifest is suitable, [more info here](/docs/reference/addon-manifest). Keep close attention to the name, description and tags of your addon. Make sure to set `"enabledByDefault"` to `false` or remove it.  
Make sure your addon doesn't break other addons.  
Make sure your code is understandable; having unnecessary comments is better than no comments.

## Step 12: Send a pull request!
Fork the repo if you haven't already, commit your new addon and send a PR!  
Keep in mind we might request you to make some changes, however, we will probably accept your addon as long as it's minimally suitable.
