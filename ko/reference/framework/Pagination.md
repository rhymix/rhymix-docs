Rhymix\Framework\Pagination
---------------------------

#### countPages()

```
public static function countPages(
    int $total_items,
    int $items_per_page,
    int $minimum = 1
): int
```

Calculate the number of pages.

#### createLinks()

```
public static function createLinks(
    string $base_url,
    int $total_pages,
    int $current_page = 1,
    int $count = 10,
    int $count_style = 1
): string
```

Create HTML for pagination.
