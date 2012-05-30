django-s3storage
================

This is our in-house version of `s3boto` from [django-storages](http://code.larlet.fr/django-storages/).

You can use this with a Django site to host your static/media files on Amazon S3.

Motivation
----------

We have a *lot* of files on our S3 server, and media-rich pages, so caching 
file names and sizes is essential to getting good website performance. 
Unfortunately, the caching found in [most forks of] s3boto uses a lot of memory...
in our case, ~150 MB just for caching what files are available!

@bradenmacdonald has modified this version to use very memory-efficient caching,
building on the caching fixes published by @alanjds.

Design goals
------------

* Work well in conjunction with [Easy Thumbnails](https://github.com/SmileyChris/easy-thumbnails),
  specifically never issuing a `HEAD` or `GET` request when thumbnails already exist.

* Look up `exist()` / `modified_time()` / `size()` requests from an in-memory cache 
  whenever possible.

* Assume other processes may be creating files and thumbnails on the S3 server, but 
  don't reload the entire cache when that happens.

* Allow multiple instances of `S3BotoStorage` to coexist and share a common cache,
  when they use the same bucket.

* Cache S3 entries using as little memory as possible.

* Assume files are never deleted from the S3 server, nor modified while the server
  is running.


Installation and Use
--------------------

All you need to do is copy the file **s3storage.py** somewhere into your web app.

Examples of how to configure it are in `example_settings.py`.

Configuration options are the same as the django-storages version.
