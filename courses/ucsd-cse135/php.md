# PHP

## Utility functions

### date and time

* strtotime\('2012-02-29'\);
  * return unix timestamp 1330502400
* format time **date** \( 'd/M/Y h:i:s' , $**date** \);

### Upper/lower cases

strtolower\(\) is one of the most popular functions of **PHP**, which is widely used to convert the string into **lowercase**. It takes a string as a parameter and converts all uppercase English character of that string to **lowercase**. Other characters such as numbers and special character of that string remain the same

## Session

In the general situation :

* the session id is sent to the user when his session is created.
* it is stored in a cookie \(called, by default, `PHPSESSID`\)
* that cookie is sent by the browser to the server with each request
* the server \(PHP\) uses that cookie, containing the session\_id, to know which file corresponds to that user.

The data in the sessions files is the content of `$_SESSION`, serialized _\(ie, represented as a string -- with a function such as_ [_serialize_](http://php.net/serialize)_\)_ ; and is un-serialized when the file is loaded by PHP, to populate the `$_SESSION` array.

  
Sometimes, the session id is not stored in a cookie, but sent in URLs, too -- but that's quite rare, nowadays.

  
For more informations, you can take a look at the [Session Handling](http://php.net/manual/en/book.session.php) section of the manual, that gives some useful informations.

For instance, there is a page about [Passing the Session ID](http://php.net/manual/en/session.idpassing.php), which explains how the session id is passed from page to page, using a cookie, or in URLs -- and which configuration options affect this

