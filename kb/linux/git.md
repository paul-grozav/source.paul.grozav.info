---
layout: page
ptitle: Git
---

[https://git-scm.com](https://git-scm.com)
### 1. General
#### 1.1. clone
The following command clones a remote project, it creates a local copy of that project. You should know that by default it uses the master branch.
{% highlight sh %}
git clone https://github.com/myUser/myProject.git
{% endhighlight %}

If you need to clone another branch(not master) you can use the -b argument:
{% highlight sh %}
git clone -b branchName https://github.com/myUser/myProject.git
{% endhighlight %}


#### 1.2. status
You can check the changes since the last commit by running:
{% highlight sh %}
git status
{% endhighlight %}
This will show you which files were changed and which files are new (not added to the git project).

You can make changes to your files and then create commit points:
{% highlight sh %}
# Step 1. : change your files
echo "Changing a file content" >> a.txt
echo "// TODO! feed the cat" >> b.cpp
{% endhighlight %}


#### 1.3. add
Prepare the commit. Commit points are versions of your project, states. You make some changes from one commit to the next one and you can provide a short description that tells you what was added/removed since the last version(commit).
{% highlight sh %}
# Step 2. : Prepare commit by adding changes to it
git add a.txt
git add b.cpp

# Step 2.1. (optional) : Check the things you are about to commit
git status

# Step 2.2. (optional) : You can also remove stuff from your next commit by doing
git rm someFile.txt
{% endhighlight %}


#### 1.4. commit
To make a new commit you can do the following:
{% highlight sh %}
# Step 3 : Create the commit and provide a short description
git commit -m "Added a todo to b.cpp and added something new to a.txt – This is my super new version of my super awesome project"
{% endhighlight %}


#### 1.5. push
You can push your brand-new commit (from current branch - assuming to be master) to the remote server (the origin – you cloned the project from here):
{% highlight sh %}
git push origin master
{% endhighlight %}


### 2. Solutions to specific problems:
{% highlight sh %}
# You can remove all changes since your last commit by doing a:
git reset HEAD --hard

# You can find the commit that added or removed a given line by doing a:
git log -S "whatever_string" --source --all

# Retrieve a single file from git
git archive --remote=git@gitlab.com:tancredi-paul-grozav/cpp_tests.git HEAD remove_non_digit_chars/Timer.h | tail -n +3

# Create a new branch, disconnected:
git checkout --orphan branch_name

# Commit with certain author and committer:
GIT_COMMITTER_NAME="Committer Name" GIT_COMMITTER_EMAIL="committer@e.mail" git commit --author="Author Name <author@e.mail>" -m "some message"
# or
GIT_COMMITTER_NAME="Tancredi-Paul Grozav" GIT_COMMITTER_EMAIL="paul@grozav.info" bash -c 'git commit --author="${GIT_COMMITTER_NAME} <${GIT_COMMITTER_EMAIL}>" -m "some message"'

# Create patch from git changes:
git diff > ../my.patch
# Apply patch:
git apply ../my.patch

# Diff with meld
git difftool --tool meld --no-prompt config.sql
{% endhighlight %}


### 3. Branches
{% highlight sh %}
# You can see the branches you have on your machine by running:
git branch
# This command also tells you which is the currently used branch, by displaying a ‘*’ in front of the current branch name.

# You can create a new branch by running:
git branch newBranchName

# You can move to a branch by running:
git checkout branchName

# You can delete a branch by running:
git branch -d branchName

# You can delete a remote branch by running:
git push origin --delete branchName

# You can undo the creation of a commit while keeping the commit changes, by running:
git reset HEAD^ # To undo last(1) commit
git reset HEAD~2 # To undo last two commits ...

# To also remove the changes from those commits you can add –hard flag like this:
git reset --hard HEAD^ # To undo last(1) commit and delete changes
git reset --hard HEAD~2 # To undo last two commits and delete changes

# You can merge last n commit(s), by running:
git rebase --interactive HEAD~2 # Merge last two commits

# You can update the local list of remote branches by running:
git fetch origin -p

# You can see a graph of the branches in CLI mode by running:
git log --graph --all --oneline
{% endhighlight %}


### 4. Stash
The git stash is a LIFO(Last In First Out) stack.
{% highlight sh %}
git stash list # List items from stash
git stash # Add current unstaged changes to the stash
git stash apply # Apply the last item from the stash (apply changes) – will not delete item from stash
git stash drop # Drop last item from stash
git show stash@{0} # Show changes from stash item nr 0, 1, 2, …
{% endhighlight %}


#### 5. Errors
Do you get this error when cloning with git over HTTPS?
{% highlight sh %}
user@host1:/home/paul # git clone https://gitlab.com/paul/my_project.git
Cloning into ‘my_project’…
fatal: unable to access ‘https://gitlab.com/paul/my_project.git/’: TCP connection reset by peer
{% endhighlight %}

TCP connection reset by peer doesn’t say much. Let’s see some details:
{% highlight sh %}
user@host1:/home/paul # GIT_CURL_VERBOSE=1 git clone https://gitlab.com/paul/my_project.git
Cloning into ‘my_project’…
* Couldn’t find host gitlab.com in the .netrc file; using defaults
* About to connect() to gitlab.com port 443 (#0)
* Trying 52.167.219.168…
* Connected to gitlab.com (52.167.219.168) port 443 (#0)
* Initializing NSS with certpath: sql:/etc/pki/nssdb
* CAfile: /etc/pki/tls/certs/ca-bundle.crt
CApath: none
* NSS error -5961 (PR_CONNECT_RESET_ERROR)
* TCP connection reset by peer
* Closing connection 0
fatal: unable to access ‘https://gitlab.com/paul/my_project.git/’: TCP connection reset by peer
{% endhighlight %}

Ah, that’s better: NSS error -5961 (PR_CONNECT_RESET_ERROR). According to https://bugzilla.redhat.com/show_bug.cgi?id=1217477 you should
{% highlight sh %}
yum update curl nss # Upgrade to at least curl-7.29.0-24.el7 and nss-3.19.1-14.el7
{% endhighlight %}
