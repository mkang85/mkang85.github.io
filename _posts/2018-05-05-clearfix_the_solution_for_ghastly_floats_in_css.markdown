---
layout: post
title:      "Who you gonna call? Clearfix! The Solution for Ghastly Floats in CSS"
date:       2018-05-05 12:48:18 -0400
permalink:  clearfix_the_solution_for_ghastly_floats_in_css
---



Recently, I've been haunted by the apparition of terrible web design.  I'm sure you've encountered it before.  I shiver just writing about it.  

Here's a warning.  Do not scroll down if you want to avoid scarring your programmer soul.  

Last warning...

![screenshot](https://imgur.com/0ezdr6z)



Whew! Sorry for that. Hopefully you're not too scarred by the footer so high up...it gives me chills to this day. 

Despite this terribly frightening layout, this is actually functioning as intended.  The layout for text and incorporating pictures makes sense if you want to have an image wrapped with text. 

However, this poses a problem if you want to have element separate, such as the footer. 

The solution to this is called "clearfix."  Here's the code below: 
```
.clearfix:after { 
   content: "."; 
   visibility: hidden; 
   display: block; 
   height: 0; 
   clear: both;
}
```


Let's break this down. 

The "after" portion of this code will essentially apply the following declarations.  It will place a "." at the end of the parent element. (This can be any character, for the interests of simplicity, this is conventionally set as ".") The visibilty of the content is set to hidden so it does not show, the height set at 0, also ensures that there is no additional space at the end of our element.  Display: block will essentially set the width to take up the whole screen.  Clear:both, will not allow any elements to the right or left of it, so it will essentially push the other elements down.  

So knowing this, if we simply set our parent element's class to "clearfix", we can avoid the subsequent elements to fill in that empty space. 


```
  <section class="clearfix">
          <img src="jazztilt40.jpg" alt="this is a guitar pic">
        </section>
```


![screenshot2](https://imgur.com/qWSeuEW)

No more footers rising from the grave!  

Remember to use "clearfix" to keep them footers, or whatever elements from creeping up in your web design! 
