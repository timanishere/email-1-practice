#General Email Gulp Template



##Basics

Make changes in `src/email.html`.<br />
If adding new components, add all files to their respective folders, making sure to add CSS files in the `SCSS` folder to compile.<br />
There are also [user-defined variables](#variables) you may need to change.

###Run `gulp` to run dev
* [sass](#tasks-sass) <br />
* [file-include](#tasks-fileinclude) <br />
* [repl-link](#tasks-repllink) <br />
* [utm](#tasks-utm) <br />
* [prettify](#tasks-prettify) <br />
* [webserver](#tasks-webserver) <br />
* [watch](#watch)

###Run `gulp dist` to generate just the distilled (inline) version

* [clean](#tasks-clean) <br />
* [sass](#tasks-sass) <br />
* [file-include](#tasks-fileinclude) <br />
* [repl-link](#tasks-repllink) <br />
* [utm](#tasks-utm) <br />
* [prettify](#tasks-prettify) <br />
* [premailer](#tasks-premailer)

###Run `gulp web` to generate inlined email and web version

* [clean](#tasks-clean) <br />
* [sass](#tasks-sass) <br />
* [replace](#tasks-replace) <br />
* [file-include](#tasks-fileinclude) <br />
* [target-html](#tasks-targethtml) <br />
* [repl-link](#tasks-repllink) <br />
* [utm](#tasks-utm) <br />
* [prettify](#tasks-prettify) <br />
* [premailer](#tasks-premailer)



##<a name="variables"></a>User Defined Variables

###<a name="UTM-tags"></a>UTM Tags
This template has UTM tagging off by default. To turn it on, change `utm.use` to `true`.

This is currently a very hacky and specific way of dealing with UTM tags. It uses the following regex format:<br />`<a id="l[0-9]*" href="http://www.sofa.com/[^"]`

Of course, you can replace this with a different format if you like. The following are notes for using this particular format.

If an `@@include` module is repeated, you may run into the problem where a URL has the UTM tag inserted multiple times into the same link.
To get around this issue, you can:

1. Use `"http://www.sofa.com//"` to differentiate the basic sofa.com link from others.<br />
Without this, `http://www.sofa.com/about-us/why-us` would be `http://www.sofa.com/?UTMTAGS/about-us/why-us`
2. Go in and manually change the id's for each link. This is the safest option.

To turn off UTM, change `utm.use` to `false`.

###Campaign Name
This is for naming the web-version email.<br />
If you delete teh string, it will default to `index.html`<br />
For simplicity, I will be referring to this file as `index.html` in this documentation.



##Tasks
Click on the task name to read the documentation on that particular plugin

###<a href="https://www.npmjs.org/package/gulp-clean" name="tasks-clean" target="_blank" style="color:black;">Clean</a>
Removes all files in the `dist` folder.


###<a href="https://www.npmjs.org/package/gulp-sass" name="tasks-sass" target="_blank" style="color:black;">Sass</a>
Compiles SCSS found at `src/scss/**/*.scss` to CSS.

* `gulp` will place the resulting CSS in `src/css/` folder.
* `gulp dist` and `gulp web` will place the resulting CSS in the `dist/css/` folder.


###<a href="https://www.npmjs.org/package/gulp-file-include" name="tasks-fileincluder" target="_blank" style="color:black;">File Include</a>
Includes external HTML into the specified HTML file. All HTML is sourced from `src/html`

* `gulp` will include the HTML files in `src/email.html`, rename the resulting file as `_tmp.email.html` and place it within the `src/` folder.
* `gulp dist` will include the HTML files in `src/email.html` and place the resulting file within the `dist/` folder.
* `gulp web` will include the HTML files in both `src/email.html` and `dist/index.html` and place the resulting files within the `dist/` folder.


###<a href="https://www.npmjs.org/package/gulp-webserver" name="tasks-webserver" target="_blank" style="color:black;">Webserver</a>
Starts up a webserver for dev at `http://localhost:8000/src/_tmp.email.html`


###<a href="https://www.npmjs.org/package/gulp-premailer" name="tasks-premailer" target="_blank" style="color:black;">Premailer</a>
Uses Ruby Gems to inline the CSS into the email. It inlines all HTML files in the `dist` folder
####Note: When used in `gulp web` it will take twice as long as in `gulp dist` since it is inline twice as many files.


###<a href="https://www.npmjs.org/package/gulp-prettify" name="tasks-prettify" target="_blank" style="color:black;">Prettify</a>
Properly indents HTML for readability reasons. Currently uses an indent size of 2.

* `gulp` prettifies `src/_tmp.email.html`
* `gulp dist` prettifies `dist/email.html`
* `gulp web` prettifies `dist/email.html` and `dist/index.html`


###<a href="https://www.npmjs.org/package/gulp-replace" name="tasks-replace" target="_blank" style="color:black;">Replace</a>
Removes the ET tracking tag for web version and renames this file to the campaign name if specified, otherwise `index.html`.


###<a href="https://www.npmjs.org/package/gulp-targethtml" name="tasks-targethtml" target="_blank" style="color:black;">Target HTML</a>
Replaces HTML with the following format:

    <!--(if target dev)><!-->
    This will appear by default
    <!--<!(endif)-->
    <!--(if target web)>
    This will appear when Target HTML is used.
    <!(endif)-->


###<a href="https://www.npmjs.org/package/gulp-replace" name="tasks-utm" target="_blank" style="color:black;">UTM</a>
Replaces the UTM tag if `utm.use == true`. See [UTM Tags](#UTM-tags) for syntax and other details.

* `gulp` will add UTM tags to `src/_tmp.email.html` and place it back in `src/`.
* `gulp dist` will add UTM tags to `dist/email.html` and place it back in `dist/`.
* `gulp web` will add UTM tags to `dist/email.html` and `dist/index.html` and place both back in `dist/`.


###<a href="https://www.npmjs.org/package/gulp-replace" name="tasks-repllink" target="_blank" style="color:black;">Repl Link</a>
Replaces the problem viewing link.

The link is set to `#prob-viewing-link` by default.<br />
If the campaign name is specified, it will replace it with:<br />
`http://www.sofa.com/newsletter/CAMPAIGNNAME/CAMPAIGNNAME.html`



##Watch

This watches only on `gulp`, since that is the only one that runs the web server.

* If changes are made in `src/scss/`, it will rerun [sass](#tasks-sass).
* If changes are made in `src/email.html` or `src/html/`, it will rerun [fileinclude](#tasks-fileinclude) and [utm](#tasks-utm).