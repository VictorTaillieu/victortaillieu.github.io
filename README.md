This was forked (then detached) by [Victor Taillieu](https://github.com/VictorTaillieu) from the [Academic Pages Jekyll Theme](https://academicpages.github.io/) itself forked from [Minimal Mistakes Jekyll Theme](https://mmistakes.github.io/minimal-mistakes/), which is © 2016 Michael Rose and released under the MIT License. See LICENSE.

# Instructions to run locally

1. Clone the repository
1. Make sure you have ruby-dev, bundler, and nodejs installed: `sudo apt install ruby-dev ruby-bundler nodejs`
1. Run `bundle clean` to clean up the directory (no need to run `--force`)
1. Run `bundle install` to install ruby dependencies. If you get errors, delete Gemfile.lock and try again.
1. Run `bundle exec jekyll serve --livereload` to generate the HTML and serve it from `localhost:4000` the local server will automatically rebuild and refresh the pages on save.

# Key files and directories

* Basic config options: `_config.yml`
* Top navigation bar config: `_data/navigation.yml`
* Single pages: `_pages/`
* Collections of pages are .md or .html files in:
  * `_portfolio/`
  * `_posts/`
* Footer: `_includes/footer.html`
* Static files (like PDFs): `/files/`
* Profile image (set in `_config.yml`): `images/profile.png`

# More documentation

- [Markdown guide](https://academicpages.github.io/markdown/)
- [Liquid syntax guide](https://shopify.github.io/liquid/tags/control-flow/)
- [Guides for the Minimal Mistakes theme](https://mmistakes.github.io/minimal-mistakes/docs/configuration/)
