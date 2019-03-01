# domtext-doc
This is a draft documentation for Drupal's domtext.path service.
Service itself is under construction, so this draft is uncompleted and not stable yet.


# domtext.path service

    \Drupal::service('domtext.path');


## International URL handling


### Note about IDN URLs in Drupal

It is good idea to store URLs on server side in ascii form
(punycoded domain name, percent-encoded other non-ascii parts).
But it looks like Drupal do not follow this way (yet?..).

Opened question around this:

* What we need to get as a base format to store IDN URLs in database?

I think that punycoded domain name in database is the best solution,
because many pieces of code just do not know how to handle IDN today.
For example: Drupal's httpClient and file_get_contents() php function.

This is not a problem for domtext.path service itself, 
but this is how to use it.


### Main targets for domtext.path

* Convert URLs to humanized form for (as typical example) user inputs, 
so user can see friendly decoded version.

* Convert user input back to valid ascii form. Input string can be in ascii
or unicode form or as mixture of both.

* Check what URL is: What the scheme? What the type (absolute, relative, local)?

* Normalize, sanitize and validate URLs on scheme basis:
Is scheme allowed? Is URL well-formed for this scheme?


### IDN hostname handling
    
IDN hostname conversion based on 
[intl php extension](http://php.net/manual/en/ref.intl.idn.php)
if it installed, or via 
[idna service module](https://www.drupal.org/project/idna)
if intl not installed. 
    
If you do not have any of this options, non-ascii hostnames 
will fail parsing and validation by domtext.path.

