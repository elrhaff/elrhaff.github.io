---
layout: post
title: Remove files from your remote git repository
comments: true
---

Oops! I commited files I don't want to be tracked to the remote again.

My most frequent oversight when dealing with git is creating a commit that includes files that I do not wish to be tracked, because I forgot to update the `.gitignore` file.

If you have already pushed your commit, then creating a new commit with an updated `.gitignore` won't delete these files from the remote (it will just keep them from being updated).

If you're as clumsy as I am, then here's what to do:

* Configure your `.gitignore` correctly by adding the files or directories you wish to ignore.
* Run the following commands at the base of your local repository:
```bash
git rm -r --cached .
git add .
git commit -m "removed unnecessary files from git"
git push origin
```

The files shouldn't appear in the remote anymore.

<div class="message">
    <strong>⚠️ Important note:</strong>

    <p>This will remove the files from the current state of your remote, but they will still be visible in your git's history. This might be an issue if you committed a large file or something sensitive like credentials. In that case, you probably want to purge it from your repository's history. <a href="https://help.github.com/en/github/authenticating-to-github/removing-sensitive-data-from-a-repository">Here</a>'s a resource on how to do it. 
    </p>
</div>
