Title: Add a Theme to Pelican
Date: 2019-10-17 20:20
Category: Python
Tags: pelican, theme
Slug: add-a-theme-to-pelican
Authors: Jack Wu
Summary: Add a theme to Pelican

After writing a post and publishing to github, it's time to make my pages looks different. [Pelican Theme Repository](https://github.com/getpelican/pelican-themes) provides many themes. Many thanks to those devevlopers and maintainers. I found my favior theme, [pelican-clean-blog](https://github.com/gilsondev/pelican-clean-blog). It really looks clean and nice. Let me show you how I applied it to my blog.

## Download

```
cd ~/projects
git clone https://github.com/gilsondev/pelican-clean-blog.git
```

## Setup

- Add following to pelicanconf.py, remeber to replace username with yours

```
THEME = '/home/[username]/projects/pelican-clean-blog'
```

That's Not it! There's a catch. A build error came up when I build for publishing. After googling, I found one [here](https://github.com/getpelican/pelican/issues/2489#issuecomment-472176621) Next step is how to fix it.

## Fix Error

- Go to folder /home/[username]/projects/pelican-clean-blog and find base.html
- Find '|format(' and replace it with '.format(slug=' without quotes 
- Rebuild again.

The End.