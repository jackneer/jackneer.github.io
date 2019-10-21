Title: Hugo GitHub Publishing
Date: 2019-03-13T14:26:05+08:00
Category: hugo
Tags: hugo, git, github
Author: Jack

There are a couple ways to publish hugo site to GitHub. I provide a simple way althouth url doesn't not look like https://[username].GitHub.io but https://[username].GitHub.io/[blog] in which [blog] is your repository name.

1. create a repository with a name that you want to be postfix of your domain name https://[username].GitHub.io, blog for example here.

2. build your static site with following command at your root dicectory of hugo site
```bash
hugo -d docs
```

3. enable git and push it to GitHub repository.
```bash
git init
git add -A
git commit -m "init commit"
git remote add origin https://github.com/[username]/[repo_name].git
git push -u origin master
```

4. go to setting of the repository of GitHub, in the GitHub Pages section set Source to [master branch /docs folder] from [None]

5. open browser at https://[username].github.io/[blog], your new site should be ready