https://portswigger.net/web-security/cross-site-scripting/dom-based

**SOURCE VS SINK**
To test for DOM XSS in an HTML ==sink==, place a random alphanumeric string into the ==source== (such as location.search), then use developer tools to inspect the HTML and find where your string appears.
Note that the ==browser's "View source" option won't work for DOM XSS testing== because it doesn't take account of changes that have been performed in the HTML by JavaScript. In Chrome's developer tools, you can use Control+F (or Command+F on MacOS) to search the DOM for your string.