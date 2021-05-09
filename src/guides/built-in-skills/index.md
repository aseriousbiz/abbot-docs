---
uid: built-in-skills
---

# Things every Abbot can do

Abbot can do a lot the moment it's installed, even before you've created your first skill. Here are some things you can do as soon as you see Abbot.

## Use Abbot as a database for facts with the `rem` skill
Abbot makes it easy to keep track of facts for your team. Set a new fact by saying `@abbot rem {keyword(s)} is {fact}`. We use `rem` to help us remember URLs to dashboards, office addresses, and funny gifs that we've shared in the past. You can search what's been saved with the `|` character. For example, to find anything saved with `cat` in the keyname, you would say `@abbot rem | cat`, and Abbot would return a list of every key containing `cat`.

Make Abbot forget a fact by saying `@abbot forget {keyword(s)}`. 


## Create a team biography with the `who` skill
Abbot can keep track of fun facts about your team as well. Say `@abbot @Username is {fun fact}` and Abbot will remember. Afterwards, anyone can see anyone else's biography by saying `@abbot who is @Username` in chat. 

Items can be removed by saying `@abbot @Username is not {fun fact}`.


## Find out who's nearby with `who is near`
Abbot can tell you who's nearby if people have set their location in chat. Say `@abbot who is near me` or `@abbot who is nearby` to find out who is within 25 miles (or 40 kilometers) of your location. You can ask Abbot about who is near another location by saying `@abbot who is near {location}`. It's almost as good as being there yourself!

Set your location in chat by saying `@abbot my location is` with your location.


## Set permissions with the `can` skill
Some skills may do dangerous things, or access sensitive data. Abbot makes it easy to control who can access those skills with the `can` skill. To use it, mark the skill as restricted in the website. Then in chat, say `@abbot @Username can use {skill}`. This will allow `@Username` to call the skill from chat. Other permission levels are:
* `edit` - In addition to using the skill, allows `@Username` to edit the skill. 
* `admin` - Gives full permissions to the skill, including granting and revoking other user's permissions.
* `not` - Revokes all permissions to the skill. Use it like this: `@abbot @User name can not {skill}`. 

You can find out who has permissions to a restricted skill by saying `@abbot who can {skill}`.

We made a short video about the `can` skill if you'd like to learn more about it here: [Restricting Skills with Abbot ACLs](https://youtu.be/6NHMyyWZtrU).


## Custom Lists
We saw a lot of scripts made to let people create lists of information (usually gifs or quotes), and return random items from the list whenever the script was called. This happened so much that we made it easy to create these types of skills directly from chat. Thes are called list skills. There's a guide specifically for list skills because we think they're so useful, check it out here: [Guide: List Skills](xref:list-skills).


## Answering questions and general tomfoolery
No matter the topic, Abbot always has something to say. Abbot tries to be useful, and if that fails, will return something... creative. Try asking Abbot some questions! Some ideas: 
* `@abbot what’s the capital of Uruguay?`
* `@abbot what’s the square root of 256?`

If your location is set using `@abbot my location is`, Abbot will try to give you results specific to your location. If you've set your location, try saying `@abbot coffee shops near me`. 

Finally, if you're not sure what to say; try saying hi to Abbot or asking Abbot's opinion about your favorite band!

## Ideas? Feature requests? Send feedback with Abbot!
Do you have a fun idea for an Abbot skill, but you're not sure how to make it happen? Is there something Abbot can do better? Send us feedback from chat by saying `@abbot feedback` followed by your feedback. If you'd like for us to respond directly, be sure to set your email first by saying `@abbot my email is {your email address}`, otherwise we may not be able to contact you with questions or updates. 