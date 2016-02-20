# PHP4 Akismet Class Documentation #
## Introduction ##

If you are new to using classes in PHP you might want to read the [Wikipedia object oriented progamming](http://en.wikipedia.org/wiki/Object-oriented_programming) article.

The [Akismet API](http://akismet.com/development/api/) documentation describes the low level interface. Its intended audience is authors of code such as the Akismet PHP4 class. As such, it contains some information you don't need to know or be concerned with, but it may help you understand what the PHP4 class is doing.

The Akismet PHP4 class takes care of the connection and communications with the Akismet server and gives you a simpler high level interface.
## Using the Class ##
**Example 1**, submitting a comment to be checked:

```
include 'akismet.class.php'; 

// Load array with comment data.
$comment = array(
		'author' => 'viagra-test-123',
		'email' => 'test@example.com',
		'website' => 'http://www.example.com/',
		'body' => 'This is a test comment',
		'permalink' => 'http://your-domain.com/path-to-your-comment-page',
		'user_ip' => 'an-ip-address', // Optional, if not in array defaults to $_SERVER['REMOTE_ADDR'].
		'user_agent' => 'user-agent-string', // Optional, if not in array defaults to $_SERVER['HTTP_USER_AGENT'].
	);
	
// Instantiate an instance of the class.
$akismet = new Akismet('http://www.yourdomain.com/', 'YOUR_WORDPRESS_API_KEY', $comment);

// Test for errors.
if($akismet->errorsExist()) { // Returns true if any errors exist.
	if($akismet->isError('AKISMET_INVALID_KEY')) {
		// Do something.
	} elseif($akismet->isError('AKISMET_RESPONSE_FAILED')) {
		// Do something.
	} elseif($akismet->isError('AKISMET_SERVER_NOT_FOUND')) {
		// Do something.
	}
} else {
	// No errors, check for spam.
	if ($akismet->isSpam()) { // Returns true if Akismet thinks the comment is spam.
		// Do something with the spam comment.
	} else {
		// Do something with the non-spam comment.
	}
}
```

**Example 2**, submitting a mis-diagnosed comment:

You should incorporate the submitSpam() and submitHam() calls in your applications. Doing so helps Akisment improve its spam checking algorithms.

```
// Load array with comment data.
$comment = array(
		'author' => 'viagra-test-123',
		'email' => 'test@example.com',
		'website' => 'http://www.example.com/',
		'body' => 'This is a test comment',
		'permalink' => 'http://your-domain.com/path-to-your-comment-page',
		'user_ip' => 'an-ip-address', // Optional, if not in array defaults to $_SERVER['REMOTE_ADDR'].
		'user_agent' => 'user-agent-string', // Optional, if not in array defaults to $_SERVER['HTTP_USER_AGENT'].
	); 
	
// Instantiate an instance of the class.
$akismet = new Akismet('http://www.yourdomain.com/', 'YOUR_WORDPRESS_API_KEY', $comment);

// submit the ham or spam comment
if (!$akismet->errorsExist()) {
	$akismet->submitHam(); // Or submitSpam().
}
```

## Additional methods ##

These methods, in additon to those shown above, are also available.

```
$akismet->getError($name); // Return a specific error message from the errors array.
$akismet->getErrors(); // Return an array of all errors.
```