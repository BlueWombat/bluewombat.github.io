---
layout: post
title: "Change commit author in Git"
description: ""
category: git
tags: git, bash, tips
---
{% include JB/setup %}

If you've ever had a reason to change the author name and/or email of one or more commits in a Git repository then fear not, it can be done. It, quite obviously, involves rewriting the history of your repository, and involves running a BASH script that utilizes the git-filter-branch utility included in the Git installer.

<!--more-->

## The script(s)

Both of these scripts should be executed in a BASH enabled terminal prompt, with git installed and in the root folder of the repository to change.

### Updating specific commits

Use this script if you have multiple committers in your repository, and you only want to change the author of specific commits

{% highlight bash linenos %}
{% raw %}
#!/bin/sh

git filter-branch --env-filter '

OLD_EMAIL="your.old@email.tld"
CORRECT_NAME="Your New Name"
CORRECT_EMAIL="your.new@email.tld"

if [ "$GIT_COMMITTER_EMAIL" = "$OLD_EMAIL" ]
then
    export GIT_COMMITTER_NAME="$CORRECT_NAME"
    export GIT_COMMITTER_EMAIL="$CORRECT_EMAIL"
    export GIT_AUTHOR_NAME="$CORRECT_NAME"
    export GIT_AUTHOR_EMAIL="$CORRECT_EMAIL"
fi
if [ "$GIT_AUTHOR_EMAIL" = "$OLD_EMAIL" ]
then
    export GIT_COMMITTER_NAME="$CORRECT_NAME"
    export GIT_COMMITTER_EMAIL="$CORRECT_EMAIL"
    export GIT_AUTHOR_NAME="$CORRECT_NAME"
    export GIT_AUTHOR_EMAIL="$CORRECT_EMAIL"
fi
' --tag-name-filter cat -- --branches --tags
{% endraw %}
{% endhighlight %}

### Updating all commits

Use this script if you only have a single committers in your repository, or if you for some reason want to change the author of all commits regardless of who committed originally.

{% highlight bash linenos %}
{% raw %}
#!/bin/sh

git filter-branch --env-filter '

OLD_EMAIL="your.old@email.tld"
CORRECT_NAME="Your New Namr"
CORRECT_EMAIL="your.new@email.tld"

export GIT_COMMITTER_NAME="$CORRECT_NAME"
export GIT_COMMITTER_EMAIL="$CORRECT_EMAIL"
export GIT_AUTHOR_NAME="$CORRECT_NAME"
export GIT_AUTHOR_EMAIL="$CORRECT_EMAIL"
' --tag-name-filter cat -- --branches --tags
{% endraw %}
{% endhighlight %}

After running one of the above scripts, and inspecting the history of the repository to make sure it's rewritten correctly, run the below command.

{% highlight bash linenos %}
git push --force --tags origin 'refs/heads/*'
{% endhighlight %}

And finally delete the temporary clone, to make sure nothing gets messed up down the road.

And that's it, you're done. I hope it all makes sense, and you find it useful. Feel free to comment if I can improve the post, or you're unsure about something.