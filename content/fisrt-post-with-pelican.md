Title: Fisrt Post with Pelican
Date: 2019-10-12 10:00
Modified: 2019-10-15 20:00
Category: blog
Tags: python, pelican, git, github
Slug: first-post-with-pelican
Author: Jack

From JekyII, Hugo and then Pelican, I am always looking for different ways of doing things. JekyII and Hugo are great, got more then what I need to make a blog. They have a great number of market share, supporting communities. If you are looking for a static site generator, they are one of the kind. Now, I am moving forward. Since leaning Python now, I want to do as many as possible with Python. After googling, Pelican goes into my house. Let me show you waht I have done.

Python and git is required. I have python3 here.


## Setup Pelican

- create a project forder

```
mkdir myblog
cd myblog
```

- create python virutal environment 

```
python3 -m venv venv
```

- activate virtual environment

```
source ./venv/bin/activate
```

- upgrade pip (optional)

```
pip install pip --upgrade
```

- install Pelican and markdown

```
pip install pelican markdown
```

- run Pelican quickstart

```
pelican-quickstart
```

- answer questions to setup you site
- start development server for testing

```
make devserver
```

- open [localhost:8000](localhost:8000) in your browser, to check your new site.

## Write Your 1st Post

- create a text file with a desired name, "your-first-post.md" inside "content" folder

```
cd content
nano your-fisrt-post.md
```

- write your post with required parameters

```
Title: Your First Post
Date: 2019-10-12 10:00
Category: Your Category
Author: Your Name

Your Post content
```

- refresh your browser (F5), then your first should be there

## Publish to github

- install package ghp-import

```
pip install ghp-import
```

- create a github repository named [your_username].github.io

- setup local repository, and add a remote
```
git init
git remote add origin https://github.com/[your_username]/[your_username].github.io.git
```

- build for publishing
```
make publish
```

- publish to github. (to master branch)
```
make github
```

And then following prompt for username and password.

Your github page will be available shortly.  You may push your source files to other branch in the same repository.

The End.



