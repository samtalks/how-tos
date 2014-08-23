## Setting up Wordpress with forums

This is a typical setup of wordpress I'd like to use to get going.

After installing Wordpress, install the following plugins and themes and activate them all:
* [bbpress plugin](https://wordpress.org/plugins/bbpress/)
* [Simple custom CSS plugin](https://wordpress.org/plugins/simple-custom-css/screenshots/) (Make sure to backup your css frequently)
* [Mailchimp for wordpress plugin](http://wordpress.org/plugins/mailchimp-for-wp/) (Don't accept immitations!)
* [Nudie theme](http://nudiewp.com/) (It's around $50).

#### Pages
* To have a separate forum section, create these pages at a minimum: **Home, Blog, Forums**

#### Forums
* Create a main forum section with all defaults selected. Call it anything you'd like.

#### Topics
* Create a topic with defaults selected except: nest it under the forum you just created above.

#### Appearance (Widgets)

  ##### bbPress widget
  * Add the bbPress login widget at the top and title it something like "Forum Login" so that it works when both logged in or out. Leave other URIs blank.
  * Add a text widget immediately below with a blank title and the following code:
    ```html
    <a href="http://[YOUR DOMAIN]/wp-login.php?action=register">Create a login account here</a><br>
    ```
  * Add the Mailchimp signup form widget and name it something like "Join our Newsletter"
  
  #### Primary widget
  * Add the Mailchimp signup form widget and name it something like "Join our Newsletter"

#### Appearance (Menus)
* Create a new menu and name it something like "Main". On **Menu settings** at the bottom, add it to the Theme locations **Primary** and **Save** it.
* Add a menu item for each of the pages you created. 

#### Appearance (Custom CSS)
* Add your custom CSS. Make sure you frequently save and version control (i.e., on Git) your ever changing CSS file since this plugin does not save versions or backups.

#### Appearance (Editor)
* This one is important. There is a bug with the bbpress plugin that allows Wordpress to highlight the wrong menu item when the forum sub-items and topics are selected. To fix this, add the following code at the bottom of the `ThemeFunctions` (`functions.php`) file.
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
  ```

