# SummittDweller/hugo-timeline

A Hugo shortcode and provided partial plus CSS capable of rendering a multi-column (up to 5 columns) timeline display of events with NO Javascript.

The project consists of three key files:

  - `shortcodes/hugo-timeline.html` - The shortcode that ultimately generates a timeline display, one per page.  This shortcode builds the timeline itself, without any events. 
  - `partials/hugo-timeline-li-event.html` - A partial called by the `hugo-timeline.html` shortcode.  This partial is called once per event and is responsible for placement of the event display on the timeline built by `hugo-timeline.html`. 
  - `static/css/hugo-timeline.css` - The timeline-specific CSS used control layout of the timeline and events.  Included in `hugo-timeline.html`.

Apart from this `README.md` file, a `sample-data.md` timeline "page" is included as an example.  The timeline and events displayed upon it are generated from the front matter provided in pages like `sample-data.md` which makes a call to the `hugo-timeline.html` shortcode.  

*__Coming soon!__* `hugo-timeline` is NOT a theme, and since there are so few files involved it can be installed as a [Hugo module](https://gohugo.io/hugo-modules/).  Details are provided below.

## A Sample

I maintain an example timeline rendered in my personal blog at https://blog.summittdweller.com/timeline. 

## Features

  - The timeline is rendered from a _Hugo_ shortcode, so it is essentially "called" from within any page by placing a `{{% timeline %}}` reference in the page's Markdown (`.md`) file.
  - The can be only one timeline per page, but you can have an unlimited number of timelines in any given site.
  - The timeline "grid" and events displayed in the timeline are governed by front matter metadata in the aforementioned page `.md` file.
  - Each timeline is rendered in reverse chronological order, with most recent events at the top of the graph.
  - Each timeline can display between 1 to 5 columns of events.  The number of columns is also determined in the page's `.md` file, so there is no fixed number of colums per site.
  - The timeline must have a specifed `startYear` in the `.md` front matter, and it may have an optional `endYear`.  If no `endYear` front matter is provided the timeline will be "current", ending with December of the current year. 
  - Timeline granularity is per month, and it always displayed by full January-to-December years. For example: front matter values of `startYear: 2017` and `endYear:2021` would create a timeline that spans from January 2017, to December 2021.  If the `endYear` parameter is omitted the timeline would span from January 2017 until the end of the current year, at the time of this writing that's December 2022.  
  - Events may be discrete (a single `to` date with no `from`) or they may have duration with both a `from` and `to` start and end date/time.  
  - In an event, if a `from` date is specified with no `to` date, the event is assumed to be "current" and its top will always be the current month.

## History

The idea for a Hugo timeline project began with a meeting of the [_Rootstalk_](https://rootstalk.grinnell.edu) development team in September 2022.  The team, and our 2022 [Vivero fellow](https://www.grinnell.edu/academics/centers-programs/ctla/dlac/vivero) in-particular, had an interest in developing a simple timeline to chronicle events related to _Rootstalk_ and the contriutors who create it.

### Timeline.js

The _Rootstalk_ team initially considered implementing a relatively simple timeline using the [Timeline.js](https://timeline.knightlab.com/) package, perhaps displayed in an `iframe` on the _Rootstalk_ home page.  However, the notion of introducing so much Javascript in the _Rootstalk_ structure without damaging other capabilities made this option relatively unattractive.

### _MetalBlueberry_'s Hugo Timeline Shortcode

We then turned to the _MetalBlueberry_ [Hugo Timeline Shortcode](https://metalblueberry.github.io/post/howto/2021-02-28_hugo_timeline_shortcode/) project, one that uses very little Javascript.  Unfortunately, the timeline's structure and restrictions didn't really fit what we wanted for _Rootstalk_ since there's not support for multiple columns, overlapping events, or discrete/single-day events.  This code also doesn't appear to "scale" events, it just places and labels them, rendered in the sequence in which they are listed, as a "stack" of event "blocks."   

### _SimplGy_'s _Jekyll_ Timeline

While looking for a timeline that might be suitable for _Rootstalk_, we also came across this splendid [_Jekyll_](https://jekyllrb.com/) timeline project: https://github.com/SimplGy/jekyll-timeline.  The [demo pages](http://www.simple.gy/jekyll-timeline/) provided for it are spectacular!  Unfortunately, _Rootstalk_'s site, and virtually all other sites were I'd like to have a timeline, are generated in _Hugo_. 

I won't go into all the reasons I choose _Hugo_ over _Jekyll_ here, but suffice it to say I have enough interest in _Jekyll_ to keep a close eye on projects built for that platform.  I also have reason to try and learn as much about _Jekyll_ as I can since I really like the [_CollectionBuilder_](https://collectionbuilder.github.io/) project and the [_Lib-Static_](https://lib-static.github.io/) approach to digital preservation.

## Rebuilding _SimplGy_'s Timeline for _Hugo_

So began my first foray into _Jekyll_-to-_Hugo_ conversion.  The product of that conversion is [this new repo](https://github.com/SummittDweller/hugo-timeline) and this `README.md` file.

As part of my _Jekyll_-to-_Hugo_ conversion I kept notes regarding steps that I thought would be common in any similar template transformations.  The notes first appeared in the comments of this repo's `hugo-timeline.html` shortcode.  They include:

Common Jekyll to Hugo changes... 
1) Replace all opening '{% comment %}' tags with Hugo comment opening: curly-curly-slash-asterisk   
2) Replace all closing '{% endcomment %}' tags with Hugo comment closing: asterisk-slash-curly-curly  
3) Regex replace all '\{% assign (.+) = ' tags with '{{ $$$1 := ' and note the space after the = sign!
4) Replace all closing ' %}' with ' }}'
5) Replace all opening '{% ' with '{{ '

I made all of the above changes, and many more, using _VSCode_ to edit the source.

## Packaged as a Hugo (Go) Module

I used the guidance posted in [Hugo Modules: Getting Started](https://scripter.co/hugo-modules-getting-started/) to make a first distribution of this repo's components as a [Hugo module](https://gohugo.io/hugo-modules/).

### Applying Module Updates

Keep this module current in your projects by updating them using:

```
hugo mod get -u github.com/SummittDweller/hugo-timeline
```
...or to update ALL Hugo modules...

```
hugo mod get -u 
```


