---
id: 14
title: My Favorite Avenue Snippet Ever.
date: 2010-05-16T16:33:30-05:00
author: Matt
excerpt: My favorite Avenue snippet.
layout: post
guid: http://maprantala.com/?p=14
permalink: /2010/05/16/my-favorite-avenue-snippet-ever/
categories:
  - Avenue
tags:
  - ArcView
  - Avenue
  - global dictionary
  - global variables
  - self
---
I worked in Avenue for many years but do not expect to do too much more. Before my Avenue skills erode to nothing, thought I would share my favorite Avenue snippet. Really, it involves two &#8220;function&#8221; scripts that clients scripts can use.

What I got use to doing is instead of using a mess of global variables is using one global variable&#8211;a dictionary&#8211;that was used to store what essentially became global variables. The main advantage of doing this is that I started storing the parameters passed to each script as a global variable named after the script. This came in EXTREMELY useful for debugging purposes because when I was having trouble with a script I could write out my global dictionary to an ODB file at the beginning of that script, thereby storing the application&#8217;s state prior to the problem.

The first script, MyApplication\_Routine\_AddSelfDict, is used to add/modify a dictionary entry:

<pre>theScript = self.get(0)
theSelf = self.get(1)

if (_MyApplication_SelfDict.is(Dictionary).not) then
  _MyApplication_SelfDict = dictionary.make(25)
end

if (self &lt;&gt; nil) then
  _MyApplication_SelfDict.add(theScript,theSelf)
  _MyApplication_SelfDict.set(theScript,theSelf)
end</pre>

The second script, MyApplication\_Routine\_GetSelfDict,  is used to retrieve a value from the global dictionary.

<pre>if (_MyApplication_SelfDict.is(Dictionary).not) then
  _MyApplication_SelfDict = dictionary.make(25)
end

return (_MyApplication_SelfDict.get(self))</pre>

Then I can have client scripts first add an entry (primary key = script name)to the global dictionary storing the parameters passed in (self).  Then I set a local variable to the entry just added.   Then, if I need to debug later, I can throw an exit (shown in red) into the script, run the process and after it exits, I can step through the script and pull the parameters passed in from the global dictionary.

<pre>If (self&lt;&gt;nil) then
  av.run("MyApplication_Routine_AddSelfDict",{"MyApplication_ClientScript",self})
End

theSelf = av.run("MyApplication_Routine_GetSelfDict","MyApplication_ClientScript")

<span style="color: #ff0000;">'**** The following EXIT is added for debugging purposes.  This gets temporarily added so I can capture 'the application  state.  After running the process and getting to this point, I remove the exit and can step 'through this script.  The snippet above retrieves the parameters as passed into this script.</span>

<span style="color: #ff0000;">EXIT</span></pre>

And that&#8217;s it.  Took me several years to refine this technique&#8211;for a long time I just stuck a check in at the beginning of each script to see if there were any parameters passed and if so, store them in a separate global.  But what I like abut this is I can write the whole global dictionary out to an ODB, quit out of ArcView and then return to the project, read in the ODB and reset my global dictionary and pick up from where I left off.

The one addition that I added later was to remove entries from the global dictionary once a script ends.  I didn&#8217;t show it here.

Having all this data in one global dictionary is probably a horrendous design approach to anyone with an Object-Orientated mindset but, as they say, it got the job done.