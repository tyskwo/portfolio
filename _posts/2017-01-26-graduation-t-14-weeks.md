---
title: "Graduation: T-14 weeks"
description: "Winter break is over and my final semester at Champlain has begun!"
---

I don't like that. What's worse is that number will only go down. I guess that just means I need to make the most of it! This past month was winter break (hence the lack of updates), and I mostly spent it unwinding from the crazy whirlwind of last semester. I did get in contact with a few gaming companies to feel out prospective employments, and I'm excited to see where those lead. I also spent more time refactoring the <em>Office Mayhem</em> code base, as well as catch [Andy](https://andrewmillsapblog.wordpress.com/) and [Tony](https://tonyl.info) up to speed. I also began rewriting my website to move away from Wordpress. That isn't even close to completion, but you can get [a sneak peek here.](http://tyskwo.github.io/) I'm using Jekyll, which is a static website generator. Interesting, and definitely more my style as opposed to Wordpress, but I've never tried my hand at web development. It's exciting and different.

For _Office Mayhem,_ these first two weeks of classes were to get the ball rolling again. The artists have been sketching concepts and planning out animations, and the design team has been hard at work finalizing the outer game loops, brainstorming items within the office, and creating different task architectures. Andy implemented a feature called 'crunch time,' where everything moves faster, tasks are worth more, and there's even more mayhem at the end of every round. Tony refactored and cleaned up my `ProgressBar` code, and he's starting to implement a rock-solid event system to be used for accolades, sound effect decoupling, and possible environmental hazards. I took this time to finish up some last-minute refactoring, commenting, and general housekeeping, as well as rewriting the <code>MenuManager</code> class, complete with custom GUI.

<figure class="align-center">
 <img src="{{ site.url }}{{ site.baseurl }}/assets/images/blog/2017/01/menumanager.gif">
  <figcaption>The custom GUI in action. Automatically finds scene files, and has options for what button needs to be pressed, whether or not all players need to press to trigger it, and whether players can undo their action.</figcaption>
</figure>

To create this functionality, there were three problems to overcome. First, be able to find scene files within the project, create a custom GUI for selecting them, and creating an expandable GUI for the `MenuManager` itself. The reason for tackling this problem was that before, scenes would have to be manually spelled (possibility of misspellings), the lists were clunky and not visible by default, and the undo/all options were limited to the current scene, thus all transitions has the same values for those parameters – they weren't on a per-transition basis.

To tackle the first problem, I needed to be able to search through the project files and find any scene files:

{% highlight csharp %}
//in class: SceneAssetPathField
public static string LoadableName(string path)
{
    string start = "Assets/Scenes/";
    string end = ".unity";

    if (path.EndsWith(end))
    {
        path = path.Substring(0, path.LastIndexOf(end));
    }

    if (path.StartsWith(start))
    {
        path = path.Substring(start.Length);
    }

    return path;
}
{% endhighlight %}

All I needed was a function that can be called on path names to determine whether the scene was in the scene folder (Assets/Scenes), or ended in .unity (which designates a scene file). If it is, I return the name of the scene. Not very difficult.

Next, I needed to create a custom GUI for selecting scenes, and I settled on a `PropertyDrawer`:

{% highlight csharp %}
//this Unity header designates that this is a property drawer of the above method.
[CustomPropertyDrawer(typeof(SceneAssetPathField))]
public class SceneAssetPathFieldPropertyDrawer: PropertyDrawer
{
    //when we are updated
    public override void OnGUI(Rect position, SerializedProperty property, GUIContent label)
    {
        //get our old path
        var oldPath = AssetDatabase.LoadAssetAtPath<SceneAsset>(property.stringValue);

        //get our current path, see if it has changed
        EditorGUI.BeginChangeCheck();
        var newScene = EditorGUI.ObjectField(position, oldPath, typeof(SceneAsset), false) as SceneAsset;

        if (EditorGUI.EndChangeCheck())
        {
            var newPath = AssetDatabase.GetAssetPath(newScene);
            property.stringValue = newPath;
        }
    }
}
{% endhighlight %}

The above class is a [custom property drawer.](https://docs.unity3d.com/Manual/editor-PropertyDrawers.html) Nothing too out of the ordinary going on, though do pay attention to the use of `var` on lines 9, 13, and 17. This is useful because it allows the type to be inferred at runtime, so I don't need to know it at compile time (similar to `auto` in C++).

Lastly, I had to create the GUI for the entire system, which uses an internal class to the Unity Editor, the `ReorderableList`:

{% highlight csharp %}
[CustomEditor(typeof(MenuManager))]
public class MenuManagerEditor: Editor
{  
  private ReorderableList list;

  private void OnEnable()
  {
	list = new ReorderableList(serializedObject,
			           serializedObject.FindProperty("transitions"),
			           false, true, true, true);


	list.drawElementCallback = (Rect rect, int index, bool isActive, bool isFocused) =>
	{
	    //get the MenuOptions object referenced
	    var element = list.serializedProperty.GetArrayElementAtIndex(index);

	    //somme breathing room!
	    rect.y += 2;

	    //display the scene picker
	    EditorGUI.PropertyField(
		new Rect(rect.x, rect.y, rect.width / 3, EditorGUIUtility.singleLineHeight),
		element.FindPropertyRelative("scene"),
                GUIContent.none);

	    //draw the rest of the elements here...
	};
    }

  //when the GUI is updated
  public override void OnInspectorGUI()
  {
        //update the object we are attached to
	serializedObject.Update();

        //draw the list
	list.DoLayoutList();

        //apple any modified properties
	serializedObject.ApplyModifiedProperties();
  }
}
{% endhighlight %}
