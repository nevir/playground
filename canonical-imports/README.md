Canonical Imports
=================

So, here's the deal. Imports are really difficult for two reasons:

**CDNs:** How do you avoid importing the same _resource_ twice, if it is hosted
at multiple URLs?

**Hosted+Vulcanized Assets:** How do you avoid _executing_ overlapping
resources if they occur in vulcanized files that you don't own?


Thoughts
--------

* The CDN problem is relatively straightforward to solve: We need some way of
  mapping URLs to each other (or to resources).

* The hosted+vulcanized problem is inherently difficult. Either...

  * ...we have to specify at import time which resources the file contains
    (not DRY), in order to prevent requests of overlapping resources.

  * ...or we have to put guards within assets so that overlaps can be handled
    at execution time (wasted network resources).

* Vulcanization is **absolutely required** for app developers; it _may not_ be
  required for CDN-hosted assets.

* Hosted assets are _extremely convenient_ for service-based elements (for
  example, a mixpanel element w/ embedded API key).


Potential Solution
------------------

Because the hosted+vulcanized problem is so thorny, and there appears to be no
real winning case, I'd like to focus on making the URL deduplication approach
as easy as possible. ...while still supporting vulcanization for the common
case.

* Imports to resources _outside the project_ are **always** referred to via a
  _canonical URL_.

* You **never** directly import vulcanized assets (via an import tag).

* Vulcanized assets are, instead, defined via configuration for the `onload`
  hook. They are configured per _application_ only.


Woah, woah, woah; that's crazy talk! I hear ya, but there's a lot of advantages
here:

* We get to completely circumvent the overlapping resources problem (because
  pointing to vulcanized/overlapping resources is an **error** - just like
  today's world).

* Project owners can still vulcanize to their heart's content; and they're in
  full control of what is vulcanized, and how. Unlike today, where every CDN'd
  element might pull in its own copy of polymer.

* By using canonical URLs (say, github URLs), we start getting package
  management style behaviors. Bower might even be rendered _unnecessary_ if
  vulcanize can follow those URLs to find packages.

* Imports _within_ a project can still be relative (I think).

* For those who want to use CDNs as shared caches for elements (across apps),
  it's simple configuration.
