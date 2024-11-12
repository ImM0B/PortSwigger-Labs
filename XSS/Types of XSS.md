- [Reflected XSS](https://portswigger.net/web-security/cross-site-scripting#reflected-cross-site-scripting), where the malicious script comes from the current HTTP request.
- [Stored XSS](https://portswigger.net/web-security/cross-site-scripting#stored-cross-site-scripting), where the malicious script comes from the website's database.
- [DOM-based XSS](https://portswigger.net/web-security/cross-site-scripting#dom-based-cross-site-scripting), where the vulnerability exists in client-side code rather than server-side code.
- ***Reflected DOM XSS*** , when the server-side application processes data from a request and echoes the data in the response. A script on the page then processes the reflected data in an unsafe way, ultimately writing it to a dangerous sink.
- ***Stored DOM XSS*** happens when the server-side application processes data from a request and stores it permanently (for example, in a database). A script on the web page then retrieves that stored data and processes it in an insecure way, writing it to a dangerous "sink" in the DOM. As a result, the malicious code will be executed every time a user loads the page and the DOM is rendered.