# Setup Social WP 1: bbPress, Nudie, Mailchimp

This is a typical setup of wordpress I'd like to use to get going.

After installing Wordpress, install the following plugins and themes and activate them all:
* [bbpress plugin](https://wordpress.org/plugins/bbpress/)
* [Simple custom CSS plugin](https://wordpress.org/plugins/simple-custom-css/screenshots/) (Make sure to backup your css frequently)
* [Mailchimp for wordpress plugin](http://wordpress.org/plugins/mailchimp-for-wp/) (Don't accept immitations!)
* [Nudie theme](http://nudiewp.com/) (It's around $50).
* [bbP last post](https://wordpress.org/plugins/bbp-last-post/)
* [bbP private groups](https://wordpress.org/plugins/bbp-private-groups/)

## Pages
To have a separate forum section, create these pages at a minimum: **Home, Blog, Forums**

## Forums
Create a main forum section with all defaults selected. Call it anything you'd like.

## Topics
Create a topic with defaults selected except: nest it under the forum you just created above.

## Appearance
Add only two widgets to keep it as simple as possible.

##### Appearance (Widgets)
* **Primary widget**

  Add the Mailchimp signup form widget and name it something like "Join our Newsletter"

* **bbPress widget**
  * Add the **bbPress login** widget at the top and title it something like "Forum Login" so that it works when both logged in or out. Leave other URIs blank.
  * Add a text widget immediately below with a blank title and the following code:

    ```html
    <a href="http://[YOUR DOMAIN]/wp-login.php?action=register">Create a login account here</a><br>
    ```

  * Add the **Mailchimp signup form** widget and name it something like "Join our Newsletter"

##### Appearance (Menus)
* Create a new menu and name it something like "Main". On **Menu settings** at the bottom, add it to the Theme locations **Primary** and **Save** it.
* Add a menu item for each of the pages you created. 

##### Appearance (Custom CSS)
* Add your custom CSS. Make sure you frequently save and version control (i.e., on Git) your ever changing CSS file since this plugin does not save versions or backups.

##### Appearance (Editor) - fix wrong forum menu highlight
* This one is important. There is a bug with the bbpress plugin that allows Wordpress to highlight the wrong menu item when the forum sub-items and topics are selected. To fix this, add the following code at the bottom of the `ThemeFunctions` (`functions.php`) file. Make sure to substitute `'forum'`, `'topic'` and `'menu-item-16'` in the first script to whatever is correct. 

	```php
	// THIS FIXES THE WRONG FORUM MENU ITEM HIGHLIGHTING
	add_filter( 'nav_menu_css_class', 'namespace_menu_classes', 10, 2 );
	function namespace_menu_classes( $classes , $item ){
	  	if ( get_post_type() == 'forum' || get_post_type() == 'topic' ) {
	    		$classes = str_replace( 'current_page_parent', '', $classes );
	    		$classes = str_replace( 'menu-item-16', 'current_page_parent', $classes );
	  	}
	  	return $classes;
	}

	// THIS REMOVES WORDPRESS LOGO FROM REGISTER PAGE
	
	function my_login_logo() { ?>
	    <style type="text/css">
	        body.login div#login h1 a {
	            display: none;
	        }
	    </style>
	<?php }
	add_action( 'login_enqueue_scripts', 'my_login_logo' );
	```

##### Appearance (Editor) - add Credit to Footer
* Edits the footer to add your design credits and link to website. Make the change to the bottom of `footer.php`.

Wrap this part of code:
```php
				<?php echo apply_atomic_shortcode( 'footer_content', hybrid_get_setting( 'footer_insert' ) ); ?>
```



With other code this:
```php
                        <!-- HIDE DEFAULT FOOTER
				<?php echo apply_atomic_shortcode( 'footer_content', hybrid_get_setting( 'footer_insert' ) ); ?>
AND INSERT YOUR COMPANY CREDITS BELOW
                        -->

                        Copyright &copy; 2014 CLIENT_WEBSITE   &emsp;&emsp;  By <a href="http://YOUR_SITE" target="_blank">YOUR SITE</a>
```

##### Appearance (Editor) - hide "voice count"
* bbPress by default clutters up the topics page with a "voice count" collumn. Do this to your files to remove those elements.

In the `loop-topics.php` file, hide this section like so:
```php
<!--HIDE	<th class="bbp-topic-voice-count"><?php _e( 'Voices', 'bbpress' ); ?></th> -->
```
In the `loop-single-topic.php` file, hide this section like so:
```php
<!--HIDE	<td class="bbp-topic-voice-count"><?php bbp_topic_voice_count(); ?></td>  -->
```


## Settings

##### Settings (General)
* Check on "Anyone can register" for **Membership**.
* During the summer, set **UTC** to EDT (Eastern Daylight Time) which is "UTC-4".  During the winter, set to EST (Eastern Savings Time) which is "UTC-5".
* Set the date and time format to:  `n-j-y`  for "1-1-14" output.

##### Settings (Reading)
**Front page displays** set to "static page" with "Front page" set to "Home" and "Posts page" set to "Blog".

##### Settings (Discussion) - These are for the Blog
At the beginning of your website, lower the barriers (and security) as much as feasible to make it as easy for people to join and participate in your blogs. Increase barriers when you start to see any fraudulent sign ups and spam. 
Uncheck the following:
* "Users must be registered and logged in to comment"
* "Automatically close comments on..."
* "Comments must be manually approved"
* "Enable threaded (nested) comments..."

##### Settings (Media)
* Uncheck "Organize my uploads into months..."

##### Settings (Permalinks)
* Set to **Post Name**

##### Settings (Forums)
* Again, minimize barriers to participation for a new website. On same token, try to reduce the features to keep the UI as clean and simple as possible. So uncheck the following:
	* "Allow users to mark topics as favorites"
	* "Allow users to subscribe to forums and topics"
	* In **Forum Root Slug**, uncheck "Prefix all forum content with the forum root slug". This prevents the "Home/Forums/Forum/..." redundancy in the breadcrumbs.

##### Settings (bbP last post)
* Name the column something short like "Updated" to fit on a phone.

##### Settings (bbP private groups)
* Keep the forum visibility unchecked.


## Mailchimp for WP

##### Mailchimp settings
* First create a Mailchimp account, create a list, add then return to Wordpress admin site to add the API.  

##### Forms
* Add the below:
```html
<p>
  <label for="mc4wp_email">Email address: </label>
  <input type="email" id="mc4wp_email" name="EMAIL" placeholder="email address" required />
<input type="submit" value="Sign up" />
</p>
```

