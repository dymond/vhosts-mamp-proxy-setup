# Creating Custom URLs for local development with MAMP vhosts

1. Set Web & MySQL ports to 80 & 3306 in MAMP preferences
2. Open `/Applications/MAMP/conf/apache/httpd.conf` and search for `-vhosts.conf`. If you have the default `httpd.conf`, only one match will show up on line 585 and you'll need to uncomment that line by deleting the `#` at the start of it.
	* It will go from this: `# Include /Applications/MAMP/conf/apache/extra/httpd-vhosts.conf`
	* to this: `Include /Applications/MAMP/conf/apache/extra/httpd-vhosts.conf`
3. Copy the `httpd-vhosts.conf` file from this repo into `/Applications/MAMP/conf/apache/extra`
4. Edit `httpd-vhosts.conf` to reflect the correct location of your site directory on your local drive and the new site URL you want
	* Lines 34, 35 and 36 should each be edited
	* If you want to work on multiple sites, duplicate the entire section below and change those three parameters for each site.
```
<VirtualHost *:80>
    DocumentRoot "/Users/*NAME*/dev/site"     <-- Put your local directory here
    ServerName site.url                       <-- Put your desired url here
    <Directory "/Users/*NAME*/dev/site">      <-- Put your local directory here
        Options Indexes FollowSymLinks
        AllowOverride All
    </Directory>
</VirtualHost>
```
5. Edit hosts file and add this line:
	* `127.0.0.1      localhost yoursite.url`
	* If you are working on multiple sites, create an entry for each site url.
	* ([Hostbuddy](https://clickontyler.com/hostbuddy/) allows quick, easy swapping of hosts files)
6. Restart the MAMP servers

## Additional step for browser-sync users
You can get browser-sync to work by add `proxy: "yoursite.url"` to your browser-sync options. Depending on your task runner environment, this can be different. These examples are for gulp and grunt. I don't use brunch, but it should be fairly simple to figure out.

### Gulp users
Add this to your gulpfile:
```var browserSyncOptions = {
	proxy: "yoursite.url"
};```

__Be careful!__ I would recommend searching for `browserSyncOptions` in your gulpfile if you didn't hand build it. If it's already set, just add `proxy: "yoursite.url",` in with the other options.

### Grunt users
You should reference [the docs](https://browsersync.io/docs/grunt). As long as your setup isn't really funky, it should be pretty straightforward to just add the proxy line to your task so that it looks something like this:
```browserSync: {
	dev: {
		bsFiles: {
			src : 'assets/css/style.css'
		},
		options: {
			proxy: "local.dev"
		}
	}
}```