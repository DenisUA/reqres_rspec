# ReqresRspec

Gem generates API documentation from your integration tests written with `rspec`.

No additional DSL needed. Beside covering rspec tests, documentation may be extended with API controller action comments in `yardoc` style.

Documentation is generated in JSON, YAML, HTML, PDF formats.

## Installation

### 1) Gem

Just add this gem to `Gemfile` of your API Application

    gem 'reqres_rspec', group: :test

And then execute:

    $ bundle

### 2) PDF generator

Install `prince` http://www.princexml.com/download/ . For MacOS installation commands are

```
wget http://www.princexml.com/download/prince-9.0r2-macosx.tar.gz
tar -xvf prince-9.0r2-macosx.tar.gz
cd prince-9.0r2-macosx
./install.sh
```

## Usage

by default `reqres_rspec` is not active (this may be configured!). To activate it, run `rspec` with

`REQRES_RSPEC=1 bundle exec rspec`

Documentation will be put into your application's `/doc` folder

### Sample controller action

```ruby
  # @description creates Category from given parameters
  # description text may be multiline
  # @params category[title] required String Category title
  # @params category[weight] in which order Category will be shown
  # param text may also be multiline
  def create
    category = Category.new(create_category_params)

    if category.save
      render json: { category: category }.to_json, status: 201
    else
      render json: { errors: category.errors.full_messages }, status: 422
    end
  end
```

Description param text is started with `@description` and may be multiline.
Each param text is started with `@params` and first word will be param name, then optionally `required`, then optionally type (`Integer`, `String` etc), and finally param description, which may be multiline as well.

### Sample rspec test

```ruby
  describe 'Create' do
    it 'creates category' do
      post :create, category: { name: 'Cookies' }
      ...
    end

    it 'some other example', collect_for_doc: false do
      ...
    end
  end
```

 By default all examples will be added to docs. Example may be excluded from docs with option `collect_for_doc: false`

 Doc will use full example description, as a title for each separate spec

### Generates documentation example

[![Generated Doc](http://i44.tinypic.com/kda1pw.png)](http://i44.tinypic.com/kda1pw.png)
[![Generated Doc](http://i39.tinypic.com/2w3p6vl.png)](http://i39.tinypic.com/2w3p6vl.png)

Documentation is written in HTML format, which then converted to PDF. PDF files are textual, support search and have internal navigation links

## Configuration

TODO: Write instruction on gem configuration

## Contributing

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request
