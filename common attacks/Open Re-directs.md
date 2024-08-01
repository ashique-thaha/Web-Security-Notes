

`Sites often use HTTP or URL parameters to redirect users to a specified URL without any user action.This behaviour can be useful, it can also cause open redirects, which happen when an attacker is able to manipulate the value of this parameter to redirect the user offsite.`

Websites often need to automatically redirect their users. For example, 

`this scenario commonly occurs when unauthenticated users try to access a page that requires logging in. The website will usually redirect those users to the login page, and then return them to their original location after they’re authenticated.` 


For example, when these users visit their account dashboards
at `https://example.com/dashboard`, the application might redirect them to the login page at `https://example.com/login`
To later redirect users to their previous location, 

the site needs to remember which page they intended to access before they were redirected to the login page.Therefore, the site uses some sort of redirect URL parameter appended to the URL to keep track of the user’s original location. 

This parameter determines where to redirect the user after login. For example, the URL `https://example.com/login?redirect=https://example.com/dashboard` will redirect
to the user’s dashboard, located at `https://example.com/dashboard`, 
after login.

Or if the user was originally trying to browse their account settings page, the site would redirect the user to the settings page after login, and the URL

would look like this`: https://example.com/login?redirect=https://example.com/settings`


During an open-redirect attack, an attacker tricks the user into visiting an external site by providing them with a URL from the legitimate site that redirects somewhere else, like this: 

`https://example.com/login?redirect=https://attacker.com`

 A URL like this one could trick victims into clicking the link, because they’ll believe it leads to a page on the legitimate site, example.com.

But in reality, this page automatically redirects to a malicious page. Attackers can then launch a social engineering attack and trick users into entering their example.com credentials on the attacker’s site. 


Another common open-redirect technique is referrer based open redirect. The referrer is an HTTP request header that browsers automatically include.

It tells the server where the request originated from. Referrer headers are a common way of determining the user’s original location, since they contain the URL that linked to the current page.

Thus, some sites will redirect to the page’s referrer URL automatically after certain user actions, like login or logout. In this case, attackers can host a site that links to the victim.

 to set the referrer header of the request, using HTML like the following:

`<html>`
`<a href="https://example.com/login">
`Click here to log in to example.com</a>`
`</html>`

This HTML page contains an <a> tag, which links the text in the tag to another location. 

This page contains a link with the text Click here to log in to example.com. When a user clicks the link,

they’ll be redirected to the location specified by the href attribute of the <a> tag.

which is https://example.com/login in this example.


If example.com uses a referrer-based redirect system, the user’s browser  would redirect to the attacker’s site after the user visits example.com, 
because the browser visited example.com via the attacker’s page.












