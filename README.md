wp-ezclasses-docs-todo
======================

Part TODO list, part roadmap (and part garbage collection, sorry).

These are ideas, links, snippets and such related to (possible) future efforts for the WordPress framework WPezClasses.

====================================================================================================================================

http://www.onextrapixel.com/2012/10/08/10-tips-for-a-deeply-customised-wordpress-admin-area/
	
http://www.catswhocode.com/blog/wordpress-hacks-and-snippets-to-efficiently-reduce-spam

http://headjs.com/

http://www.catswhocode.com/blog/10-awesome-php-functions-and-snippets

http://www.catswhocode.com/blog/wordpress-snippets-to-work-with-social-networks

http://www.catswhocode.com/blog/wordpress-snippets-hacks-and-tips-to-enhance-your-comments-section

http://www.catswhocode.com/blog/wordpress-hacks-and-snippets-to-efficiently-reduce-spam

http://www.catswhocode.com/blog/wordpress-dashboard-hacks-for-developers-and-freelancers

http://www.catswhocode.com/blog/super-useful-wordpress-action-hooks-and-filters

http://digwp.com/2010/03/wordpress-functions-php-template-custom-functions/

http://niroze.wordpress.com/2011/09/04/common-set-of-custom-wordpress-functions/

http://digwp.com/2010/04/wordpress-custom-functions-php-template-part-2/

http://digwp.com/2010/02/template-heirarchy-chart/

http://digwp.com/2009/09/tumblr-style-links-for-posts-and-feeds/

http://digwp.com/2009/09/tumblr-style-links-for-posts-and-feeds/
http://digwp.com/2011/04/tumblr-links-post-formats/

// -http://wordpressapi.com/2010/12/08/wordpress-pagination-style-wordpress-plugin/


// ------------------------------------------------------------------------------------------------

- Invesitgate the possbility of creating a class for the code, and one for the geographic entities to ez-tize this plugin. 
http://wordpress.org/plugins/gf-forms-uk-address-format/

// ------------------------------------------------------------------------------------------------

// ------------------------------------------------------------------------------------------------

- Per FB AWP: Second answer said to be the one to read
http://wordpress.stackexchange.com/questions/28359/how-to-require-a-minimum-image-dimension-for-uploading

// ------------------------------------------------------------------------------------------------

Add modernizer (via CND) to presets enqueue scritps

// ------------------------------------------------------------------------------------------------


 add_action('init', 'cleanup_head');
 
function cleanup_head() {

remove_action( 'wp_head', 'wp_generator' );  // WP version
  
add_filter( 'style_loader_src', 'remove_wp_ver_css_js', 9999 ); // remove WP version from css
  
add_filter( 'script_loader_src', 'remove_wp_ver_css_js', 9999 ); // remove Wp version from scripts

remove_action( 'wp_head', 'index_rel_link' ); // index link

remove_action( 'wp_head', 'parent_post_rel_link', 10, 0 ); // previous link

remove_action( 'wp_head', 'start_post_rel_link', 10, 0 ); // start link

remove_action( 'wp_head', 'adjacent_posts_rel_link_wp_head', 10, 0 ); // links for adjacent posts

remove_action( 'wp_head', 'rsd_link' );

remove_action( 'wp_head', 'wlwmanifest_link' );  // windows live writer

}

// remove WP version from scripts
function remove_wp_ver_css_js( $src ) {
    if ( strpos( $src, 'ver=' ) )
        $src = remove_query_arg( 'ver', $src );
    return $src;
}
 
 
// ------------------------------------------------------------------------------------------------
//
// - wp_ezfunctions_get_the_page_id()
//
if (!function_exists('wp_ezfunctions_get_the_page_id')) {
	function wp_ezfunctions_get_the_page_id() {
		global $wp_query;
		//
		// Some WP templates (e.g., 404.php) won't have this set so we need to test
		//
		if (isset($wp_query->queried_object)){
			return $wp_query->get_queried_object()->ID;
		}
	}
}

//
// - wp_ezfunctions_get_the_page_name_slug()
//
if (!function_exists('wp_ezfunctions_get_the_page_name_slug')) {
	function wp_ezfunctions_get_the_page_name_slug() {
		global $wp_query;
	//
		// Some WP templates (e.g., 404.php) won't have this set so we need to test
		//
		if (isset($wp_query->queried_object)){
			return  $get_the_page_object = $wp_query->get_queried_object()->post_name;
		}
	}
}


/// ------------------------------------------------------------------------------------------------

NOTE: This might already be in the status class. Double check

// - wp_ezfunctions_get_domain_name()
//
// -- strips off the 'www' and returns just the domain name. 
//
if (!function_exists('wp_ezfunctions_get_domain_name')) {
	function wp_ezfunctions_get_domain_name() {
		$http_host = $_SERVER['HTTP_HOST'];
		$http_host = preg_replace('/^www\./i', '$http_host' ,1);
		return $http_host;
	}
}

// ------------------------------------------------------------------------------------------------



// ------------------------------------------------------------------------------------------------
// - wp_ezfunctions_body_class_add_filter()
//
// -- Adds custom classes to the array of body classes.
//
// -- Adds a class of single-author to blogs with only 1 published author
//
// Credit(s): Nicked from Rachael Baker's Boilerstrap WP theme

if (!function_exists('wp_ezfunctions_body_class_add_filter')){
	function wp_ezfunctions_body_class_add_filter( $classes ) {
		
		if ( ! is_multi_author() ) {
			$classes[] = 'single-author';
		}
		return $classes;
	}
}


// ------------------------------------------------------------------------------------------------

NOTE: Probably best to push these onto an array such that the array would tell you the full story of which conditional tags are currently true.

//
// - wp_ezfunctions_get_conditional_tag($wp_ezfunctions_archive_granular)
//
// -- More or less the reverse of the standard conditional tags: http://codex.wordpress.org/Conditional_Tags
//
// -- The parms:
// ---- $wp_ezfunctions_archive_granular - (bool) Allows you to define how you want to aggregate (or not) archive page
// ---- $wp_ezfunctions_blog_granular  - (bool) Allows you to define how to want to aggregate (or not) blog 
//
if (!function_exists('wp_ezfunctions_get_conditional_tag')) {
	function wp_ezfunctions_get_conditional_tag(  $wp_ezfunctions_blog_granular = true, $wp_ezfunctions_archive_granular = true ) {
		if (((!$wp_ezfunctions_blog_granular) && ( is_home() || is_single())) || ( !$wp_ezfunctions_archive_granular && !$wp_ezfunctions_blog_granular && is_archive() )) {
			return 'is-blog';
		} elseif ( is_home()){
			return 'is-home';
		} elseif ( is_front_page() ){
			return 'is-front-page';
		} elseif ( is_page() ){
			return 'is-page';
		} elseif ( is_single() ){
			return 'is-single';
		} elseif ( is_archive() ) {
			if ( !$wp_ezfunctions_archive_granular ) {
				return 'is-archive';
			} elseif ( is_category() ) {
				return 'is-category';
			} elseif ( is_tag() ){ 
				return 'is-tag';
			} elseif ( is_author() ){ 
				return 'is-author';
			} elseif ( is_tax() ){ 
				return ('is-tax');
			} elseif ( is_year() ){ 
				return 'is-year';
			} elseif ( is_date() ){ 
				return 'is-date';
			}	
		} elseif ( wp_ezfunctions_is_blog() ){ // TODO - This next coming into play - without some sort of arg
			return 'is-blog';
		} elseif ( is_404() ){
			return 'is-404';
		} elseif ( is_search() ){
			return 'is-search';
		}
			
	}
}

// ------------------------------------------------------------------------------------------------


Incorporate this idea of target body classes in the ezBS Lay Obj Mngt Sys. Seems easy since the logic in centralized, and won't have to be baked into the function like this is :)

/**
 * Extends the default WordPress body class to denote:
 * 1. Using a full-width layout, when no active widgets in the sidebar
 *    or full-width template.
 * 2. Front Page template: thumbnail in use and number of sidebars for
 *    widget areas.
 * 3. White or empty background color to change the layout and spacing.
 * 4. Custom fonts enabled.
 * 5. Single or multiple authors.
 *
 * @since Twenty Twelve 1.0
 *
 * @param array Existing class values.
 * @return array Filtered class values.
 */
function twentytwelve_body_class( $classes ) {
	$background_color = get_background_color();

	if ( ! is_active_sidebar( 'sidebar-1' ) || is_page_template( 'page-templates/full-width.php' ) )
		$classes[] = 'full-width';

	if ( is_page_template( 'page-templates/front-page.php' ) ) {
		$classes[] = 'template-front-page';
		if ( has_post_thumbnail() )
			$classes[] = 'has-post-thumbnail';
		if ( is_active_sidebar( 'sidebar-2' ) && is_active_sidebar( 'sidebar-3' ) )
			$classes[] = 'two-sidebars';
	}

	if ( empty( $background_color ) )
		$classes[] = 'custom-background-empty';
	elseif ( in_array( $background_color, array( 'fff', 'ffffff' ) ) )
		$classes[] = 'custom-background-white';

	// Enable custom font class only if the font CSS is queued to load.
	if ( wp_style_is( 'twentytwelve-fonts', 'queue' ) )
		$classes[] = 'custom-font-enabled';

	if ( ! is_multi_author() )
		$classes[] = 'single-author';

	return $classes;
}
add_filter( 'body_class', 'twentytwelve_body_class' );


// ------------------------------------------------------------------------------------------------


	/* TODO
		function showMessage($message, $errormsg = false){
			if ($errormsg) {
				echo '<div id="message" class="error">';
			} else {
				echo '<div id="message" class="updated fade">';
			}
			echo "<p><strong>$message</strong></p></div>";
		}  
		*/
		
		// TODO add_action('admin_notices', 'showAdminMessages'); 

		/* TODO
		function showAdminMessages() {
			showMessage("You need to upgrade your database as soon as possible...", true);

			if (user_can('manage_options') {
				showMessage("Hello admins!");
			}
		}
		*/