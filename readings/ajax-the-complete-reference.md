---
description: A book written by Thomas Powell
---

# AJAX: The Complete Reference

## Talk with entrepreneur

### About Ajax

> fun…well the site has most of the goodies.  The reality is the book was literally written by me ~13-14 years ago.  The fact that I was able to keep much of it relevant is a testament to how I approach web dev but I would give you some different advice now.
>
> 1. Consider using the fetch API
> 2. Consider avoiding any discussion of how state management is used in the browser from the book and look into native browser history  - pushState, replaceState, etc. as well as related events.  Also be aware of localStorage as you do not need simulation of storage.  Though the discussion is correct.
> 3. Use negotiation of your requests to fetch appropriate content as opposed to sending a custom header.  If you make a JSON request and set right MIME type your server side should understand and respond with JSON, if standard x-www-form-urlencoding then you respond with a rendered page.  Basically the later happens with JS off or in a fallback.
> 4. Look into custom events in JS
>
> I am sure there is more, but those jump out at me.  I think diagramming the flow of how your router is going to work will reveal what to do.  Sometimes a functional style or even procedural style might more sense for that task.  You might want to design it first before thinking of the code model you plan on employing.

### About CMSes and businesses

> well I agree with a few of the statements you made esp. with the idea of JS on CS and SS.  I believe in that strongly, however, I am not sure I believe in the way it is done by many.  A stricter lang server side might provide some better security tho, but we can deal with that if we are cautious.  I am a bit worried about the component model being reinvented by React.  The more I think about it the more I really a mystified why a person wants to do all the things a browser does in some other abstraction.   As someone tweeted and I belly laughed about with “I never have re-rendered a complete DOM tree using JS in my many years of dev, what exactly are you doing?”  For me, it can be simply it doesn’t have to be hard but it takes work
>
> Generally for sites though we did not talk about it in class but 90% of what we do we use site compilers
>
> We basically drill DBs and churn pages out there is no need for fancy client side at all
>
> if we do any of that we do a little bit of Turbolinks style stuff with JS and it works just fine
>
> we keep our CMSes mostly off the Internet so they aren’t probed as well…and publish to a remote destination
>
> like Google Cloud, etc. and then just some small services are poked thru.  The chargers site was like 20k pages managed by 3 people and running with a homegrown site compiler style CMS…Drupal is such a pig we have really had to fight with it at customers we’ve taken over.  Complete overkill for what simple stuff they actually do
>
> these days if we inherit a Wordpress of Drupal site we try to treat it as a headless CMS and drill out of it
>
> This lets the org edit content in their known way but we aren’t restricted by it

### About Native apps, form or function

> well don’t fret about the native…mostly unless it really used native features really isn’t that special just syntax.  Few apps I see native really need it, just it is a nice item for shops that hire you because you touched something.As a guy who has touched lots of things over the years I wonder why awareness of syntax is so important?  Seems to me like hiring a builder because they built something the same as your house or with tools you know as opposed to having building skills in particular?So consider I’ve written native lots of C/C++ for ISAPi filters and some windows apps.  There were many lessons there and  I bring some lessons there into this class, but did you know that or did you think that that is different because native vs web? Ex: Node Modules directory being choked with 3rd party stuff is no different than the dll hell we faced on windows.  It is about dependency management.  The specifics are just…specifics.  Same with transpiling…look into things like [https://en.wikipedia.org/wiki/Tcl](https://en.wikipedia.org/wiki/Tcl) and note the date.  Flutter, Reactive Native, etc. this has all been done before.Try to focus on the deeper learnings and be confident you can figure whatever out you need to in the specifics.  First principles first, not form first.  Of course form will always change, but it won’t really at the design level at least not really.  At least that’s how it has been for me for a couple of decades now.

## Topics of book covered in lectures

{% file src="../.gitbook/assets/ajax.pdf" caption="Mindmap" %}



