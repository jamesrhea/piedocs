---
date: 2016-09-17T12:00:00
draft: false
tags: [technical writing]
title: Ordered Lists and Data Integrity
summary: WYSIWYG editors usually allow you to create ordered lists. But you should avoid letting your editor break your list's integrity.
---


WYSIWYG editors usually allow you to create ordered lists. In HTML, ordered lists look like this:

``` html
<ol>
	<li>Buy ice cream</li>
	<li>Open ice cream</li>
	<li>Eat ice cream</li>
</ol>
```

But imagine you want to add a tip after the "buy" step. In your editor, you might add a line after the first list item, add your tip, and then start the list number where you left off. Here's one way the HTML might look after doing that:

``` html
<ol>
	<li>Buy ice cream</li>
</ol>
<div class="tip">Bring a cooler!</div>
<ol start="2">
	<li>Open ice cream</li>
	<li>Eat ice cream</li>
</ol>
```

Here's what that looks like:
<ol>
	<li>Buy ice cream</li>
</ol>

<div style="margin: -1em 0 -1em 3em; padding-left: .5em; background: aliceblue; font-style: italic;"><p>Bring a cooler!</p></div>

<ol start="2">
	<li>Open ice cream</li>
	<li>Eat ice cream</li>
</ol>

Perfect, right? Unfortunately, although this might look okay in an editor or even in the resulting web output, the data integrity of the list is broken. When you look at the content in a WYSIWYG editor, what you _appear_ to have is a single list with a tip connected to the first list item. But what you _actually_ have is two unrelated lists with an unrelated tip between them. Why is this a problem?

Consider what happens if you decide you forgot a step. You add one:

``` html
<ol>
	<li>Find ice cream</li>
	<li>Buy ice cream</li>
</ol>
<div class="tip">Bring a cooler!</div>
<ol start="2">
	<li>Open ice cream</li>
	<li>Eat ice cream</li>
</ol>
```

Output:

<ol>
	<li>Find ice cream</li>
	<li>Buy ice cream</li>
</ol>

<div style="margin: -1em 0 -1em 3em; padding-left: .5em; background: aliceblue; font-style: italic;"><p>Bring a cooler!</p></div>

<ol start="2">
	<li>Open ice cream</li>
	<li>Eat ice cream</li>
</ol>

Now there are two number twos and no number four. Instead a better approach is to integrate the tip into the list item it's associated with. Then let the ordered list auto-number all the way through:

``` html
<ol>
	<li>Find ice cream</li>
	<li>Buy ice cream
		<div class="tip">Bring a cooler!</div>
	</li>
	<li>Open ice cream</li>
	<li>Eat ice cream</li>
</ol>
```

Output:

<ol>
	<li>Find ice cream</li>
	<li>Buy ice cream
<div style="margin-left: 1em; padding-left: .5em; background: aliceblue; font-style: italic;">Bring a cooler!</div></li>
	<li>Open ice cream</li>
	<li>Eat ice cream</li>
</ol>

This approach prevents accidental misnumbering. It also makes it possible to do potentially useful things, such as query your content set to find out the average number of steps in your procedures.