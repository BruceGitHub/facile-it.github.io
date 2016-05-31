# Facile.it Engineering Blog
This is the repo behind [http://engineering.facile.it/](http://engineering.facile.it/)

## Cloning instructions
```
git clone git@github.com:facile-it/facile-it.github.io.git
```
## Usage
 * No need to install anything, Hugo binaries are committed in the repo (for Linux and OSX, both x64)
 * To serve the site locally (with drafts enabled):
```
./hugo server --buildDrafts
```

 * To deploy the blog: 
```
./deploy
```

## Front matter documentation

`authors`: an array of the authors of the article; the names must match the .yml files in the `/data/authors` folder

`comments`: enables Disqus at the end of the page (disabled anyway when serving locally)

`date`: the supposed date of publishing

`draft`: to enable a draft status, which will block the article from being published during a deploy

`share`: if true, enables the social sharing buttons

`categories`: the categories where the article will be listed under; watch out for caps matching, and add always the language category

`title`: the title of the article

`languageCode`: suggested, not needed, default is "en-En"

`type`: always "post" for articles

`aliases`: an array of URLS which will redirect to this page; used for the transition from the old site

`twitterImage`: used for the twitter cards when sharing the url of this page; needs to be at least 120x120, will be cropped to square. If not specified, the ENGR logo will be used

`twitterLargeImage`: used for the twitter cards too; needs to be at least 280x150px.
