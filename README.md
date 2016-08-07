Making A Great, Robust, Maintainable WP Site
============================================

This readme contains only a few sentences about how to run the WP site contained in this repository. Mostly it's about the steps I took -- which you
may be able to replicate -- to build a WordPress site that leaves me feeling like I'm using modern tools and producing a high-quality product.

If you follow these steps you'll have a site that uses Bootstrap SASS with a well-organized template structure, your assets will be concatenated, minified, and placed correctly, you'll have a working Gulp setup with live reload, you'll get free SSL and DDOS protection from CloudFlare and free hosting without having to worry about server management from OpenShift.

Local Setup
-----------
1. Clone the [OpenShift WordPress Developer
Quickstart](https://github.com/openshift-quickstart/openshift-wordpress-developer-quickstart).
(More on this in [OPENSHIFT.md](OPENSHIFT.md))
1. Make yourself a database and update `wp-config.php`.
1. Point Apache or MAMP at the appropriate place and fire up your web browser!
1. Run the famous WordPress 5-minute install.
1. Probably best to update WordPress to the latest version now.
1. Download the [Sage starter theme](https://github.com/roots/sage/releases/latest)
by roots.io.
1. Check out the [Sage README.md](wp-content/themes/sage/README.md), and set up
Gulp/Bower accordingly. (You must do this from `wp-content/themes/sage`.)
1. Find `manifest.json` and change `example.dev` to `localhost`
1. In your theme directory, run `gulp watch` and check out your new site!

Deploy to OpenShift
-------------------
I choose OpenShift because it's free, and it's a repo-backed system whose
server I basically never want to worry about. YMMV, and if you're a bigger site
with more interactive features or heavier plugins, you may need something with
a bit more kick.

1. Install the Red Hat Cloud client tools ([docs](https://developers.openshift.com/managing-your-applications/client-tools.html)) and run `rhc setup`.
1. Make a new app per the instructions in [OPENSHIFT.md](OPENSHIFT.md) with `rhc app create wordpress php-5.4 mysql-5.5 --from-code=https://github.com/openshift-quickstart/openshift-wordpress-developer-quickstart.git`
1. Open your site in a browser and run the WordPress install. (This one was [http://wordpress-theopen.rhcloud.com/](http://wordpress-theopen.rhcloud.com/).)
1. Make sure to remove `dist` from the theme's `.gitignore` file, because this project isn't currently set up for OpenShift to handle the Gulp tasks.
1. Find the ridiculously long ssh link for your site and add a corresponding remote with `git remote add rhc ssh://57a76be62d52716f83000007@wordpress-theopen.rhcloud.com/~/git/wordpress.git`.
1. Deploy your site with `git push rhc`. (The first time through, you may need to force it with `git push -f rhc`.)

Plugins and Basic Performance Tuning
------------------------------------
1. Set up CloudFlare.
1. Install and activate these plugins:
  1. Amazon Web Services -- required for WP Offload
  1. WP Offload S3 Lite -- required for media storage on OpenShift.
  **WARNING: this step is required to ensure that uploads are permanent on s3
  instead of temporary on OpenShift's filesystem.**
  1. Autoptimize -- be sure to check these advanced settings
    1. :white_check_mark: Optimize JavaScript Code
    1. :x: Force JavaScript in < head >
    1. :white_check_mark: Also aggregate inline JS
    1. :white_check_mark: Optimize CSS Code
    1. :white_check_mark: Also aggregate inline CSS
  1. WP-Optimize -- cleans up your database
  1. Yoast SEO -- proper SEO management
1. Tell OpenShift to use "hot deployments", which are just fine for most
WordPress setups, by adding a blank file at `.openshift/markers/hot_deploy`.

Make the Theme Your Own
-----------------------
1. In this repo I'm copying the theme for my own purposes. `sage` remains the
default theme, and `the-open.net` is the new one. There are a couple of
performance enhancements below that involve modifying some theme files, and
usually you should apply them to your theme, but you'll want to test them out
yourself.
1. I'm copying from an existing Bootstrap SASS site (Jekyll) so I'm just going
to replace the `_variables.scss` file and check that out -- the colors are already looking more like mine!
1. Next, take the rest of my existing `main.scss` and put it here, ensuring we get the dependencies right, but not the placement in compontent or layout files (we can do that later).


Theme-Specific Optimizations
----------------------------
1. Remove unused Bootstrap components from includes list in `_bootstrap.scss`
1. To shrink your scripts even further, go to Autoptimize's option "Exclude
scripts from Autoptimize" and add "jquery" to the list. (It will also
exclude jQuery migrate.) Then add them back into the HTML in `base.php` just
above `wp_footer()`. Most users will have these files cached from other websites
so they'll only load one rather small concatenated script from our site.
