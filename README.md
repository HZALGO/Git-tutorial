Project History & Git Practice Steps

This repository is my personal Git handbook.
Below is a summary of the steps I followed in this project and the Git commands I practiced along the way.

# Step 1 – Create the repository

Create the project folder:

    mkdir git-handbook
    cd git-handbook


Initialize the repository:

    git init


Create a README.md file and edit it with vi:

    vi README.md


Write some content, then save and exit (Esc, then :wq).

Check status, stage, and commit:

    git status
    git add README.md
    git commit -m "Initial commit with README"


Before the first commit, configure your Git identity (global):

    git config --global user.name "Your Name"
    git config --global user.email "you@example.com"


View commit history:

    git log


For each chapter of my Git learning, I create a new file and follow the cycle:

    create file
    git add <file>
    git diff
    git commit -m "Add <chapter> notes"
    git log


If a file was staged by mistake:

    git reset <filename>
    # or
    git restore --staged <filename>


To remove a file from Git and from the working directory:

    git rm temp-notes.txt
    git commit -m "Remove temp-notes.txt"

# Step 2 – Working with branches

Create a branch to improve the init notes:

    git branch improve-init-notes
    git checkout improve-init-notes


Edit 1. init and commit (or similar) and then:

    git status
    git add <file>
    git diff --cached
    git commit -m "Improve init and commit notes"
    git log


Merge this branch back into master:

    git checkout master
    git merge feature/improve-init-notes


Delete the branch after a successful merge:

    git branch -d feature/improve-init-notes


The same scenario can be repeated for other feature branches.

# Step 3 – Remote and GitHub

Create an empty repository on GitHub (e.g. Git-tutorial).

Copy its URL from GitHub.

Connect the local repo to the remote and push:

    git remote add origin <github-url>
    git push -u origin master

Conflict scenario

On the local machine, in README.md, add a line like:

Version from local


but do not push yet.

On GitHub, open the same README.md file in the browser and add another line, for example:

Version from GitHub


Then commit the change on GitHub.

Back on the local machine, pull from the remote:

    git pull origin master


You should see a merge conflict warning.

Fix the conflict manually in README.md, then:

    git add README.md
    git commit -m "Resolve README conflict between local and GitHub"

# Step 4 – Versioning with tags

Use git log to find the commit ID where a specific chapter was added, then create an annotated tag on that commit:

    git log --oneline
    git tag -a v1.0 b846bd2a -m "First complete version of git handbook"


Show a tag:

    git show v1.0


Later, after adding more content, create another tag (on the current HEAD):

    git tag -a v1.1 -m "Added remote & conflict chapter"


Push all tags to the remote:

    git push origin --tags


Check out a specific version by tag:

    git checkout v1.0


Note: You cannot commit directly on a tag.
If you want to continue from that version, create a new branch from the tag.

# Step 5 – GPG signing

Generate and list a GPG key:

    gpg --gen-key
    gpg --list-secret-keys --keyid-format LONG


Configure Git to use your signing key:

    git config --global user.signingkey <YOUR_KEY_ID>


Create a signed tag:

    git tag -s v2.0 -m "Signed release of git handbook"


Verify the signed tag:

    git tag -v v2.0


Create a signed commit:

    git commit -S -m "Add security & signing chapter"

# Step 6 – Debugging history (blame & bisect)

When I want to see who wrote a specific line and when:

Blame a single line (example: line 4):

    git blame '5. signing.md' -L 4


Blame the entire file:

    git blame '5. signing.md'


To find which commit introduced a bug, use git bisect:

    git bisect start
    git bisect bad          # the current commit is known to be bad
    git bisect good v1.0    # a version that is known to be good


Git will now check out commits in the middle of the range.
Each time:

If the content is correct:

    git bisect good
    

If the content is still wrong:

    git bisect bad


When Git finishes, it will tell you which commit introduced the bug.
Then reset bisect:

    git bisect reset