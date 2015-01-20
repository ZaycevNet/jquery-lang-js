# Instantly switch content languages client-side
### Change language without reloading the page or doing a request to the server. See the example provided in index.html

This jQuery plugin allows you to create multiple language versions of your content by supplying phrase
translations from a default language such as English to other languages.

## Live Production-Ready Example
Load https://www.orbzu.com and click the flag top-left of the screen. Select a different language to see the page change languages automatically.

## Features
* Instant language switching - page no reload required
* Automatic translation of dynamic sections of the page (e.g. loaded via AJAX or added via jQuery)
* Language persistence across pages and reloads via cookie (requires jquery-cookie.js plugin)
* Supports regular expression search / replace for non-token-based translations
* Supports changing image urls when language changes
* Event hooks for custom processing
* No intervals or timeouts, just high-performance code
 
## Who Made This?
This plugin and all related code was created by Irrelon Software Limited, a U.K. registered company with a mission to create awesome web applications and push the boundries of app development. Check us out at http://www.irrelon.com

#### LinkedIn
http://uk.linkedin.com/pub/rob-evans/25/b94/8a5/

#### French Translation of Code
https://github.com/emgepub/jquery-lang-js

# Changelog
2015-01-09 - Version 2.8
* Added support for changing images when the language changes

2015-01-07 - Version 2.7
* Removed console.log() calls so that old versions of Internet Explorer won't break

2014-06-06 - Version 2.6
* Changed _getTextNodes & _setTextNodes methods to work under IE8. _getTextNodes returns now array of objects where every object have two properties:
	- node - current text node
	- langDefaultText - remember data of current text node 

2014-04-19 - Version 2.5
* Changed dynamically loaded language packs to JSON format. This allows the packs to be loaded by other programming languages such as PHP that can natively interpret JSON but not JavaScript.

2014-02-22 - Version 2.4
* Added dynamic loading of language packs so they don't need to be loaded up front, saving on HTTP requests and memory usage

2014-02-02 - Version 2.3
* Fixed bug when switching from non-default language to other non-default language

2014-02-01 - Version 2.2
* Check that a language pack actually exists before trying to switch to it
* Allow disabling event firing so dynamic elements wont fire update events
* Allow cookies to override passed current language on constructor if passing true as third arg to new Lang()
* Fix to allow auto-translate when using jQuery appendTo() method
* Fix for dynamic translation of root-element text nodes

2014-02-01 - Version 2.1
* Fixed break in jQuery chaining
* Added cookies for persistence when $.cookie exists
* Supports automatic translation for dynamically added content (when added via jQuery)
* Added support for regular expression matches
* Added support for translating text nodes mixed with html nodes

2014-01-31 - Version 2.0
* Complete plugin re-write

# How to use

*If you want language selection to persist across pages automatically, please ensure you include the
jquery-cookie plugin available from: https://github.com/carhartl/jquery-cookie on your page as well.*

Include the plugin script in your head tag.

    <script src="js/jquery-lang.js" charset="utf-8" type="text/javascript"></script>

Add the following to your HTML page in either the head or body:

    <script type="text/javascript">
    // Create language switcher instance and set default language to en
	window.lang = new Lang('en');
    </script>

# Loading Language Packs Dynamically (Recommended)
Instead of loading all the language packs your site provides up front, it can be useful to only load the packs when the
user requests a language be changed. The plugin allows you to simply define the packs and their paths and then it will
handle loading them on demand. To define a language pack to load dynamically call the lang.dynamic() method after the
plugin has loaded and been instantiated:

	// Define the thai language pack as a dynamic pack to be loaded on demand
	// if the user asks to change to that language. We pass the two-letter language
	// code and the path to the language pack js file
	window.lang.dynamic('th', 'js/langpack/th.json');

## Including Language Packs Up-Front (Optional)
*The recommended way to use language packs is to define them dynamically (see Loading Language Packs Dynamically above)*

You can include any language pack you have created up-front so they are ready to use straight away on your page. If you
do it this way you must make the language pack a JS file and include a line of code to insert the pack into the Lang 
prototype. See the ./langPack/nonDynamic.js file for an example. Pay attention to the regex section as well as this needs
to define regex patterns directly instead of as strings.

	<script src="js/langpack/nonDynamic.js" charset="utf-8" type="text/javascript"></script>

# Defining which elements to translate

In the HTML content itself you can denote an element as being available for translation by adding a "lang"
attribute with the language of the content as such:

    <span lang="en">Translate me</span>

Or any element with some content such as:

    <select name="testSelect">
        <option lang="en" value="1">An option phrase to translate</option>
        <option lang="en" value="2">Another phrase to translate</option>
    </select>

# Button elements

The plugin will automatically translate button element text when defined like this:

    <button lang="en">Some button text</button>

# Placeholder text

You can also translate placeholder text like this:

    <input type="text" placeholder="my placeholder text" lang="en" />

When you change languages, the plugin will update the placeholder text where a translation exists.

# Translating other text in JavaScript such as alert() calls

If you need to know the current translation value of some text in your JavaScript code such as when calling
alert() you can use the translate() method:

    alert(window.lang.translate('Some text to translate'));

# Dynamic content

If you load content dynamically and add it to the DOM using jQuery the plugin will AUTOMATICALLY translate
to the current language. Ensure you have added "lang" attributes to any HTML that you want translated BEFORE
you add it to the DOM.

# Switching languages

To switch languages simply call the .change() method, passing the two-letter language code to switch to. For
instance here is a switcher that will change between English and Thai as used in the example page:

    <a href="#lang-en" onclick="window.lang.change('en'); return false;">Switch to English</a> | <a href="#lang-en" onclick="window.lang.change('th'); return false;">Switch to Thai</a>

The onclick event is the only part that matters, you can apply the onclick to any element you prefer.

# Define a language pack

Language packs are defined in JSON files and are added to the plugin like so:

    {
    	// Define token (exact phrase) replacements here
    	"token": {
			"Property Search":"ค้นหา",
			"Location":"สถานที่ตั้ง",
			"Budget":"งบประมาณ",
			"An option phrase to translate":"งบประมาณงบประมาณสถานที่ตั้ง",
		},
		// Define regular expression replacements here
		"regex": [
			["My regex", "i", "someReplacement"]
		]
    }

That example just defined a language pack to translate from the default page language English into Thai (th).
Each property inside the token object has a key that is the English phrase, then the value is the Thai equivalent.

The regex section allows you to specify regular expressions to use for matching and replacement. The regular expressions
are only evaluated IF a token-based replacement is not found for a section of text.

It's that simple!

### Using Dynamic Packs
When you call lang.change('th'), the plugin will check if the language pack is already loaded and if not, will load
the pack first before changing languages. Once the pack file has been loaded the plugin will change to that language.

Loading languages dynamically is only done when the the change() method is called. This means if you request a
translation of a string via the translate() method BEFORE the language pack for that language is loaded the translation
will fail.

If you require a language pack to be loaded and don't want to change the page language you can request loading the pack
manually by calling the loadPack() method like so:

	lang.loadPack('th');

If you need to know when the pack has been loaded pass a callback method:

	lang.loadPack('th', function (err) {
		if (!err) {
			// The language pack loaded
		} else {
			// There was an error loading the pack
		}
	});

# Upgrades and Pull Requests

We actively encourage you to upgrade this plugin and provide pull requests to share your awesome work! Please ensure
that any changes you make are:

1. Non-breaking changes
2. Tested thoroughly against the latest version
3. Documented with the JSDoc standard as the other methods are

Thank you to everyone who takes their time to write updates to the plugin!

# Roadmap

* **COMPLETED** - Dynamic language pack loading
* **COMPLETED** - Swap image assets based on language
* Swap video assets based on language

# License

This plugin and all code contained is Copyright 2014 Irrelon Software Limited. You are granted a license to use
this code / software as you wish, free of charge and free of restrictions under the MIT license. See the source
code file jquery-lang.js for the full license details.

If you like this project, please consider Flattr-ing it! http://bit.ly/qCEbTC

This project is updated and maintained by:

Irrelon Software Limited
http://www.irrelon.com