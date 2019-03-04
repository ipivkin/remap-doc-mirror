![Middleman Version](https://img.shields.io/badge/Middleman-4.1.1-yellow.svg)
![Build Status](https://gitlab.com/middleman-themes/middleman/badges/master/build.svg)

----

View site: https://middleman-themes.gitlab.io/middleman/

----

# Build a [Middleman] site with GitLab Pages

## Configurations:

- Theme: Middleman Default Theme provided via `middleman init project` (with small adaptations)
- Build method: with [Bundler] - `bundle exec middleman build`
- GitLab CI - view [`.gitlab-ci.yml`]
- Bundler - view [`Gemfile`][Gemfile]
- Middleman - view [`config.rb`]


## Usage:

_**Note:** we assume you have [Middleman 4.1.1] installed, up and running locally._

- Fork, clone or download this project
- Install [Bundler] if you don't have it already - `gem install bundler`
- Open the [Gemfile] and add or remove gems to work locally on the group `development`
- Configure [`config.rb`] according to your project
- Navigate to the project folder (`cd path/to/project`)
- Run `bundle install`
- Serve middleman with bundler - `bundle exec middleman server` and/or
- Build Middleman with bundler - `bundle exec middleman build`

## Notes:

- You won't need the gems in the [`development` group][Gemfile] for deploying your site remotely, that's why
we isolated them. See [Bundler groups documentation][b-doc] for reference.
- About `config.rb`: do NOT change the line 39: `set :build_dir, 'public'` otherwise GitLab Pages will not work with your new project.
- About the theme: we slightly addapted the default theme by adding a custom [stylesheet] and an [extra page].

----

## Working with other Middleman Themes:

Some projects might need other gems or configurations to work, locally or remotelly. But, generally, what you can't miss to use Middleman with GitLab Pages is:

- Make sure you set the build path to `public` on `config.rb` (`set :build_dir, 'public'`)
- Make sure you have the `.giltab-ci.yml` with the following minimal configurations:


```yaml
# requiring the environment of Ruby 2.1.x
image: ruby:2.1

# add bundle cache to 'vendor' for speeding up builds
cache:
  paths: 
    - vendor/

before_script:
  - apt-get update -qy
  - apt-get install -y nodejs # JS runtime
  - bundle install --path vendor # view notes for adding the flag '--without development'

# the 'pages' job will deploy and build your site to the 'public' path
pages:
  stage: deploy
  script:
    - bundle exec middleman build
  artifacts:
    paths:
      - public
  only:
    - master # this job will affect only the 'master' branch
```

----

## FAQ

### Do I need the `development` group in my `Gemfile`?

No, you don't. Set it up as you wish. 

If you don't group the gems, your GitLab CI [configuration file][`.gitlab-ci.yml`] won't need the flag `--without development`.

### Why do I need this: `set :build_dir, 'public'` ?

The default build directory for Middleman is called `build` but GitLab Pages is set 
to build the site from the `public` directory. So we need to tell Middleman to build the site
to the `public` folder instead.

----

## Enjoy!


[Bundler]: http://bundler.io
[b-doc]: http://bundler.io/v1.11/groups.html
[Middleman]: https://middlemanapp.com/
[Middleman 4.1.1]: https://rubygems.org/gems/middleman/versions/4.1.1
[`config.rb`]: https://gitlab.com/middleman-themes/middleman/blob/master/config.rb
[Gemfile]: https://gitlab.com/middleman-themes/middleman/blob/master/Gemfile
[`.gitlab-ci.yml`]: https://gitlab.com/middleman-themes/middleman/blob/master/.gitlab-ci.yml
[stylesheet]: https://gitlab.com/middleman-themes/middleman/blob/master/source/stylesheets/_custom.scss
[extra page]: https://gitlab.com/middleman-themes/middleman/blob/master/source/hello-world.html.erb