elfeed-backends
==============
TODO [![MELPA](http://melpa.org/packages/elfeed-backends-badge.svg)](http://melpa.org/#/elfeed-backends)

Provide extra backends to make self-hosting RSS readers works
with [elfeed](https://github.com/skeeto/elfeed),
including
[Nextcloud/ownCloud News](https://nextcloud.com/),
[Tiny Tiny RSS(TODO)](https://tt-rss.org/fox/tt-rss),
[NewsBlur(TODO)](https://newsblur.com/) and even more.

# Installation through MELPA
TODO

    ;; Install through package manager
    M-x package-install <ENTER>
    elfeed-backends <ENTER>

# Initialization
Setup elfeed-backends, then switch to search view and and press G to update entries:

        ;; recommend curl
        (setq elfeed-use-curl t)
        (elfeed-set-timeout 36000)
        (setq elfeed-curl-extra-arguments '("--insecure")) ;necessary for https without a trust certificate

        ;; setup for owncloud news backend
        (elfeed-backends-enable 'ocnews)
        (setq elfeed-backends-ocnews-url "http://127.0.0.1:8080")
        (setq elfeed-backends-ocnews-username "user")
        (setq elfeed-backends-ocnews-password "password")

# Backend Details
## ownCloud News
1. Fetch all articles with the lastest modified time
1. Support sync unread and starred tags, the starred tag name defined
   in `elfeed-backends-ocnews-star-tag` which default value is
   `star`. For example, if user add `star` tag to one article, the
   stat will be sync to server, too

# Have a Try
If you never use such slef-hosting RSS readers, why not deploy one in 10 minutes. For
example Nextcloud:

1.  Fetch Nextcloud image and run it

        docker pull nextcloud
        docker run --rm -p 8080:80 nextcloud

2.  Open <http://127.0.0.1:8080> in browser to setup
    1.  Create admin user and select database to SQLite, then press "Finish setup"
    2.  Press left top popup menu and select "+Apps", select
        "Multimedia", and enable the "News" app
    3.  Press left top popup menu and switch to "News" app, then
        subscribe some feeds

3.  Setup elfeed-backends or
    other
    [Nextcloud News clients](https://github.com/owncloud/News-Android-App),
    both will works OK

# Problems
1. Sometimes emacs may be blocked if the parsing downloaded articles
   is too large, for example >50MB. This is caused by the known emacs
   bug that CPU will be in high usage if a text line is too
   long. There three methods to workaround this:
   1. Method 1, setup the server side do not download one-line data
      e.g. wrapped JSON if possible
   2. Method 2, run emacs which cpulimit to prevent the use of CPU to
      affect other programs

          cpulimit -l 80 emacs

   3. Method 3, limit the download article size or reset the lastest
      modified time to skip some data:

          (elfeed-backends-ocnews--set-last-modified (- (time-to-seconds) (* 1 3600)))

# License

Released under the terms of the GNU GPLv3+.
