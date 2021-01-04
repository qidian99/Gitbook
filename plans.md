# Plans

01/04

1. Get familiar with the modules 
2. Help Joe test the ingestion data
3. Get some good food
4. Show Chelsea the mobile app
5. Do some work in Drupal views for data ingestion
6. Take the first lecture of Math 142B
7. Read PHP coding standard \(Done\)
   * Code MUST follow a “coding style guide” PSR \[[PSR-1](https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-1-basic-coding-standard.md)\].
   * Code MUST use 4 spaces for indenting, not tabs.
   * There MUST NOT be a hard limit on line length; the soft limit MUST be 120 characters; lines SHOULD be 80 characters or less.
   * There MUST be one blank line after the `namespace` declaration, and there MUST be one blank line after the block of `use` declarations.
   * Opening braces for classes MUST go on the next line, and closing braces MUST go on the next line after the body.
   * Opening braces for methods MUST go on the next line, and closing braces MUST go on the next line after the body.
   * Visibility MUST be declared on all properties and methods; `abstract` and `final` MUST be declared before the visibility; `static` MUST be declared after the visibility.
   * Control structure keywords MUST have one space after them; method and function calls MUST NOT.
   * Opening braces for control structures MUST go on the same line, and closing braces MUST go on the next line after the body.
   * Opening parentheses for control structures MUST NOT have a space after them, and closing parentheses for control structures MUST NOT have a space before
8. Unix timestamp and datetime conversion
   1. `select * from file_managed where filemime = 'text/csv' AND Date(FROM_UNIXTIME(timestamp)) = '2021-01-04';`
   2. Trigger a transform from unix time to date time when a insert/update happens
9. Inline file in browser: In a regular HTTP response, the **`Content-Disposition`** response header is a header indicating if the content is expected to be displayed _inline_ in the browser, that is, as a Web page or as part of a Web page, or as an _attachment_, that is downloaded and saved locally.

