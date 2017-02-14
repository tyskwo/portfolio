---
title: "A Quick Foray into Unity Editor Extensions"
description: "I am by no means an expert, but it's a good tutorial, I swear!"
---

Unity's entire user interface is created using classes Unity has made available to end users, which is pretty cool. Putting two and two together, this lets developers create tools for designers that have the native look and feel of Unity, and can be run inside the Unity ecosystem without having to open up another program. In Unity, there are two kinds of editor extensions: custom property drawers, and custom editor windows. Property drawers live inside a component, and in general affect the object they're attached to. Editor windows, on the other hand, aren't attached to a specific object, and can perform logic and operations to the entire project. For _Office Mayhem_, [I created](http://tyskwo.com/2016/09/30/office-mayhem-a-b-c/) a suite of tools to let my designers dictate how items and tasks relate to one another using editor windows. In this blog post, I'll give a short run down on how they work.

<figure class="align-center" style="width: 640px">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/blog/2016/09/relationship_creation_tool.gif" alt="">
  <figcaption>What we'll be creating.</figcaption>
</figure>

To start, create a new script in Unity. I called mine `ItemCreation_Tool`. Inside this class, rather than extending `MonoBehavior`, extend `EditorWindow`. This will give us access to all of the tools and classes the Unity team has created for UI building. In the gif above, you can see that I set the relationships up in an 'A + B = C' format for the designers. Internally, I call this an `EditorEquation`, which consist of three `ItemTypes`, which is just an enum that holds the different types of items. Since we're going to be defining many of these equations, our `ItemCreation_Tool` class will have a `List` of `EditorEquations`. (Note: to use `List`, you'll have to import `System.Collections.Generic`).

There are three inherited methods we are going to implement in our class: `ShowWindow`, `OnEnable`, and `OnGUI`. `ShowWindow` is called right when the user hits the button under Window in the menu bar with your tool's name on it; think of it as an initializer. The code for this method is dead-simple:

{% highlight csharp %}
    // shortcut = shift, cmd R
    [MenuItem("Window/Item Creation Tool %#R")]
    public static void ShowWindow()
    {
        //show existing window instance. creates one if it doesn't exist
        EditorWindow.GetWindow(typeof(ItemCreation_Tool));
    }
{% endhighlight %}

I think the comments make it self-explanatory. What's nifty, is that you can assign a keyboard shortcut to launching your tool, as seen in the first line. % is SHIFT, # is CMD/CTRL depending on your OS, and & stands for OPT/ALT. It's a small quality-of-life tidbit that is really easy to implement.

Next up, we have `OnEnable`. This method is called right after `ShowWindow`. In this method, we will check to see if our Relationships asset is already created. If it is, we'll load it into memory, if not, we'll make it! We're using an asset for two reason: 1) to keep the relationships consistent over multiple launches of Unity, and 2) the relationships will always be true for every level of the game.

{% highlight csharp %}
    void OnEnable()
    {
        //load the relationships asset
        lists = AssetDatabase.LoadAssetAtPath<RelationshipManager>("Assets/Prefabs/Resources/Relationships.asset");

        //if it doesn't exist, make it!
        if(!lists)
        {
            lists = ScriptableObject.CreateInstance<RelationshipManager>();
            AssetDatabase.CreateAsset(lists, "Assets/Prefabs/Resources/Relationships.asset");
        }
    }
{% endhighlight %}

`RelationshipManager` is a class that extends `ScriptableObject`, which allows it to be saved as an asset file. The class only holds two lists: one of relationships that create items, and one of relationships that destroys items, in the format of usedItemType, heldItemType, and createdItemType, using the same `ItemType` enum from before.

Lastly, there's `OnGUI`, which is the 'meats and potatoes' of the class. In it, this is where we layout the actual UI for the tool. I'll post the code first, and then go through it block by block:


{% highlight csharp %}
    void OnGUI()
    {
        //#1
        GUILayout.Label ("Relationships:", EditorStyles.boldLabel);

        //#2
        //label to show what order the items are in
        GUILayout.BeginHorizontal();
        GUILayout.Label ("Used Item  ");
        GUILayout.Label ("Held Item  ");
        GUILayout.Label ("Created Item");
        GUILayout.EndHorizontal();



        //#3    
        //for each equation, layout format
        foreach(EditorEquation e in equations.ToArray())
        {
            GUILayout.BeginHorizontal();

            //#4
            e.item1    = (ItemType) EditorGUILayout.EnumPopup(e.item1, GUILayout.ExpandWidth(false));

            GUILayout.Label ("+", EditorStyles.miniLabel);
            e.item2    = (ItemType) EditorGUILayout.EnumPopup(e.item2, GUILayout.ExpandWidth(false));
            GUILayout.Label ("=", EditorStyles.miniLabel);
            e.creation = (ItemType) EditorGUILayout.EnumPopup(e.creation, GUILayout.ExpandWidth(false));

            //#5
            if(GUILayout.Button("X", EditorStyles.miniButton)) RemoveEquation(e);

            GUILayout.EndHorizontal();
        }

        //#6
        //some breathing room :)
        EditorGUILayout.Space();
        EditorGUILayout.Space();

        //#7
        //add the buttons
        if(GUILayout.Button("Add Relationship")) equations.Add(new EditorEquation());
        if(GUILayout.Button("Add To Game"))      CreateAndSendRelationships();
        if(GUILayout.Button("Refresh"))          PullEquationsFromGame();
    }
{% endhighlight %}

At #1, we create a label at the top of our tool, simply saying "Relationships:", bolded. There are a plethora of styling options with almost every component. #2 is where we create the header above the first equation to show what order the items should be in. `BeginHorizontal()` tells Unity that everything between this and `EndHorizontal()` should be on the same line. #3 is where we layout each equation in the list. Since each equation is its own line, we'll use `BeginHorizontal()` again. #4 creates an `EnumPopup`, which is a dropdown pre-populated with all of the types of that enum. We set expand width to false so that it isn't super wide. In the rest of this block, we create a little + label, another enum popup, a = label, and a third popup to complete our equation. In #5, we create a button – a little 'x' – which calls a function: `RemoveEquation()`. Can you guess what this does? #6 adds a slight buffer between the equations and the bottom buttons, solely for aesthetic purposes. At #7, we create some more buttons for creating new relationships, adding them to the game, and refreshing the list.

In terms of the actual Editor GUI, that is all of it; it really is not a lot! There is one last piece of information though, which relates to saving the asset file. When we click the 'Add To Game' button, we call `CreateAndSendRelationships()`:

{% highlight csharp %}
    //called when 'Add to Game' button is pressed
    void CreateAndSendRelationships()
    {
        //get a game-format list
        List<Equation> toGame = new List<Equation>();

        //add all the equations to the list
        foreach(EditorEquation e in equations)
        {
            toGame.Add(new Equation(new Relationship(e.item1, e.item2), e.creation));
        }

        //send the list to the game
        //lists is a RelationshipManager
        lists.createOnUse = toGame;

        //and update and save the asset
        AssetDatabase.SaveAssets();
        EditorUtility.SetDirty(lists);
        AssetDatabase.Refresh();
    }
{% endhighlight %}

There is one thing to keep in mind here: `lists` is the `RelationshipManager` (singleton), which is the asset that we're saving. In order to actually save it, we have to call those last three lines, in that order. This saves the asset, sets it as 'dirty' in the eyes of the editor and compiler, and then refreshes the asset database so that the changes are reflected in the editor and in the game.

To get the class in its entirety, [click here,](https://github.com/tyskwo/OfficeMayhem/blob/master/Assets/Scripts/EditorExtensions/RelationshipCreations.cs) which is a link to the project on GitHub. I'll be sure to have some more behind-the-scenes tutorials soon. For now, the task at hand is getting [Will's art](http://willconcannonart.com) into the build, and fleshing out the task system. 'Till next time!
