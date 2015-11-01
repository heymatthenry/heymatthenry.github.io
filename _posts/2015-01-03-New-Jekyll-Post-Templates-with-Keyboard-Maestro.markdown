---
layout: post
title:  "New Jekyll Post Templates with Keyboard Maestro "
date:   2015-01-03 12:11:32
categories: workflows
---

I've been a Keyboard Maestro user for a couple of years now, but haven't delved too deeply into building my own macros. The new blog seemed like a good opportunity to get my hands a little dirty.

## Requirements

I needed a basic macro that would:

1. Ask me for a post title
2. Create a new markdown file in the Jekyll directory (with a properly formatted file name).
3. Populate it with basic YAML front-matter.

## The Workflow

So here's what it actually does. First, prompt me for the post's title:

![Post title prompt]({{ site.baseurl }}/assets/jekyll-km-prompt.png)

The step in the macro looks like this:

![Set a variable with the post title]({{ site.baseurl }}/assets/jekyll-km-post-title.png)

Jekyll needs specifically formatted time and date stamps for both the post filename and for the front matter.[^need] So next we set KM variables with those formatted strings.

![Set variables for the datetime]({{ site.baseurl }}/assets/jekyll-km-datetime.png)

[^need]: I'm not a Jekyll expert, so I don't know if it "needs" them, but the basic templates use them, and I'm sticking with vanilla Jekyll right now.

Next is the fun part. To put together the filename we want to create, we take the post title from the first step and replace any spaces with dashes. We can do this with an embedded shell script in the macro. This took me a minute to figure out, since [KM has a particular way you need to call its variables from shell scripts](http://www.keyboardmaestro.com/documentation/6/scripting.html#scripting_actions "Keyboard Maestro 6 Documentation: Scripting"), but it's straightforward. So here's how I did it (I'm not much of an awk-er, so there's probably a better way):

![Dasherize the post title]({{ site.baseurl }}/assets/jekyll-km-dasherize-better.png)

**Update:** The original version of this macro choked on one-word post titles. The conditional checks for that condition now & skips the dasherize thing if the post title is just one word. Like literally everything about this macro, there are a million better ways to do this.

Next, we create the post file and insert some basic front-matter. There are three main things going on in this step:

1. Setting a block of text that's going to go into the new file
2. Setting the post title and timestamp with the variables we set before
3. Creating a new markdown file with the name `new-post.markdown` (we're about to change this).

![Set the frontmatter]({{ site.baseurl }}/assets/jekyll-km-frontmatter.png)

Then finally, we just rename the file we just created to the match the desired format. This just involves concatenating a bunch of variables we've already set.

![Rename the post file]({{ site.baseurl }}/assets/jekyll-km-rename.png)

That's it! It's a lot of steps, but it's pretty straightforward. In putting this together, I learned a lot about how KM works, and it was surprisingly easy. Now I feel like there are a lot of things I could do with this.

## Next steps

This is a minimum viable product for this workflow. Things I may or may not add as I use it more:

* Obviously the file name formatter is as basic as it could be. It would be nice to strip out punctuation, etc.
* It would be cool to be able to select a different layout/post type from a dropdown as I implement more of them.
* Maybe create a subdirectory under `/assets` with the post slug, so I can keep assets organized.
* Open the new post for editing in Sublime or Byword or whatever.

I'm willing to bet I could implement a similar workflow in [Workflow for iOS](https://workflow.is). Stay tuned I guess?
