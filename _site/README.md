# What?

The stuff behind https://phptherapy.timdev.com.

Stuck on github in case anyone wants to make a pull-request.

## How?

This is based on jeykll.  It could probably be hosted on github.io, but isn't.  See github's docs on github-pages+jekyll
for some background.

In summary:

### New Setup

Make sure bundler is installed.

Run something like `bundle install` to get all the dependencies if things are borked.

### Usual Updates

Run `bundle exec jekyll serve` and browse to http://localhost:4000

Jekyll will watch for changes and build static stuff in _site.

### Deployment

For now, just push from local machine to github, and pull from github to properly-configured live server.

