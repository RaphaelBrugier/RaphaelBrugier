HUGO sources


# git subtree command:

Add the master branch as the public folder:

    git subtree add --prefix=public git@github.com:raphaelbrugier/raphaelbrugier.github.io.git master --squash


Push the public folder to the master branch:

    git subtree push --prefix=public git@github.com:raphaelbrugier/raphaelbrugier.github.io.git master

See [this post](http://codethejason.github.io/blog/setupghpages/).


# Editing

## Create a new post:

    hugo new blog/2016-03-30-testing-strategy-for-apache-spark-jobs-2-of-2.md


## Run the server in draft mode:

    hugo server --buildDrafts -w


Signed commit
