---
layout: page
title: About
permalink: /about/
---

## Purpose

Sometimes I end up mentoring developers who kind of stumbled their way into programming, got enough knowledge to be
dangerous, which got them some gigs, and they've been busy ever since.

They usually come to me when they're buried, or come up against some requirements they don't know how to satisfy.

This is almost never for green-field development.  There's already some existing code base, usually a bunch of files
floating around in the webroot that end with a .php extension.

Inside those files, I uncover atrocities.  Or, at least, areas where things can be improved with a some knowledge and
a little practice.

These developers aren't dumb, or lazy. They're ambitious, hard-working, and mostly self-taught/self-made.  They've 
typically been running a web development business, with real businesses for clients.  And their clients love them!  
Typically, this success has been their proverbial downfall.

These people *know* they should be improving their skills, but years of tight deadlines, demanding clients, and flavor-
of-the-week technology trends have made it so that, from the outside 2005-2015 seem like a lost decade.

As I work with these people, I try to facilitate incremental improvements in their understanding a practices.  Throwing
a framework at them is ineffective - they never have time or mental bandwidth to climb the learning curve. They do like 
tools and workflows that make them more efficient.  And such tools and workflows exist!

Each of the pages in this site are written to help such a developer take a small positive step forward as a developer.
Each page will dissect one bad practice, anti-pattern, or operational failing that I've encountered working with real
developers.  The page will:
 
  * Describe what the developer was doing wrong.
  * Explain why it was the wrong thing to do.
  * Present and explore a better way.
  * Demonstrate how adopting the better practice will improve their lives.
  
I've started with the grand-daddy of PHP atrocities: [continued usage of the `mysql`]({% post_url 2015-02-17-stop-using-mysql-or-the-dog-gets-it %}) library (exposed in PHP as the 
`mysql_*` family of functions).

## Credits

These pages are written by [timdev](https://github.com/timdev), a guy who's been doing web application development for
probably far too long.

## Contributing

If you think you can improve on anything here, feel free to create pull request, or open an issue [at github](https://github.com/timdev/php-therapy.timdev.com).