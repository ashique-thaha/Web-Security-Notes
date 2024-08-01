
This is  a vulnerability that makes users perform the actions that they don't intend to do. Through this vulnerability the attacker will manage to get around `same origin policy` which is a mechanism to prevent different websites from interfering each other.

Depending on the nature of action and users privilege the attacker might be able to gain control partially or fully.

#### How does it work?

For this to be possible , some conditions has to be fulfilled:


1. ==Relevant action==: This might be an action that will let the attacker have interest , for example, changing user's password.

2. ==cookie-based session handling==: To find who is accessing the resource the server solely relies on cookies.There must be a session handling with cookies

3. ==No unpredictable request parameters==: The request that perform the action cannot contain anything unpredictable. for example, when attacker trying to change the password of a user, if the action is not possible without knowing the current password then the function is not vulnerable.


