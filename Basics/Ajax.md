

Ajax is a collection of programming techniques used on the client side to create UI that aim to mimic the smooth interaction and dynamic behaviour of traditional desktop applications.

The name originally was an acronym for “Asynchronous JavaScript and XML,” although in today’s web Ajax requests need not be asynchronous and need not employ XML.

The earliest web applications were based on complete pages. Each user action, such as clicking a link or submitting a form, initiated a window-level navigation event, causing a new page to be loaded from the server. 

This approach resulted in a disjointed user experience, with noticeable delays while large responses were received from the server and the whole page was re rendered.

With Ajax, some user actions are handled within client-side script code and do not cause a full reload of the page. 

Instead, the script performs a request “in the background” and typically receives a much smaller response that is used to dynamically update only part of the user interface. 

For example, in an Ajax-based shopping application, clicking an Add to Cart button may cause a background request that updates the server-side record of the user’s shopping cart and a lightweight response that updates the number of cart items showing on the user’s screen. 

Virtually the entire existing page remains unmodified within the browser, providing a much faster and more satisfying experience for the user.

The core technology used in Ajax is XMLHttpRequest. After a certain consolidation of standards, 
this is now a native JavaScript object that client-side scripts can use to make “background” requests without requiring a window-level navigation event. 


Note that although most Ajax applications do use asynchronous communications with the server, this is not essential. 

In some situations, it may actually make more sense to prevent user interaction with the application while a particular action is carried out. In these situations, Ajax is still beneficial in providing a more seamless experience by avoiding the need to reload an entire page.

Historically, the use of Ajax has introduced some new types of vulnerabilities into web applications. 

More broadly, it also increases the attack surface of a typical application by introducing more potential targets for attack on both the server and client side. Ajax techniques are also available for use by attackers when they are devising more effective exploits for other vulnerabilities. 