Rhymix\Framework\Formatter
--------------------------

#### text2html()

```
public static function text2html(
    string $text,
    int $options = 0
): string
```

Convert plain text to HTML.

#### html2text()

```
public static function html2text(string $html): string
```

Convert HTML to plain text.

#### markdown2html()

```
public static function markdown2html(
    string $markdown,
    int $options = 0
): string
```

Convert Markdown to HTML.

#### html2markdown()

```
public static function html2markdown(string $html): string
```

Convert HTML to Markdown.

#### bbcode()

```
public static function bbcode(string $bbcode): string
```

Convert BBCode to HTML.

#### applySmartQuotes()

```
public static function applySmartQuotes(string $html): string
```

Apply smart quotes and other stylistic enhancements to HTML.

#### compileLESS()

```
public static function compileLESS(
    $source_filename,
    string $target_filename,
    array $variables = [],
    bool $minify = false
): bool
```

Compile LESS into CSS.

#### compileSCSS()

```
public static function compileSCSS(
    $source_filename,
    string $target_filename,
    array $variables = [],
    bool $minify = false
): bool
```

Compile SCSS into CSS.

#### minifyCSS()

```
public static function minifyCSS(
    $source_filename,
    string $target_filename
): bool
```

Minify CSS.

#### minifyJS()

```
public static function minifyJS(
    $source_filename,
    string $target_filename
): bool
```

Minify JS.

#### concatCSS()

```
public static function concatCSS(
    $source_filename,
    string $target_filename,
    bool $add_comment = true,
    array &$imported_list = []
): string
```

CSS concatenation subroutine for compileLESS() and compileSCSS().

#### concatJS()

```
public static function concatJS($source_filename): string
```

JS concatenation subroutine.

#### convertIECondition()

```
public static function convertIECondition($condition)
```

Convert IE conditional comments to JS conditions.
