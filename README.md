# NAME

Dancer::Plugin::JSON::Schema - JSON::Schema interface for Dancer applications

# VERSION

version 0.001

# SYNOPSIS

    use Dancer;
    use Dancer::Plugin::JSON::Schema qw(json_schema);

    post '/search' => sub {
      my $structure = param('q');
      my $result = json_schema('default')->validate($structure);

      # If you are accessing the 'default' schema, then you can just do:
      my $result = json_schema->validate($structure);

      ...
    };

    dance;

# DESCRIPTION

This plugin makes it very easy to create [Dancer](https://metacpan.org/pod/Dancer) applications that interface
with JSON Schema.

It automatically exports the keyword `json_schema` which returns an [JSON::Schema](https://metacpan.org/pod/JSON::Schema) object.

You just need to configure where to get the schema from.

For performance, JSON::Schema objects are cached in memory and are lazy loaded the first time they are accessed.

# CONFIGURATION

Configuration can be done in your [Dancer](https://metacpan.org/pod/Dancer) config file.

## Simple example

Here is a simple example. It defines one schema named `default`:

    plugins:
      'JSON::Schema':
        default:
          schema: schemas/item.json

## Multiple schemas

In this example, there are 2 schemas configured named `default` and `accessories`:

    plugins:
      'JSON::Schema':
        default:
          schema: schemas/item.json
        user:
          schema: schemas/user.json

Each schema configured must at least have a `schema` option set.

If you only have one schema configured, or one of them is named
`default`, you can call `json_schema` without an argument to get the only
or `default` schema, respectively.

## schema\_info

Alternatively, you may also declare your schema information inside a hash named `schema_info`:

    plugins:
      'JSON::Schema':
        default:
          schema_info:
            schema: schemas/item.json

## alias

Aliases allow you to reference the same underlying schema with multiple names.

For example:

    plugins:
      'JSON::Schema':
        default:
          schemas/item.json
        products:
          alias: default

Now you can access the default schema with `json_schema()`, `json_schema('default')`,
or `json_schema('products')`.

# FUNCTIONS

## json\_schema

    my $result = json_schema->validate( $structure );

The `json_schema` keyword returns a [JSON::Schema](https://metacpan.org/pod/JSON::Schema) object ready for you to use.

If you have configured only one schema, then you can simply call `json_schema` with no arguments.

If you have configured multiple schemas, you can still call `json_schema` with no arguments if there is a schema named `default` in the configuration.

With no argument, the `default` schema is returned.

Otherwise, you **must** provide `json_schema()` with the name of the schema:

    my $user = json_schema('accessories')->select( ... );

# AUTHORS AND CONTRIBUTORS

This module is based on [Dancer::Plugin::DBIC](https://metacpan.org/pod/Dancer::Plugin::DBIC), as at 22 October 2014, and adapted for JSON::Schema by Daniel Perrett.

The following had authored [Dancer::Plugin::DBIC](https://metacpan.org/pod/Dancer::Plugin::DBIC) at this time:

- Al Newkirk <awncorp@cpan.org>
- Naveed Massjouni <naveed@vt.edu>

The following had made contributions to [Dancer::Plugin::DBIC](https://metacpan.org/pod/Dancer::Plugin::DBIC) at this time:

- Alexis Sukrieh <sukria@sukria.net>
- Dagfinn Ilmari Mannsåker <[https://github.com/ilmari](https://github.com/ilmari)>
- David Precious <davidp@preshweb.co.uk>
- Fabrice Gabolde <[https://github.com/fgabolde](https://github.com/fgabolde)>
- Franck Cuny <franck@lumberjaph.net>
- Steven Humphrey <[https://github.com/shumphrey](https://github.com/shumphrey)>
- Yanick Champoux <[https://github.com/yanick](https://github.com/yanick)>

# AUTHORS

- Daniel Perrett <dp13@sanger.ac.uk>
- Al Newkirk <awncorp@cpan.org>
- Naveed Massjouni <naveed@vt.edu>

# COPYRIGHT AND LICENSE

This software is copyright (c) 2010 by awncorp.

This is free software; you can redistribute it and/or modify it under
the same terms as the Perl 5 programming language system itself.
