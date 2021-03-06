[properties_documentation]: http://docs.oracle.com/javase/6/docs/api/java/util/Properties.html#load(java.io.Reader)

# ProperProperties

[![Build Status](http://img.shields.io/travis/tnarik/proper_properties.svg)](https://travis-ci.org/tnarik/proper_properties)
[![Code Climate](http://img.shields.io/codeclimate/github/tnarik/proper_properties.svg)](https://codeclimate.com/github/tnarik/proper_properties)
[![Coveralls](http://img.shields.io/coveralls/tnarik/proper_properties.svg)](https://coveralls.io/r/tnarik/proper_properties)
[![RubyGems](http://img.shields.io/gem/v/proper_properties.svg)](http://rubygems.org/gems/proper_properties)
[![Gemnasium](http://img.shields.io/gemnasium/tnarik/proper_properties.svg)](https://gemnasium.com/tnarik/proper_properties)

A ruby library to read and write [Java properties files](http://en.wikipedia.org/wiki/.properties), which format is better described [in the Java documentation][properties_documentation].

## Installation

Install via Rubygems or Gemfile

```zsh
$ gem install proper_properties
```

## Loading files

You can load a valid Java properties file from the file system using a path:

```ruby
properties = ProperProperties.load("path/to/my.properties")
properties[:foo] # => "bar"
properties['foo'] # => "bar"
```

If have already the content of the properties file at hand than parse the content as:

```ruby
properties = ProperProperties.load("foo=bar")
properties[:foo] # => "bar"
properties['foo'] # => "bar"
```

## Writing files

You can write any Hash-like structure (with symbol or string keys) as a properties file:

```ruby
hash = {:foo => "bar", "foobar" => "barfoo"}
ProperProperties.write(hash, "path/to/my.properties")
```

Or if you want to omit the file you can receive the content directly:

```ruby
hash = {:foo => "bar"}
ProperProperties.generate(hash)  # => "foo=bar"
```

## Encodings and special chars

Although ISO 8859-1 is assumed for Java properties files, they normally use UTF-8 encoding. This tool tries to convert them:

```
"ה" <=> "\u05d4"
```

The tool also escaped every '=', ' ' and ':' in the name part of a property line:

```ruby
ProperProperties.generate({"i : like=strange" => "bar"}) 
# => "i\ \:\ like\=strange=bar"
```

## Multi line and line breaks

In Java properties files there is distinction between natural lines and logical lines (than may span several lines by escaping the line terminator characters). 

Assume the following input:

```ini
my=This is a multi \
   line content with only \n one line break
```

The parses would read:

```ruby
{:my => "This is a multi line content which only \n one line break"}
```

In the opposite direction line breaks will be correctly escaped but the generator will never use multi line values.

## Contributing

1. [Fork it!](https://github.com/tnarik/proper_properties/fork)
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request

## Author

- Tnarik Innael (@tnarik)
- Jonas Thiel (@jonasthiel) : [original project](https://github.com/jnbt/java-properties) upto [1f2c4b008d69d0eae1084b1adfdea814e2b29899](https://github.com/tnarik/proper_properties/commit/1f2c4b008d69d0eae1084b1adfdea814e2b29899)

## References

For a proper description about the properties file format have a look at the [Java Plattform documentation][properties_documentation].

## License

This gem is released under the MIT License. See the LICENSE.txt file for further details.
