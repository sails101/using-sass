How to compile SASS in [Sails](http://sailsjs.org)
==================================================
> Build: Sails 0.10.x

## Sass task

Run this task with the grunt sass command.

Sass is a preprocessor that adds nested rules, variables, mixins and functions, selector inheritance, and more to CSS. Sass files compile into well-formatted, standard CSS to use in your site or application.

This task requires you to have [Ruby](http://www.ruby-lang.org/en/downloads/) and [Sass](http://sass-lang.com/download.html) installed. If you're on OS X or Linux you probably already have Ruby installed; test with ruby -v in your terminal. When you've confirmed you have Ruby installed, `run gem install sass` to install Sass.

NOTE: Above text taken from [grunt-contrib-sass](https://www.npmjs.org/package/grunt-contrib-sass)

## Set up and Configure

* Create a new Sails app: `sails new path_to_app`
* Install dependency: `npm install --save grunt-contrib-sass`
* Add `importer.scss` file to the `assets` > `styles`
```
/**
 * importer.scss
 *
 * By default, new Sails projects are configured to compile this file
 * from SASS to CSS.  Unlike CSS files, SASS files are not compiled and
 * included automatically unless they are imported below.
 *
 * The SASS files imported below are compiled and included in the order
 * they are listed.  Mixins, variables, etc. should be imported first
 * so that they can be accessed by subsequent SASS stylesheets.
 *
 * (Just like the rest of the asset pipeline bundled in Sails, you can
 * always omit, customize, or replace this behavior with SASS, SCSS,
 * or any other Grunt tasks you like.)
 */

@import 'styles.scss';

// For example:
//
// @import 'foundation.scss';
//
// etc.
```
* Add `sass.js` file to `tasks` > `config`, add config settings for SASS:
```javascript
module.exports = function(grunt) {

	grunt.config.set('sass', {
		dev: {
			files: [{
				expand: true,
				cwd: 'assets/styles/',
				src: ['importer.scss'],
				dest: '.tmp/public/styles/',
				ext: '.css'
			}]
		}
	});

	grunt.loadNpmTasks('grunt-contrib-sass');
};
```
* Add line `sass:dev` to `register` > `compileAssets.js`:
```javascript
module.exports = function (grunt) {
	grunt.registerTask('compileAssets', [
		'clean:dev',
		'jst:dev',
		'less:dev',
		'sass:dev', <-- Add that
		'copy:dev',
		'coffee:dev'
	]);
};
```
* Add line 'sass:dev' to `register` > `syncAssets.js`:
```javascript
module.exports = function (grunt) {
	grunt.registerTask('syncAssets', [
		'jst:dev',
		'less:dev',
		'sass:dev', <-- Add that
		'sync:dev',
		'coffee:dev'
	]);
};
```

## Done!
That's it! On server start with `sails lift`, the SCSS imported to `importer.scss` will compile and create `importer.css`:

```
/* importer.scss
 *
 * By default, new Sails projects are configured to compile this file
 * from SASS to CSS.  Unlike CSS files, SASS files are not compiled and
 * included automatically unless they are imported below.
 *
 * The SASS files imported below are compiled and included in the order
 * they are listed.  Mixins, variables, etc. should be imported first
 * so that they can be accessed by subsequent SASS stylesheets.
 *
 * (Just like the rest of the asset pipeline bundled in Sails, you can
 * always omit, customize, or replace this behavior with SASS, SCSS,
 * or any other Grunt tasks you like.)
 */
body {
  font: 100% Helvetica, sans-serif;
  color: #333333; }
```