# Pagination

Pagination allows user to page back and forth through multiple pages of content. WordPress provides pagination for posts archives or single post

## Loop pagination

If you wish to build pagination, first of all create list of posts

```php
$posts = Post::get();
```

Hmmm... Nothing has been changed. It is still the same `get()` method which retrieves posts from global query. Where can I specify the amount of posts per page? Why does this method called not `paginate()` or something like?

And here is why and where

When WordPress builds pagination, it refers to global `WP_Query` object. When Kawa Framework resolves appropriate route it know nothing about handler of this route. What does this mean? Let's say you have 8 posts and within admin panel you set to get 3 posts per page (Settings >> Reading >> Blog pages show at most). You would expect 3 pagination pages. So does WordPress it smart enough to count it

Now within your handler method you want to set 2 posts per page like `Post::paginate(2)`. You'll see on a first page - it works. Second, third - everything OK. But when you will look at `/page/4` it will show 404 error. That is because WordPress does nothing about your local query and build pages for 3 posts per pages

In order to fix it we need to hook into `pre_get_posts` hook. If you look at `src/Http/Kernel.php` file you will see `paginated()` method, which should return associative array of `'post_type' => 'posts_per_page'` pairs. So

```php
protected function paginated() : array
{
    return [
        Post::POST_TYPE => 2,
    ];
}
```

Will do the trick. Now you will get correct response on `/page/4`, showing your last 2 posts. This mean there is no need to pass `$perPage` into `Post::paginate(2)` - it will do nothing until you hooked in from Kernel object. That is why we decided to remove this method - to not confuse developers with familiar syntax

`Post::get()` will work as for global pagination (from admin panel) or custom (from Kernel). Consider it like you always get paginated objects. Actually, this is how WordPress works by default

## Getting Pagination

When you create posts Collection, you may access pagination on front in many ways

You can get simple links HTML

```latte
{$posts->pagination->links()|noescape}
```

or shorthand

```latte
{$posts->pagination|noescape}
```

Or you can get HTML list of links

```latte
{$posts->pagination->list()|noescape}
```

Both methods will retrieve same result as WordPress `paginate_links()` for `plain` and `list` pagination types. You also may pass same arguments into method

```latte
{$posts->pagination->list([
    'show_all' => true,
])|noescape}
```

If ypu wish to have more control over building pagination links, you may use `$posts->pagination` as `Pagination` object. It will return an array of `PaginationLink` objects  

```latte
<ul n:if="$posts->pagination">
    {foreach $posts->pagination as $link}
        <li class="">
            <a href="{$link->link}" n:tag-if="$link->isLink">
                <span>
                    {$link->label|noescape}
                </span>
            </a>
        </li>
    {/foreach}
</ul>
```

Available properties are

| Property | Meaning |
| --- | --- |
| `$link->isCurrent` | Does this page is current or not |
| `$link->isLink` | Does this page has link or not (href attribute) |
| `$link->label` | Link label ('1', '2', 'Next', '...', etc) |
| `$link->link` | Link to a page |
| `$link->isDot` | Does this links separator |
| `$link->html` | Link HTML built by WordPress |

### Pagination on front page

If you will use same approach to get pagination on front page, you will get broken pagination links. It because on static front page WordPress uses `page` query var, not `paged`. To fix it just pass query var name as argument for `get()` method like

```php
Post::get('page');
```

This way Post Collection pagination will be built using `page` query var not `paged`.
