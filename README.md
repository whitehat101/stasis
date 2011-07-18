Stasis
======

Stasis is a dynamic framework for static sites.

Install
-------

Install via RubyGems:

<!-- language:console -->

    $ gem install stasis

Templates
---------

At its most essential, Stasis takes a directory tree with [supported template files](#supported_markup_languages) and renders them.

Example directory structure:

<!-- language:console -->

    project/
        index.html.haml
        other.txt

Run `stasis`:

<!-- highlight:stasis language:console -->

    $ cd project
    $ stasis

Stasis creates a `public` directory:

<!-- highlight:public/ language:console -->

    project/
        index.html.haml
        other.txt
        public/
            index.html
            other.txt

`index.html.haml` renders to `public/index.html`.

`other.txt` is copied as-is because `.txt` is an unrecognized template extension.

Controllers
-----------

Controllers contain Ruby code that is evaluated once before all templates render.

Example directory structure:

<!-- highlight:controller.rb language:console -->

    project/
        controller.rb
        index.html.haml
        subdirectory/
            controller.rb
            index.html.haml

Before
------

Use `before` blocks within `controller.rb` to execute code right before a template renders.

`controller.rb`:

    before 'index.html.haml' do
      @something = true
    end

`@something` is now available to the `index.html.haml` template.

The `before` method can take any number of paths and/or regular expressions:

    before 'index.html.haml', /.*html.erb/ do
      @something = true
    end

Layouts
-------

`layout.html.haml`:

    %html
      %body= yield

In `controller.rb`, set the default layout:

    layout 'layout.html.haml'

Set the layout for a particular template:

    layout 'index.html.haml' => 'layout.html.haml'

Use a regular expression:

    layout /.*html.haml/ => 'layout.html.haml'

Set the layout from a `before` block:

    before 'index.html.haml' do
      layout 'layout.html.haml'
    end

Render
------

Within a template:

    %html
      %body= render '_partial.html.haml'

Within a `before` block:

    before 'index.html.haml' do
      @partial = render '_partial.html.haml'
    end

Text:

    render :text => 'Hello'

Local variables:

    render 'index.html.haml', :locals => { :x => true }

Include a block for the template to `yield` to:

    render 'index.html.haml' { 'Hello' }

Instead
-------

The `instead` method changes the output of the template being rendered:

    before 'index.html.haml' do
      instead render('subdirectory/index.html.haml')
    end

Helpers
-------

`controller.rb`:

    helpers do
      def say_hello
        'Hello'
      end
    end

The `say_hello` method is now available to all `before` blocks and templates.

Ignore
------

Use the `ignore` method in `controller.rb` to ignore certain paths.

Ignore filenames with an underscore at the beginning:

    ignore /_.*/

Priority
--------

Use the `priority` method in `controller.rb` to change the file process order.

Copy `.txt` files before rendering `index.html.erb`:

    priority /.*txt/ => 2, 'index.html.erb' => 1

The default priority is `0` for all files.

More
----

### Development Mode

To continuously regenerate your project as you modify files, run:

<!-- highlight:-d language:console -->

    $ stasis -d

### Supported Markup Languages

Stasis uses [Tilt](https://github.com/rtomayko/tilt) to support the following template engines:

<!-- language:console -->

    ENGINE                     FILE EXTENSIONS
    -------------------------- ----------------------
    ERB                        .erb, .rhtml
    Interpolated String        .str
    Erubis                     .erb, .rhtml, .erubis
    Haml                       .haml
    Sass                       .sass
    Scss                       .scss
    Less CSS                   .less
    Builder                    .builder
    Liquid                     .liquid
    RDiscount                  .markdown, .mkd, .md
    Redcarpet                  .markdown, .mkd, .md
    BlueCloth                  .markdown, .mkd, .md
    Kramdown                   .markdown, .mkd, .md
    Maruku                     .markdown, .mkd, .md
    RedCloth                   .textile
    RDoc                       .rdoc
    Radius                     .radius
    Markaby                    .mab
    Nokogiri                   .nokogiri
    CoffeeScript               .coffee
    Creole (Wiki markup)       .creole
    Yajl                       .yajl