# What?

This is based on jeykll.

I was going to use github pages, but then decided ... fuck it, I can host my own stuff.

## New Setup

Make sure bundler is installed.

Run something like `bundle install` to get all the dependencies if things are borked.

## Usual Updates

Run `bundle exec jekyll serve` and browse to http://localhost:4000

Jekyll will watch for changes and build static stuff in _site.

## Deployment

For now, just push from local machine to bitbucket, and pull from bitbucket to properly-configured live server.

