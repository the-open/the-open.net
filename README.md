Making A Great, Robust, Maintainable WP Site
============================================

This readme contains only a few sentences about how to run the WP site contained in this repository. Mostly it's about the steps I took -- which you
may be able to replicate -- to build a WordPress site that leaves me feeling like I'm using modern tools and producing a high-quality product.

If you follow these steps you'll have a site that uses Bootstrap SASS with a well-organized template structure, your assets will be concatenated, minified, and placed correctly, you'll have a working Gulp setup with live reload, you'll get free SSL and DDOS protection from CloudFlare and free hosting without having to worry about server management from OpenShift.

Local Setup
-----------
1. Clone the [OpenShift WordPress Developer Quickstart](https://github.com/openshift-quickstart/openshift-wordpress-developer-quickstart).
  (More on this in [OPENSHIFT.md](OPENSHIFT.md))
1. Make yourself a database and update `wp-config.php`.
1. Point Apache or MAMP at the appropriate place and fire up your web browser!
1. Run the famous WordPress 5-minute install.
1. Probably best to update WordPress to the latest version now.
1. Download the [Sage starter theme](https://github.com/roots/sage/releases/latest) by roots.io.
1. Check out the [Sage README.md](wp-content/themes/sage/README.md), and set up Gulp/Bower accordingly. (You must do this from `wp-content/themes/sage`.)
1. Find `manifest.json` and change `example.dev` to `localhost`
1. In your theme directory, run `gulp watch` and check out your new site!

Deploy to OpenShift
-------------------
1. Install the Red Hat Cloud client tools ([docs](https://developers.openshift.com/managing-your-applications/client-tools.html)) and run `rhc setup`.
1. Make a new app per the instructions in [OPENSHIFT.md](OPENSHIFT.md) with `rhc app create wordpress php-5.4 mysql-5.5 --from-code=https://github.com/openshift-quickstart/openshift-wordpress-developer-quickstart.git`
1. Open your site in a browser and run the WordPress install. (This one was [http://wordpress-theopen.rhcloud.com/](http://wordpress-theopen.rhcloud.com/).)
