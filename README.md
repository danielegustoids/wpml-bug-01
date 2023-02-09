# wpml-bug-01
it sems that custom url rewrite get broken with translated taxonomies

# what we do
i want to use a structure like:
domain/taxonomy-name/term-name/custom-post-type-slug  

so we created the post-type and the taxonomy:  

```
function CUSTOM_test_rewrite() {


    register_post_type(
        'hotel',
        array(
            'labels' => ['name' => 'Hotels', 'singular_name' => 'Hotel'],
            'public' => true,
            'publicly_queryable' => true,
            'show_ui' => true,
            'show_in_menu' => true,
            'query_var' => true,
            'taxonomies' => array('city_category'),
            'rewrite' => array( 'slug' => 'citta/%city_category%', 'with_front' => false ),
            'has_archive' => 'citta',
            'capability_type' => 'post',
            'hierarchical' => false,
        )
    );

    register_taxonomy(
        'city_category',
        'hotel',
        array(
            'rewrite' => array( 'slug' => 'citta', 'with_front' => false ),
            'show_in_nav_menus' => true,
            'labels' => ['name' => 'Cities', 'singular_name' => 'City'],
            'hierarchical' => true,
        )
    );
    
}

add_action( 'init', 'CUSTOM_test_rewrite', 10 );


add_filter('post_type_link', 'CUSTOM_test_rewrite_permalinks', 10, 4);
function CUSTOM_test_rewrite_permalinks($post_link, $post, $leavename, $sample) {
    if (false !== strpos($post_link, '%city_category%')) {
        $projectscategory_type_term = get_the_terms($post->ID, 'city_category');
        if (!empty($projectscategory_type_term))
            $post_link = str_replace('%city_category%', array_pop($projectscategory_type_term)->slug, $post_link);
        else
            $post_link = str_replace('%city_category%', 'uncategorized', $post_link);
    }
    return $post_link;
}
```


it works like a charm when, wpml is installed but:
- custom-post-type is not translated
- taxonomy is not translated

# second step
- we define custom-post-type as translatable with the following slugs:  
-- IT (main lang): citta/%city_category%  
-- EN: city/%city_category%  
-- FR: ville/%city_category%  

