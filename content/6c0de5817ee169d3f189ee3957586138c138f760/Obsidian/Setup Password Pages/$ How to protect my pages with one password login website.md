**Refer:** [Link](https://stackoverflow.com/questions/27065192/how-do-i-protect-a-directory-within-github-pages)

GitHubPages (like Bitbucket Pages and GitLab Pages) only serve static pages, so the only solution is something client side (Javascript).

A solution could be, instead of using real authentication, just to share only a secret (password) with all the authorized persons and implement one of the following scheme:

1. put all the private files in a (not listed) subdirectory and name that with the hash of the chosen password. The index page asks you (with Javascript) for the password and build the correct start link calculating the hash.
    
    See for example: [https://github.com/matteobrusa/Password-protection-for-static-pages](https://github.com/matteobrusa/Password-protection-for-static-pages)
    
    PRO: Very simple approach _protecting_ a whole subdirectory tree
    
    CONS:
    
    - possible attack: sniffing the following requests to obtain the name of the subdirectory
    - the admins on the hosting site have access to the full contents

![[Pasted image 20241018145107.png]]