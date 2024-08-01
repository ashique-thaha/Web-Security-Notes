Search an organisation’s `GitHub repositories` for sensitive data that has been accidentally committed, 

or information that could lead to the discovery of a vulnerability

Start by finding the GitHub usernames relevant to our target. 
we should be able to locate these by searching the organisation’s name or product names via GitHub’s search bar, or by checking the GitHub accounts of known employees.

When we find `usernames to audit`, visit their pages. Find repos related to the projects we’re testing and record them, along with the usernames of the organisation’s top contributors, which can help you find more relevant repositories

Then `dive into the code`. For each repository, pay special attention to the Issues and Commits sections. These sections are full of potential info leaks: they could point attackers to `unresolved bugs, problematic code`, and the most recent code fixes and security patches. 

Recent code changes that haven’t stood the test of time are more likely to contain bugs. Look at any protection mechanisms implemented to see if you can bypass them. 

You can also search the Code section for potentially vulnerable code snippets. 

Once you’ve found a file of interest, check the Blame and History sections at the top-right corner of the file’s page to see how it was developed.


Look for hardcoded secrets such as `API keys, encryption keys,and database passwords`. 

Search the organisation’s repositories for terms like `key, secret, and password` to locate hardcoded user credentials that you can use to access internal systems. 

After we’ve found leaked credentials, you can use `KeyHacks (https://github.com/streaak/keyhacks/)` to check if the credentials are valid and learn how to use them to access the target’s services.


we should also search for sensitive functionalities in the project. See if any of the source code deals with important functions such as authentication, password reset, state-changing actions, or private info reads. 

Pay attention to code that deals with user input, such as `HTTP request parameters, HTTP headers, HTTP request paths, database entries, file reads, and file uploads`, because they provide potential entry points for attackers to exploit the application’s vulnerabilities.

Look for any configuration files, as they allow you to gather more information about your infrastructure. Also, `search for old endpoints and S3 bucket URLs that we can attack`.

Record these files for further review in the future.

`Outdated dependencies` and the unchecked use of `dangerous functions` are also a huge source of bugs. 

Pay attention to dependencies and imports being used and go through the versions list to see if they’re outdated.

Record any outdated dependencies. we can use this information later to look for publicly disclosed vulnerabilities that would work on our target.


Tools like `Gitrob` and `TruffleHog` can automate the GitHub recon process. `Gitrob (https://github.com/michenriksen/gitrob/)` locates potentially sensitive files pushed to public repositories on GitHub. 

`TruffleHog (https://github.com/trufflesecurity/truffleHog/)` specialises in finding secrets in repositories by conducting regex searches and scanning for high-entropy strings.

