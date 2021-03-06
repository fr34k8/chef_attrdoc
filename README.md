chef_attrdoc
============
[![Gem Version](https://badge.fury.io/rb/chef_attrdoc.png)](http://badge.fury.io/rb/chef_attrdoc)
[![Build Status](https://travis-ci.org/mapleoin/chef_attrdoc.svg?branch=master)](https://travis-ci.org/mapleoin/chef_attrdoc)
[![Code Climate](https://codeclimate.com/github/mapleoin/chef_attrdoc.png)](https://codeclimate.com/github/mapleoin/chef_attrdoc)

*The problem:* README documentation gets outdated because it's not close to the code. A lot of cookbook documentation describes the configuration options that the cookbook provides; so it naturally lies in attributes files.

*The solution:* Extract documentation from chef cookbooks' attributes files and format and output it to the cookbook's README.md file.

`chef_attrdoc` groups attribute initialization lines together with the comments immediately above them. Any lines containing an attribute initialization which are not separated by an empty line are considered a group. The comment immediately above them is assumed to describe the group of attributes below. Groups of attribute initialization lines which are not immediately preceded by a comment line are ignored and will not show up in the output.

chef_attrdoc currently ignores *TODO*, *XXX*, *NOTE* and *foodcritic* comments.

### Usage:

```bash
$ gem install chef_attrdoc
$ chef_attrdoc ~/cookbooks/mycookbook
```

It's that simple. Only one command to run and `chef_attrdoc` will know how to do the rest.

`chef_attrdoc` will try to find an Attributes heading in the README.md file in that directory and replace its contents with the generated attributes documentation. The attributes documentation is compiled from all the files in the cookbook's `attributes/` directory. All the files ending in `.rb` in that directory are considered to be attributes files.

`chef_attrdoc` uses ruby's stdlib `ripper` module and so does not have any dependencies.

`chef_attrdoc` currently requires `ruby >= 1.9`.

### Examples

These are the two input files from a chef cookbook:

```bash
$ cat cookbook-example/attributes/default.rb
```
    # Copyright 1970, nobody

    # this is the attribute
    default['some']['attribute'] = 'foo'

    default['this']['will']['be'] = 'ignored'

    # NOTE code blocks without a comment are ignored as are those beginning
    # NOTE with 'NOTE', 'XXX', 'TODO' or foodcritic comments

    # a longer block of code
    case something
    when 'foo'
      default['some']['foo'] = 'baz'
    else
      default['some']['foo'] = 'qux'
    end

```bash
$ cat cookbook-example/README.md
```
    chef_attrdoc example README
    ===========================

    This is just an example

    Attributes
    ==========
    nothing here now


    License
    =======
    This is usually important.

This is the output README file after running:

```bash
$ chef_attrdoc cookbook-example
$ cat cookbook-example/README.md
```
    ```
    chef_attrdoc example README
    ===========================

    This is just an example

    Attributes
    ==========

    ## default.rb

    this is the attribute

    ```ruby
    default['some']['attribute'] = 'foo'
    ```

    a longer block of code

    ```ruby
    case something
    when 'foo'
      default['some']['foo'] = 'baz'
    else
      default['some']['foo'] = 'qux'
    end
    ```


    License
    =======
    This is usually important.
    ```

Here are some longer examples from openstack chef cookbooks:

[openstack-identity attributes file](https://github.com/stackforge/cookbook-openstack-identity/blob/552488824aa720f08249bc4d19f5224e9382aa00/attributes/default.rb) - [output](https://gist.github.com/mapleoin/d211b29f68dd519a4878#attributes)

[openstack-common attributes file](https://github.com/stackforge/cookbook-openstack-common/tree/f0e55b8742d9285742163d8c2633833dc6f4241c/attributes) - [output](https://gist.github.com/mapleoin/d211b29f68dd519a4878#attributes)
