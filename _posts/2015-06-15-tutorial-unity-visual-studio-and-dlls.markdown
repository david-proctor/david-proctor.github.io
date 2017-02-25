---
layout: post
title:  "Tutorial: Unity, Visual Studio, and DLLs"
date:   2015-06-15 14:01:04 -0800
categories: blog c# unity tutorial
---
So you’re a little fed up with Unity. You’re finding it difficult to cram all of your classes into MonoBehavior-shaped boxes, or you miss handy access to interfaces, or you just want to write some unit tests. Perhaps you’ve seen skilled programmers mention that next time they plan on doing as much as possible outside of Unity, treating the editor itself as a sort of graphics library more than anything else. That’s certainly how I want to organize things, but I feel the documentation lacks detail and clarity when describing this approach. Here's how I did it.<!--more-->

## Create your DLL

![Create DLL dialog]({{ site.url }}/assets/unity-dll-tutorial-newproject.png)

Open up Visual Studio, and go to File > New Project. Select “Visual C#” from the Templates pane and “Class Library” from the main view.

Very importantly, at the top of this dialog, you’ll need to set the project to use .NET Framework 3.5. This came as a disappointment to me, as I was hoping to use some newer Linq features, but Unity’s reliance on Mono sadly prevents you from doing so. Fortunately, you can still use newer language features included in later versions of the compilers, including string interpolation.

Set up your name and choose a folder to create your Visual Studio solution in. I recommend putting it in a folder on the same level as your Unity project’s folder. Here’s what my setup looks like:


![Create DLL dialog]({{ site.url }}/assets/unity-dll-tutorial-folderstructure.png)

Notice that this is also the root folder of my Git repository. These two different types of projects are kept separate, but they both belong to the same Git repo, keeping everything nice and clean. I used Gitignore.io to build a gitignore for Unity and Visual Studio; you can simply smack them into the same file.

## Set up your dependencies

One of the best things about this setup is that you still have access to the Unity classes that you need – your Character class, for example, can have a GameObject member for its avatar. Right-click on your project in the Solution Explorer and go to Add > Reference. Click Browse and navigate to your Unity install folder (that’s the Unity Engine install folder, not your project folder). Navigate further to Unity\Editor\Data\Managed. From this folder you want to add UnityEngine.dll, and UnityEditor.dll if you also want to modify the editor’s behavior.

Now you can simply add a `using UnityEngine;` directive to the top of your file. Let’s put together a quick test file:

{% highlight c# linenos %}
using UnityEngine;

namespace StratBoyDLL
{
    public class Guy
    {
        public GameObject Avatar { get; private set; }

        public override string ToString()
        {
            return "Hello world!";
        }
    }
}
{% endhighlight %}

## Set up DLL copying upon build

This is where the really cool piece is: we’re going to set up MSBuild to automatically copy our DLL to the appropriate folder in the Unity project as soon as it’s built.

Right-click on your project and click “Unload Project”; its name will change to be non-bold and be marked “unavailable.” Right-click on it again and select “Edit .csproj”.

The XML file that you see is the MSBuild file that controls configuration of the build process for your project. We’re going to add a step that will copy the new DLL to our Unity project. At the bottom of the file, just above the `</Project>` tag, add the following:

{% highlight xml linenos %}
  <Target Name="AfterBuild">
    <Copy SourceFiles="$(OutputPath)$(AssemblyName).dll" DestinationFolder="..\..\Unity\Assets\Scripts\" />
  </Target>
{% endhighlight %}

`$(OutputPath)` and `$(AssemblyName)` are variables representing the path to the built DLL and the name of your assembly, respectively. `DestinationFolder` is relative to the .csproj file, which is kept in the project folder (not the solution folder). You’ll want to change this value depending on where you want the DLL copied to and what you’ve named the folder it’s kept in.

Save and close your .csproj file, and right-click the project in the Solution Explorer and choose “Reload Project.” Now you can build it and take a peek at what shows up in your Unity project.

## Use your DLL

Open the Unity editor; it should detect your DLL in the location you specified. Place a GameObject and add a test script to it – something like this:

{% highlight c# linenos %}
using UnityEngine;
using DemoDLL;

public class TestClass : MonoBehavior
{
    void Start()
    {
        var guy = new Guy();
        Debug.Log(guy.ToString());
    }
}
{% endhighlight %}

Save your work and hit the play button in the editor. If you see “Hello world!” popping up in the console, you’ll know everything is working properly!

If Unity refuses to let you start the game and instead shows a `ReflectionTypeLoadException`, it’s probably because you forgot to target .NET 3.5 when creating your project. You can correct this by going back to Visual Studio, right-clicking on the project, and opening properties. In the Application category there is a setting for Target Framework; set it to “.NET Framework 3.5”.

One last point: you will, of course, have to remember to build your DLL before testing it in Unity – the one disadvantage of this approach is that Unity can no longer recompile your code the moment you hit the play button. On the other hand, this approach makes it easier to create much cleaner, testable, and even SOLID code. You are freed from the burdens of working with files that are not properly organized into projects and overwhelmingly contain classes that inherit from MonoBehavior. That’s a beautiful thing.
