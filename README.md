# Blog with Jekyll
## Define env variables
### Jekyll version
At this moment, I'm using the 3.8 version. 
```bash
export JEKYLL_VERSION=3.8
```
### Blog's home
Path when you clone the blog repository. It's used to cache the build process
```bash
export BLOG_HOME=/home/db/proyectos/pocs/jekyll-blog
```
### Httpd
As a static server, I use Apache.
```bash
export HTTPD_VERSION=2.4
```

## Build
### Gems
I am using many gems that work as Jekyll plugins. You have to update the catalog and download these plugins. 

The best way is using a volume because it is enought updating just once time. 
```bash
docker  run \
        --rm \
        --volume="$BLOG_HOME:/srv/jekyll" \
        -it \
        jekyll/jekyll:$JEKYLL_VERSION bundle update
```

### Build the blog
When all the gems are available, the following step is to build the blog. I am using Docket to build it. Remember to mount the volume when you have download the plugins.

```bash
docker  run \
        --rm \  
        --volume="$BLOG_HOME:/srv/jekyll" \
        --volume="$BLOG_HOME/vendor/bundle:/usr/local/bundle" \
        -it \
        jekyll/jekyll:JEKYLL_VERSION jekyll build
```

## Serve
To serve the blog, you can use Jekyll "serve" command. As the blog is static content, I prefer just to use the httpd server.

You just have to mount the "_site" folder, generated previously with your blog content.

```bash
docker  run -dit \
        --name blog \
        -p 8080:80 \
        -v $BLOG_HOME/_site:/usr/local/apache2/htdocs/ \
        httpd:$HTTPD_VERSION
```