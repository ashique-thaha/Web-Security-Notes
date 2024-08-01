
Like XSS and open redirects, insecure direct object references (IDORs) are a type of bug present in almost every web application.

They happen when the application grants direct access to a resource based on the user’s request, without validation.


so imagine a scenario;

here we have a  messaging application that uses a user ID parameter in the URL to differentiate its users.

it is like: user_id = 1234

from trial and error, we can observe that changing the user id will change the access to their resources.

this is IDOR.

the vulnerability does not just rely on the parameter, but the application fails to verify that the request originated from the authorised user.

## prevention

Firstly, the application can verify the user's cookie for authentication and authorisation, and thus implement access control.

secondly, the application should randomise the object reference sting, here t is user_id with a long unguessable unique string.

