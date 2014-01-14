---
title: Storing data: INI
layout: post
permalink: /en/Storing-data-INI.html
---

# Storing data: INI

## INI
An INI file is a in section seperated configuration file which allows you to store different data in one or more sections.

The structure of an ini file is simple:

{% highlight ini linenos %}
[StructureName]
Key1=Name
Key2=Age
Key3=EyeColour
[StructureName2]
Key1=Name
Key2=Age
Key3=EyeColour
{% endhighlight %}

It's perfect for storing numbers or important information which the program or the script needs. It also provides a good overview.

## IniWrite

As the name already says, you can use this command to write values into an ini. The command's structure is:

__IniWrite, Value, Filename, Section, Key__

The value can be either a number or letters. Please note, you can not keep this emty - instead use %A_Space%, in case you don't want to add something.

Secondly, you need to add the filepath of the ini file, so the script can add the value to it. It can be either normal string or a variable like %A_desktop%\example.ini.
The section, as above already mentioned, is basically the data block's name. The script needs it to work out which Key (see example) it should use because the ini file allows you to use duplicated key names.
And finally, the key, you add it to retrieve the piece of data which you previously added to it.

An example:

{% highlight ahk linenos %}; any AutoHotkey version
IniWrite,Tim,%A_desktop%\example.ini,Member1,Key1
{% endhighlight %}

The result in the ini would look like:

{% highlight ini linenos %}
[Member1]
Key1=Tim
{% endhighlight %}

## IniRead

Imagine, you would have a script which allows the user to add details of a person to an ini. The IniWrite was a good example for it. But how would you get these details to show up without opening the ini file?

The IniRead command allows you to access it - and it isn't even complicated! The command's structure is basically as the IniWrite one with 2 differences: the outputvar (and the default - later)

__IniRead, OutputVar, Filename, Section, Key [, Default]__

As always, it starts with the command itself, followed by the variable where to store the ini's key's data (e.g the name of member1).
Then comes the file name, where it will access the ini.
Followed by the section and the key.

Let's take a look on the iniwrite example again:

{% highlight ini linenos %}
[Member1]
Key1=Tim
{% endhighlight %}

How would you retrieve the string Tim? Already figured it out? Good.

{% highlight ahk linenos %}; any AutoHotkey version
IniRead,Name,%A_desktop%\example.ini,Member1,Key1
MsgBox % Name ; Will show you the name you retrieved - which will be: Tim
{% endhighlight %}

Or you retrieve the data of the key1 from the second member, then it could be:

{% highlight ahk linenos %}; any AutoHotkey version
IniRead,Name,%A_desktop%\example.ini,Member2,Key1
{% endhighlight %}

Same key name but a different block name.

## IniDelete

So now, the member left your community but the details about him are still in the ini file - how would you delete them? - manually? We'll, of course, use a command again. This time it's IniDelete.

The command's structure:

__IniDelete, Filename, Section [, Key]__

As you already saw, it contains this []. You've learned before that these indicate that the command part enclosed by them is optional.

Back to our example:

{% highlight ini linenos %}
[Member1]
Key1=Tim
{% endhighlight %}

We now want to delete his name:

{% highlight ahk linenos %}; any AutoHotkey version
IniDelete,%A_desktop%\example.ini,Member1,Key1
{% endhighlight %}

This will only delete Key1 of the section 'Member1'.
To delete the whole section, we simply remove the key name:

{% highlight ahk linenos %}; any AutoHotkey version
IniDelete,%A_desktop%\example.ini,Member1
{% endhighlight %}

Full example script:

{% highlight ahk linenos %}; any AutoHotkey version
; You can even use the output of a msgbox or a control to write it into the ini.
; Imagine an user inputed the age of Tim in an InPutBox, we'll now use it in the command.
; AgeOfTim (the variable we use for the output contains 21
IniWrite,Tim,%A_desktop%\example.ini,Member1,Name
IniWrite,%AgeOfTim%,%A_desktop%\example.ini,Member1,Age
IniWrite,blue,%A_desktop%\example.ini,Member1,EyeColour

IniWrite,Tom,%A_desktop%\example.ini,Member1,Name
IniWrite,Age,%A_desktop%\example.ini,Member1,Age
IniWrite,green,%A_desktop%\example.ini,Member1,EyeColour

; We'll now read the ini's data 
IniRead,Name,%A_desktop%\example.ini,Member1,Name ; You can use the same name as the output & key name
; But make sure, you use another name for the second user if you are retrieving the information at once without using them right after (example afer this one)
IniRead,Age,%A_desktop%\example.ini,Member1,Age
IniRead,EyeColour,%A_desktop%\example.ini,Member1,EyeColour

IniRead,Name2,%A_desktop%\example.ini,Member2,Name
IniRead,Age2,%A_desktop%\example.ini,Member2,Age
IniRead,EyeColour2,%A_desktop%\example.ini,Member2,EyeColour

MsgBox % "Member1: " Name
             . "His age: " Age
             . "His eye colour: " eyecolour
             . "Member2: " Name2
             . "His age: " Age2
             . "His eye colour: " eyecolour2
IniDelete,%A_desktop%\example.ini,Member1
IniDelete,%A_desktop%\exmaple.ini,Member2 ; ini is now emty.
{% endhighlight %}

In one of the comments I said, it wouldn't be smart to use the same variable to retrieve a content of 2 different blocks/sections.
Let's say, the script saves Tim in the variable Name but then it goes to the second section and saves Tom in the same variable - what do you think does the variable Name now contain?
Right, it contains Tom because it automatically overwrote the first content.

## Tip

If you do not want to store the information in a seperate INI file you can use the script file itself by adding a commented section with the proper INI sections and keys. Use the built-in variable __A_ScriptFullPath__. Of course this will only work for **uncompiled** scripts.

**example**

{% highlight ahk linenos %}; any AutoHotkey version
; your script

; to read:
IniRead, Key1, %A_ScriptFullPath%, settings, key1

; to write:
IniWrite, %key1%, %A_ScriptFullPath%, settings, key1

/*
[settings]
key1=value
key2=value

*/
{% endhighlight %}

