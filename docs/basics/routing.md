# Routing

WordPress handles incoming request with a help of template hierarchy and [conditional]() tags. Each time you got to archive page. WordPress get appropriate template

Kawa routing based on callback handlers

## WordPress Conditional Routing

### Front page

Handles request on static front page

```php
Route::isFrontPage(/** callback */);
```

### Home (blog archive page)

Handles request on posts archive pages (Page, defined as Page for Posts in admin settings)

```php
Route::isHome(/** callback */);
```

> Note: on first installation home page ('/') will return true for both `is_front_page()` and `is_home()`. This mean both routes are satisfies this condition

### Archive page

Determines whether the query is for an existing archive page

```php
Route::isArchive(/** callback */);
```

### Attachment page

Determines whether the query is for an existing attachment page

```php
Route::isAttachment(/** callback */, int|string|array $attachment = '');
```

### Author archive

Determines whether the query is for an existing attachment page

```php
Route::isAuthor(/** callback */, int|string|int[]|string[] $author = '');
```

`$author` - User ID, nickname, nicename, or array of such to check against

### Category archive

Determines whether the query is for an existing category archive page

```php
Route::isCategory(/** callback */, int|string|int[]|string[] $category = '');
```

`$category` - Category ID, name, slug, or array of such to check against

### Date archive

Determines whether the query is for an existing date archive

```php
Route::isDate(/** callback */);
```

### Day archive

Determines whether the query is for an existing day archive

```php
Route::isDay(/** callback */);
```

### Month archive

Determines whether the query is for an existing month archive

```php
Route::isMonth(/** callback */);
```

### Page

Determines whether the query is for an existing single page

```php
Route::isPage(/** callback */, int|string|int[]|string[] $page = '');
```

`$page` - Page ID, title, slug, or array of such to check against

### Page template

Determines whether the current post uses a page template

```php
Route::isPageTemplate(/** callback */, string|string[] $template = '');
```

`$template` - The specific template filename or array of templates to match

### Paged archive

Determines whether the query is for a paged result and not for the first page

```php
Route::isPaged(/** callback */);
```

### Post type archive

Determines whether the query is for an existing post type archive page

```php
Route::isPostTypeArchive(/** callback */, string|string[] $post_types = '');
```

`$post_types` - Post type or array of posts types to check against

### Privacy policy page

Determines whether the query is for the Privacy Policy page

```php
Route::isPrivacyPolicy(/** callback */);
```

### Search results page

Determines whether the query is for a search.

```php
Route::isSearch(/** callback */);
```

### Single post page

Determines whether the query is for an existing single post

```php
Route::isSingle(/** callback */, int|string|int[]|string[] $post = '');
```

`$post` - Post ID, title, slug, or array of such to check against

### Singular post type page

Determines whether the query is for an existing single post of any post type (post, attachment, page, custom post types).

```php
Route::isSingular(/** callback */, string|string[] $post_types = '');
```

`$post_types` - Post type or array of post types to check against

### Sticky post page

Determines whether a post is sticky.

```php
Route::isSticky(/** callback */, int $post_id);
```

`$post_id` - Post ID. Default is the ID of the global $post

### Tag archive

Determines whether the query is for an existing tag archive page.

```php
Route::isTag(/** callback */, int|string|int[]|string[] $tag = '');
```

`$category` - Tag ID, name, slug, or array of such to check

### Custom taxonomy archive

Determines whether the query is for an existing custom taxonomy archive page.

```php
Route::isTax(/** callback */, string|string[] $taxonomy = '', int|string|int[]|string[] $term = '');
```

`$category` - Taxonomy slug or slugs to check against
`$term` - Term ID, name, slug, or array of such to check against

### Time archive

Determines whether the query is for a specific time.

```php
Route::isTime(/** callback */);
```

### Year archive

Determines whether the query is for an existing year archive.

```php
Route::isYear(/** callback */);
```

### 404 Not found page

Determines whether the query has resulted in a 404 (returns no results).

```php
Route::is404(/** callback */);
```

### Call amy conditional

If you wish to call any conditional tag, you may use `condition` route

```php
Route::condition(string|array $condition, /** callback */);
```

`$category` - Any conditional

```php
Route::condition('is_home', /** callback */);

// same as
Route::isHome(/** callback */);

Route::condition(['is_page', 17], /** callback */);

// same as
Route::isPage(/** callback */, 17);
```


- uri routes
- group
- nested groups
- handlers
- name
- middleware
- view routes
- responses