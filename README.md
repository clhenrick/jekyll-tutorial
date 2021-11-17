# Jekyll Tutorial

## 01 [Setup](https://jekyllrb.com/docs/step-by-step/01-setup/)

1. install [rbenv](https://github.com/rbenv/rbenv) to manage Ruby installs
2. install ruby. I installed version `3.0.2`
3. set local ruby version (e.g. to `3.0.2`) via `rbenv local 3.0.2`
4. run `gem install jekyll bundler` to create a Gemfile
5. edit the `Gemfile` and add the following gems:
    ```
    gem "jekyll"
    gem "webrick"
    ```
6. Run `bundle` to install gems
7. Create an `index.html` file with basic HTML
8. Run either `bundle exec jekyll serve` to start a local server or `bundle exec jekyll build` to build the site.

## 02 [Liquid](https://jekyllrb.com/docs/step-by-step/02-liquid/)

Liquid templates have three main components: objects, tags, and filters.

- objects represent data that can be written to a template using curly braces, e.g. `{{ object.property }}`

- tags are used for logic and control flow, and use the syntax `{% %}`; e.g. `{% if page.show_sidebar %} <!-- show the sidebar --> {% endif %}`

- filters change the output of a Liquid object

**Note:** to use Liquid in your pages you need to append "front matter" to the top of the file, e.g.

```html
---
---
<!DOCTYPE html>
```

## 03 [Front Matter](https://jekyllrb.com/docs/step-by-step/03-front-matter/)

Front matter is a snippet of YAML placed between two triple-dashed lines at the start of a file.

- You may set variables in a page and call them in the template
- Front matter is required for Jekyll to interpret / process a file

## 04 [Layouts](https://jekyllrb.com/docs/step-by-step/04-layouts/)

Layouts are templates that can be used by any page in your site and wrap around page content. They are stored in a directory called `_layouts`.

**Note:** `content` is a special variable that returns the rendered content of the page on which it’s called.

## 05 [Includes](https://jekyllrb.com/docs/step-by-step/05-includes/)

The `include` tag allows you to include content from another file stored in an _includes folder. Includes are useful for having a single source for source code that repeats around the site or for improving the readability.

Jekyll's [variables](https://jekyllrb.com/docs/variables/) can be used to alter what is in each `include`, for example the styling of an element.

## 06 [Data Files](https://jekyllrb.com/docs/step-by-step/06-data-files/)

Jekyll supports loading data from YAML, JSON, and CSV files located in a `_data` directory. Data files are a great way to separate content from source code to make the site easier to maintain.

- Data is made available through the variable `site.data.data_file_name`
- You can use it for example to dynamically create page navigation links and properties

## 07 [Assets](https://jekyllrb.com/docs/step-by-step/07-assets/)

Using CSS, JS, images and other assets is straightforward with Jekyll. Place them in your site folder and they’ll copy across to the built site.

- typically these are placed in a directory called `assets`

### Sass

Jekyll supports Sass out of the box:

1. create a directory called `_scss` for your `.scss` partials
2. create a `styles.scss` in the `assets` directory that `@import`s your `main.scss`
3. link to your primary style sheet from the `assets` directory where needed, e.g. in any `layout`

## 08 [Blogging](https://jekyllrb.com/docs/step-by-step/08-blogging/)

In true Jekyll style, blogging is powered by text files only.

Blog posts live in a folder called `_posts`. The filename for posts have a special format: the publish date, then a title, followed by an extension.

Note the `date_to_string` filter, this formats a date into a nicer format.

All blog posts data will be accessible in a variable called `site.posts`

Each `post` has the following variables:

- `post.url` is automatically set by Jekyll to the output path of the post
- `post.title` is pulled from the post filename and can be overridden by setting title in front matter
- `post.excerpt` is the first paragraph of content by default

## 09 [Collections](https://jekyllrb.com/docs/step-by-step/09-collections/)

Collections are similar to posts except the content doesn’t have to be grouped by date.

IMO it's a bit complex to set up...

## 10 [Deployment](https://jekyllrb.com/docs/step-by-step/10-deployment/)

Preparing for deployment to a production environment.

### Gemfile

It’s good practice to have a `Gemfile` for your site. This ensures the version of Jekyll and other gems remains consistent across different environments.

Bundler installs the gems and creates a `Gemfile.lock` which locks the current gem versions for a future `bundle install`. If you ever want to update your gem versions you can run `bundle update`.

When using a `Gemfile`, you’ll run commands like `jekyll serve` with `bundle exec` prefixed. So the full command is: `bundle exec jekyll serve`

This restricts your Ruby environment to only use gems set in your `Gemfile`.

### Plugins
Jekyll plugins allow you to create custom generated content specific to your site. There’s many plugins available or you can even write your own.

There are three official plugins which are useful on almost any Jekyll site:

- `jekyll-sitemap` - Creates a sitemap file to help search engines index content
- `jekyll-feed` - Creates an RSS feed for your posts
- `jekyll-seo-tag` - Adds meta tags to help with SEO

To use these first you need to add them to your Gemfile. If you put them in a `jekyll_plugins` group they’ll automatically be required into Jekyll.

Add them to your `_config.yml` as well.

### Environments

You can set the environment by using the `JEKYLL_ENV` environment variable when running a command.

By default `JEKYLL_ENV` is development. The `JEKYLL_ENV` is available to you in liquid using `jekyll.environment`.

Useful for only outputting an analytics script in production.

```liquid
{% if jekyll.environment == "production" %}
  <script src="my-analytics-script.js"></script>
{% endif %}
```

### Deployment
The final step is to get the site onto a production server. The most basic way to do this is to run a production build

```
JEKYLL_ENV=production bundle exec jekyll build
```

And then copy the contents of _site to your server.

A better way is to automate this process using a CI or 3rd party.

**Note:**  The contents of `_site` are automatically cleaned, by default, when the site is built. Files or folders that are not created by your site's build process will be removed. Some files could be retained by specifying them within the `keep_files` configuration directive. Other files could be retained by keeping them in your assets directory.