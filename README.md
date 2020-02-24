# Enqueueing Scripts And Styles

## Description

When you’re creating your theme, you may want to create additional stylesheets or JavaScript files. However, remember that a WordPress website will not just have your theme active, it will also be using many different plugins. So that everything works harmoniously, it’s important that theme and plugins must load scripts and stylesheets using the standard WordPress method. This will ensure the site remains efficient and that there are no incompatibility issues. The proper way to add scripts and styles to your theme is to _enqueue_ them in the `functions.php` files. The `style.css` file is required in all themes, but it may be necessary to add other files to extend the functionality of your theme. It may be possible to add scripts and styles to your Wordpress admin pages, so you have to enqueue scripts or style to admin pages. These serve to help improve the look and feel of your admin pages, or to expand the functionality of admin pages.

* * *

## Objectives

At the end of this lesson, you will be able to:

*   Enqueue styles in the functions.php file rather than using @import in the .css file.
*   Identify whether you want to enqueue CSS or script.
*   Decide where you want to enqueue style or script in: admin side or front side.

* * *

## Prerequisite Skills

You will be better equipped to work through this lesson if you have experience in and familiarity with:

*   Scripting knowledge ( [JavaScript ](https://codex.wordpress.org/Using_Javascript))
*   Cascading Style Sheets ( [CSS](https://make.wordpress.org/training/handbook/theme-school/intro-to-css/) )
*   How to use [Custom Functions in WordPress](https://make.wordpress.org/training/handbook/theme-school/what-to-include-in-functions-php/)
*   The use of [add_action()](https://developer.wordpress.org/reference/functions/add_action/)

* * *

## Assets

*   WordPress installation with default Twenty Seventeen theme

* * *

## Screening Questions

*   Are you familiar with Scripts and CSS?
*   Can you create a custom function?
*   Are you familiar with hooks?
*   Are you ready to customize some features of a WordPress theme?

* * *

## Teacher Notes

*   **Time Estimate:** 45 minutes
*   Adding scripts and styles to WordPress is a fairly simple process. Essentially, you will create a function that will enqueue all of your scripts and styles.
*   When enqueuing a script or stylesheet, WordPress creates a handle and path to find your file and any dependencies it may have (like jQuery) and then you will use a hook that will insert your scripts and stylesheets.
*   Emphasize that the student should never enqueue any scripts or styles under **'wp_head'** hook.

* * *

## Hands-on Walkthrough

### Enqueuing Styles

Before enqueuing any style, a safe way to start is to register a CSS style that you want to enqueue. Use this function to register a style : **wp_register_style( $handle, $src, $deps, $ver, $media );**

*   Parameters Description

**$handle**

(string) (required) Name of the stylesheet (which should be unique as it is used to identify the script in the whole system).

Default: None

**$src**

(string) (required) URL to the stylesheet. You should never use hardcode URLs to local styles, instead use plugins_url() (for Plugins) and get_template_directory_uri() (for Themes) to get a proper URL.

Default: None

**$deps**

(array) (optional) Array of handles of any stylesheets that this stylesheet depends on. Dependent stylesheets will be loaded before this stylesheet.

Default: array()

**$ver**

(string|boolean) (optional) String specifying the stylesheet version number, if it has one. This parameter is used to ensure that the correct version is sent to the client regardless of caching, and so should be included if a version number is available and makes sense for the stylesheet. The version is appended to the stylesheet URL as a query string, such as _?ver=3.5.1_. By default, or if false, the function uses the WordPress version string. If null, nothing is appended to the URL.

Default: false

**$media**

(string) (optional) String specifying the media for which this stylesheet has been defined. Examples: 'all', 'screen', 'handheld', 'print'. See this list for the full range of valid CSS-media-types.

Default: 'all'

#### Example of register Styles

// Register style sheet. add_action( 'wp_enqueue_scripts', 'register_plugin_styles' ); /** * Register style sheet. */ function register_plugin_styles() { // Register Plugin style wp_register_style( 'my-plugin', plugins_url( 'my-plugin/css/plugin.css' ) ); // Register them style wp_register_style( 'data_table_css', get_template_directory_uri() . '/css/jquery.dataTables.css' ) } After you register the style you can enqueue the register style with this function: **wp_enqueue_style( $handle, $src, $deps, $ver, $media );** This uses the same parameters/arguments as the register style function.

#### Example of Enqueue Styles

```PHP
// Enqueue style sheet. 
add_action( 'wp_enqueue_scripts', 'register_styles' ); 
/* Enqueue Register style sheet. */
function register_styles() { 
   // Register Plugin style 
   wp_register_style( 'my-plugin', plugins_url( 'my-plugin/css/plugin.css' ) );
   // Enqueue Style 
   wp_enqueue_style( 'my-plugin' );
   // Register them style 
   wp_register_style( 'data_table_css', get_template_directory_uri() . '/css/jquery.dataTables.css' );
   // Enqueue Style
   wp_enqueue_style( 'data_table_css' );
}
```

### Enqueuing Scripts

Same as style Before enqueuing any script, a safe way is first to register a script which you want to enqueue. **wp_register_script( $handle, $src, $deps, $ver, $in_footer );**

*   Parameters Description

**$handle**

(string) (Required) Name of the script. Should be unique.

Default: None

**$src**

(string) (required) URL to the Script. You should never hardcode URLs to local styles, use plugins_url()(for Plugins) and get_template_directory_uri() (for Themes) to get a proper URL.

Default: None

**$deps**

(array) (Optional) An array of registered script handles this script depends on.

Default: array()

**$ver**

(string|bool|null) (Optional) String specifying script version number, if it has one, which is added to the URL as a query string for cache busting purposes. If version is set to false, a version number is automatically added equal to current installed WordPress version. If set to null, no version is added.

Default: false

**$in_footer**

((bool) (Optional) Whether to enqueue the script before `<body>` instead of in the `<head>`. Default is 'false'.

Default: 'false'

#### Example of register Script

// Register Script. add_action ('wp_enqueue_scripts', 'wpb_adding_scripts'); /** * Register Script. */ function wpb_adding_scripts() { // Register Plugin Script wp_register_script('my_amazing_script', plugins_url('amazing_script.js', _FILE_), array('jquery'), '1.1', true); // Register them Script wp_register_script('data_table_script', get_template_directory_uri().'/js/jquery.dataTables.js' ); } After you register the script you are ready to enqueue the register script with this function: **wp_enqueue_script(( $handle, $src, $deps, $ver, $in_footer );** Which used same parameter/argument as Register Script function.

#### Example of Enqueue Script

```PHP
// Enqueue style sheet.
add_action( 'wp_enqueue_scripts', 'enqueue_register_scripts' );
/* Enqueue Register style sheet. */
function enqueue_register_scripts() {
   // Register Plugin Script 
   wp_register_script('my_amazing_script', plugins_url('amazing_script.js', _FILE_), array('jquery'), '1.1', true);
   // Enqueue Style
   wp_enqueue_script('my_amazing_script');
   // Register them style
   wp_register_script('data_table_js', get_template_directory_uri() . '/css/jquery.dataTables.css' );
   // Enqueue Style
   wp_enqueue_script('data_table_js');
}
```

You should call the function using the **wp_enqueue_scripts** action hook if you want to call it on the front-end of the site, like in the examples above. To call it on the administration screens, use the **admin_enqueue_scripts** action hook. For the login screen, use the **login_enqueue_scripts** action hook. Calling it outside of an action hook can lead to problems, see the ticket [#11526](https://core.trac.wordpress.org/ticket/11526) for details.

#### Example: Target a Specific Admin Page to load Scripts and Styles

```PHP
/* Enqueue style sheet and scripts for the admin side. */
function my_enqueue($hook) { 
   if ( 'edit.php' != $hook ) { return; }
   wp_register_script('my_custom_script', get_template_directory_uri() . '/myscript.js', array('jquery'), '1.1', true);
   wp_enqueue_script( 'my_custom_script');
   wp_register_style( 'custom_wp_admin_css', get_template_directory_uri() . '/css/jcustom_admin_css.css' );
   wp_enqueue_style( 'custom_wp_admin_css');
}
add_action( 'admin_enqueue_scripts', 'my_enqueue' );
```

* * *

## Exercises

Now you are able to Enqueue scripts and styles to add various them to your theme. Here are a few exercises that you can perform to explore and practice your techniques.

*   Register any plugin style and enqueue it on your theme.
*   Create some custom JavaScript and add it to your them.
*   Create a custom style to add on to the admin pages, and enqueue it.
*   Enqueue custom JavaScript when the user is logged in to your system.

* * *

## Quiz

**What should you use to enqueue scripts or style?**

1.  Action Hook
2.  Filter
3.  CSS
4.  JavaScript

**Answer:** 1. Action Hook

**Which Action should you use to enqueue a script on the admin side.**

1.  login_enqueue_scripts
2.  admin_enqueue_scripts
3.  wp_enqueue_scripts
4.  none

**Answer:** 2. admin_enqueue_scripts
