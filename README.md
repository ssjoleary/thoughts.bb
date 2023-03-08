# thoughts.bb

thoughts.bb is a program for making tweet-sized text posts and putting them on the internet.

thoughts pages are an attempt at a quieter, slower, more personal internet. A little space
on the web, just for you (and whoever you'd like to share them with).

See [Thoughts](https://github.com/dimriver/thoughts) and [thoughts.page](https://thoughts.page/) for more info about this.
I guess it's microblogging by another name?

See [API.md](API.md) on how to use this.

Compatible with [babashka](#babashka) and [Clojure](#clojure).

Includes hot-reload.

See an example [HERE](https://ssjoleary.com/thoughts.bb/example)

## Who's using thoughts.bb

- [~automaticpancake](https://tilde.club/~automaticpancake)

Feel free to PR yours.

## quickblog

thoughts.bb is a modification of [quickblog](https://github.com/borkdude/quickblog).
An excellent light-weight static blog engine for Clojure and babashka. Check it out
for your more traditional blogging needs!

## Quickstart

### Babashka

thoughts.bb is meant to be used as a library from your [Babashka](https://github.com/babashka/babashka#introduction) project.
The easiest way to use it is to add a task to your project's `bb.edn`.

This example assumes a basic `bb.edn` like this:

```clojure
{:deps {io.github.ssjoleary/thoughts.bb
        #_"You use the newest SHA here:"
        {:git/sha "389833f393e04d4176ef3eaa5047fa307a5ff2e8"}}
 :tasks
 {:init (def opts {:blog-title "thoughts"
                   :blog-description "An attempt at a quieter, slower, more personal internet"})
  thoughts {:doc "Run `bb thoughts help` for details."
            :requires ([thoughts.cli :as cli])
            :task (cli/dispatch opts)}}}
```

To put a new thought to virtual paper:

```clojure
$ bb thoughts new --file "test.md" --title "Test"
```

This will create a new file and pop open whatever `$EDITOR` is set to, defaulting to `vi`.

To start an HTTP server and re-render on changes to files:

```
$ bb thoughts watch
```

### Clojure

thoughts can be used in Clojure with the exact same API as the bb tasks.
Default options can be configured in `:exec-args`.

```clojure
:thoughts
{:deps {io.github.ssjoleary/thoughts.bb
        #_"You use the newest SHA here:"
        {:git/sha "389833f393e04d4176ef3eaa5047fa307a5ff2e8"}
        org.babashka/cli {:mvn/version "0.3.35"}}
 :main-opts ["-m" "babashka.cli.exec" "thoughts.cli" "run"]
 :exec-args {:blog-title "thoughts"
             :blog-description "An attempt at a quieter, slower, more personal internet"}}
```

After configuring this, you can call:

```
$ clj -M:thoughts new --file "test.md" --title "Test"
```

To watch:

```
$ clj -M:thoughts watch
```

etc.

## Features

### Markdown

Thoughts are written in Markdown and processed by
[markdown-clj](https://github.com/yogthos/markdown-clj), which implements the
[MultiMarkdown](https://github.com/fletcher/MultiMarkdown/wiki/MultiMarkdown-Syntax-Guide)
flavour of Markdown.

### Metadata

Thought metadata is specified in the thought file using [MultiMarkdown's metadata
tags](https://github.com/fletcher/MultiMarkdown/wiki/MultiMarkdown-Syntax-Guide#metadata).
thoughts.bb expects three pieces of metadata in each post:

- `Title` - the title of the post
- `Date` - the date when the post will be published (used for sorting posts, so
  [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) datetimes are recommended)
- `Tags` - a comma-separated list of tags

`thoughts new` generates a datetime stamp for the title and also provides sensible
defaults for `Date` and `Tags`. The idea being that you can jump straight in to
typing out a quick thought and know exactly when you thought it up! Feel free to
edit these fields, especially the `Title` if it suits you better.

You can add any metadata fields to posts that you want. See the [Social
sharing](#social-sharing) section below for some useful suggestions.

**Note: metadata may not include newlines!**

### favicon

**NOTE:** when enabling or disabling a favicon, you must do a full re-render of
your site by running `bb thoughts clean` and then your `bb thoughts render`
command.

To enable a [favicon](https://en.wikipedia.org/wiki/Favicon), add `:favicon
true` to your thoughts.bb opts (or use `--favicon true` on the command line).
thoughts.bb will render the contents of `templates/favicon.html` and insert them
in the head of your pages.

You will also need to create the favicon assets themselves. The easiest way is
to use a favicon generator such as
[RealFaviconGenerator](https://realfavicongenerator.net/), which will let you
upload an image and then gives you a ZIP file containing all of the assets,
which you should unzip into your `:assets-dir` (which defaults to `assets`).

You can read an example of how to prepare a favicon here:
https://jmglov.net/blog/2022-07-05-hacking-blog-favicon.html

thoughts.bb's default template expects the favicon files to be named as follows:

- `android-chrome-192x192.png`
- `android-chrome-512x512.png`
- `apple-touch-icon.png`
- `browserconfig.xml`
- `favicon-16x16.png`
- `favicon-32x32.png`
- `favicon.ico`
- `mstile-70x70.png`
- `mstile-144x144.png`
- `mstile-150x150.png`
- `mstile-310x150.png`
- `mstile-310x310.png`
- `safari-pinned-tab.svg`
- `site.webmanifest`

If any of these files are not present in your `:assets-dir`, a thoughts.bb default
will be copied there from `resources/thoughts/assets`.

### Social sharing

Social media sites such as Facebook, Twitter, LinkedIn, etc. display neat little
preview cards when you share a link. These cards are populated from certain
`<meta>` tags (as described in "[How to add a social media share card to any
website](https://dev.to/mishmanners/how-to-add-a-social-media-share-card-to-any-website-ha8)",
by Michelle Mannering) and typically contain a title, description / summary, and
preview image.

thoughts.bb's [base
template](https://github.com/ssjoleary/thoughts.bb/blob/389833f393e04d4176ef3eaa5047fa307a5ff2e8/resources/thoughts/templates/base.html)
adds meta tags for the page title for all pages and descriptions for the
following pages:

- Index: `{{blog-description}}`
- Tags: Tags - `{{blog-description}}`
- Tag pages: Thoughts tagged "`{{tag}}`" - `{{blog-description}}`

If you specify a `:blog-image URL` option, a preview image will be added to the
index, archive, tags, and tag pages. The URL should point to an image; for best
results, the image should be 1200x630 and maximum 5MB in size. It may either be
an absolute URL or a URL relative to `:blog-root`.

For thought pages, meta tags will be populated from `Title`, `Description`,
`Image`, `Image-Alt`, and `Twitter-Handle` metadata in the document.

If not specified, `Twitter-Handle` defaults to the `:twitter-handle` option to
thoughts.bb. The idea is that the `:twitter-handle` option is the Twitter handle
of the person owning the blog, who is likely also the author of most thoughts on
the blog.

For example, a thought could look like this:

```text
Title: 2023-03-01T13:04:54
Date: 2023-03-01
Tags: demo
Image: assets/2023-03-01-sharing-preview.png
Image-Alt: A leather-bound notebook lies open on a writing desk
Twitter-Handle: thoughts.bb
Description: thoughts.bb now creates nifty social media sharing cards / previews. Read all about how this works!

You may have already heard the good news: thoughts.bb is more social than ever!
...
```

The value of the `Image` field is either an absolute URL or a URL relative to
`:blog-root`. As noted above, images should be 1200x630 and maximum 5MB in size
for best results.

`Image-Alt` provides alt text for the preview image, which is extremely
important for making pages accessible to people using screen readers. I highly
recommend reading resources like "[Write good Alt Text to describe
images](https://accessibility.huit.harvard.edu/describe-content-images)" to
learn more.

Resources for understanding and testing social sharing:

- [Meta Tags debugger](https://metatags.io/)
- [Facebook Sharing Debugger](https://developers.facebook.com/tools/debug/)
- [LinkedIn Post Inspector](https://www.linkedin.com/post-inspector/)
- [Twitter Card Validator](https://cards-dev.twitter.com/validator)

## Templates

thoughts.bb uses the following templates in site generation:

- `base.html` - All pages. Page body is provided by the `{{body}}` variable.
- `post.html` - Thought bodies.
- `style.css` - Styles for all pages.
- `favicon.html` - If `:favicon true`, used to include favicon in the `<head>`
  of all pages.
- `tags.html` - Tag overview page.
- `post-links.html` - Used to render lists of thoughts in the archive and
  each page corresponding to a single tag.
- `index.html` - Index page. Thoughts containing the marker comment

thoughts.bb looks for these templates in your `:templates-dir`, and if it doesn't
find them, will copy a default template into that directory. It is recommended
to keep `:templates-dir` under revision control so that you can modify the
templates to suit your needs and preferences.

The default templates are occasionally modified to support new features. When
this happens, you won't be able to use the new feature without making the same
modifications to your local templates. The easiest way to do this is to run `bb
thoughts.bb refresh-templates`.

## Improvements

Feel free to send PRs for improvements.

My wishlist (taken from quickblog):

- There might be a few things hardcoded that still need to be made configurable.
- Upstream improvements to [markdown-clj](https://github.com/yogthos/markdown-clj)
