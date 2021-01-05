# Operation 속성

`<condition>` 요소의 `operation` 속성은 조건의 연산자를 지정합니다. 아래와 같은 연산자를 사용할 수 있습니다.

| 연산자          | 의미                                                                   | 지원 버전 |
|----------------|-----------------------------------------------------------------------|----------|
| equal          | column = (var or default)                                             | > 2.0.0  |
| more           | column >= (var or default)                                            | > 2.0.0  |
| gte            | same as `more`                                                        | > 2.0.0  |
| excess         | column > (var or default)                                             | > 2.0.0  |
| gt             | same as `excess`                                                      | > 2.0.0  |
| less           | column <= (var or default)                                            | > 2.0.0  |
| lte            | same as `less`                                                        | > 2.0.0  |
| below          | column < (var or default)                                             | > 2.0.0  |
| lt             | same as `below`                                                       | > 2.0.0  |
| regexp         | column REGEXP (var or default)                                        | > 2.0.0  |
| notregexp      | column NOT REGEXP (var or default)                                    | > 2.0.0  |
| not_regexp     | same as `notregexp`                                                   | > 2.0.0  |
| notequal       | column != (var or default)                                            | > 2.0.0  |
| not_equal      | same as notequal                                                      | > 2.0.0  |
| notnull        | column is not null                                                    | > 2.0.0  |
| null           | column is null                                                        | > 2.0.0  |
| like_prefix    | column like 'var or default%'                                         | > 2.0.0  |
| like_head      | same as `like_prefix`                                                 | > 2.0.0  |
| like_suffix    | column like '%var or default'                                         | > 2.0.0  |
| like_tail      | same as `like_suffix`                                                 | > 2.0.0  |
| like           | column like '%var or default%'                                        | > 2.0.0  |
| notlike        | column NOT LIKE '%var or default%'                                    | > 2.0.0  |
| notlike_prefix | column NOT LIKE 'var or default%'                                     | > 2.0.0  |
| notlike_head   | same as `notlike_prefix`                                              | > 2.0.0  |
| notlike_suffix | column NOT LIKE '%var or default'                                     | > 2.0.0  |
| notlike_tail   | same as `notlike_suffix`                                              | > 2.0.0  |
| search         | 사용된 단어들을 자동으로 분리하여 그 갯수만큼 LIKE % 조건을 만들어 줍니다.    | > 2.0.0  |
| in             | column in (var or default)                                            | > 2.0.0  |
| notin          | column not in (var or default)                                        | > 2.0.0  |
| null           | column IS NULL                                                        | > 2.0.0  |
| notnull        | column IS NOT NULL                                                    | > 2.0.0  |

## References

[Pull request #1322](https://github.com/rhymix/rhymix/pull/1332)