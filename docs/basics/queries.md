# Query Builder

To get objects from a database, you need to create Query object. WordPress doing this with a help of `WP_Query` object. Kawa uses same approach - we do not interacting with database directly - instead we are passing query arguments to a `WP_Query` object using helper Facades objects including Models

```php
use Kawa\Support\Facades\Query;

$posts = Query::query(['post_type' => 'post', 'posts_per_page' => 15])->get();
```

But there is no need to pass `post_type` property when you're using models - same methods available using same syntax

```php
$posts = Post::query(['posts_per_page' => 15])->get();
```

## Queries

All available methods for `Builder` object are:

### Getting all available published posts

```php
Post::all();
```

Returns: `\Kawa\Queries\PostCollection`

### Getting paginated list of posts

```php
Post::get();
```

Will paginate according to global pagination settings or Kernel settings. Read [more](/front-end/pagination.md) about pagination.

Parameters:

| Parameter | Meaning |
| --- | --- |
| `string $var = 'paged'` | Pagination query var. On static front should be set as `page` |

Returns: `\Kawa\Queries\PostCollection`

### Getting some specific amount of posts

Works same way as get, but does not care about building of pagination object

```php
Post::take(4);
```

Parameters:

| Parameter | Meaning |
| --- | --- |
| `int $perPage | Amount of posts to get |

Returns: `\Kawa\Queries\PostCollection`

### Querying anything you wish

Builds complex queries

```php
Post::query(/** same array as for WP_Query */);
```

Parameters:

| Parameter | Meaning |
| --- | --- |
| `array $query | Query arguments |

> Note: this method returns Builder object - this mean it can be chained. In order to retrieve posts collection finish ot with `get`, `all` or `take` methods

Returns: `\Kawa\Queries\Builder`
