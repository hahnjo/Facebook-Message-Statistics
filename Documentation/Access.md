Access to the App
=================

There are 3 ways to access this App:

* http://hahnjo.github.io/Facebook-Message-Statistics/
This is the easiest ways as this page is served by Github. I only had to authorize this URL for the app.

* http://apps.facebook.com/449609528480957/ (without SSL)
In order to make an app available directly from Facebook, you have to specify a Canvas URL. This URL must end with a slash, so you are not allowed to specify a filename. No problem till here because I was using Github Pages (see above).
But now comes the trick: Facebook is sending some form data within its request that the Github server doesn't like. It crashes and answers with a "405 Not Allowed".
I worked around this issue by creating a PHP script on my webspace that simply redirects to the page on Github which did the trick. (for the source see below)

* https://apps.facebook.com/449609528480957/ (with SSL)
For apps on Facebook you also have to specify a Secure Canvas URL that must end with a slash, too. This is also necessary for most of the users as they have enabled "Secure Browsing" in their settings (so have I).
Getting this to work was some more work because Github Pages aren't served with SSL. The redirect trick would not work either as the compete page has to be delivered with SSL.
So I requested a SSL Proxy from 1blu where I host my webspace. Then I extended the PHP script so that it downloads the contents from the Github Pages and serves them within the SSL-Proxy. I also added a .htaccess file so the scripts and images are downloaded correctly.

Sources
-------

http://fb.hahnjo.de/messagestatistics/ or https://ssl-154395.1blu.de/messagestatistics/

* .htaccess

```
RewriteEngine on
RewriteCond %{REQUEST_URI} !messagestatistics/$
RewriteRule ^messagestatistics/(.*)$ messagestatistics/index.php?file=$1 [qsappend,L]
```

* messagestatistics/index.php

```PHP
<?php

    if (isset($_SERVER["HTTP_X_FORWARDED_SERVER"]) && !empty($_SERVER["HTTP_X_FORWARDED_SERVER"]) && strpos($_SERVER["HTTP_X_FORWARDED_SERVER"], "ssl") !== false) {
		
		// Access via HTTPS
		if (!isset($_GET["file"]) || empty($_GET["file"]) || strpos($_GET["file"], "index") !== false) {
			$requestedFile = "";
		} else {
			$requestedFile = $_GET["file"];
			if (substr($requestedFile, -2) === "js") {
				header("Content-Type: text/javascript");
			} else if (substr($requestedFile, -3) === "gif") {
				header("Content-Type: image/gif");
			}
		}
		echo file_get_contents("http://hahnjo.github.io/Facebook-Message-Statistics/".$requestedFile);
		
	} else {
		// Simply redirect when accessed via HTTP
		header("Location: http://hahnjo.github.io/Facebook-Message-Statistics/");
	}
	
?>
```