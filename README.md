# wpml-bug-01
it sems that custom url rewrite get broken with translated taxonomies

# what we do
i want to use a structure like:
domain/taxonomy-name/term-name/custom-post-type-slug

it works like a charm when, wpml is installed but:
- custom-post-type is not translated
- taxonomy is not translated

# second step
- we define custom-post-type as translatable with the following slugs:

-- IT (main lang): citta/%city_category% 
-- EN: city/%city_category% 
-- FR: ville/%city_category% 
