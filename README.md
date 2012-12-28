# JSON Schema Tools

[![Build Status](https://travis-ci.org/salesking/json_schema_tools.png?branch=master)](https://travis-ci.org/salesking/json_schema_tools)

Set of tools to help working with JSON Schemata:

* read schema files into a ruby hash
* add schema properties to a class
* convert any object into it's schema json markup
* clean parameters according to a schema (e.g. in an api controller)

## Usage

Hook the gem into your app

    gem 'json_schema_tools'

## Read Schema

Before the fun begins, with any of the tools, one or multiple JSON schema files
must be available(read into a hash). So first provide a base path where the
schema.json files are located.

    SchemaTools.schema_path = '/path/to/schema-json-files'

Now you can read a single or multiple schemas:

    schema = SchemaTools::Reader.read :client
    # read all *.json files in schema path
    schemata = SchemaTools::Reader.read_all
    # see schema cached in registry
    SchemaTools::Reader.registry[:client]

Read files from a custom path?

    schema = SchemaTools::Reader.read :client, 'my/path/to/json-files'
    schemata = SchemaTools::Reader.read_all 'my/path/to/json-files'

Don't like the global path and registry? Go local:

    reader = SchemaTools::Reader.new
    reader.read :client, 'from/path'
    reader.registry


## Object to Schema

A schema provides a (public) contract about an object definition. Therefore an
internal object is converted to it's schema version on delivery(API access).
First the object is converted to a hash containing only the properties(keys)
from its schema definition. Afterwards it is a breeze to convert this hash into
JSON, with your favorite generator.

Following uses client.json schema(same as peter.class name) inside the global
schema_path and adds properties to the clients_hash simply calling
client.send('property-name'):

    peter = Client.new name: 'Peter'
    client_hash = SchemaTools::Hash.from_schema(peter)
    #=> "client"=>{"id"=>12, "name"=> "Peter", "email"=>"",..} # whatever else you have as properties
    # to_json is up to you .. or your rails controller

### Customise Schema Hash

Only use some fields e.g. to save bandwidth

    client_hash = SchemaTools::Hash.from_schema(peter, fields:['id', 'name'])
    #=> "client"=>{"id"=>12, "name"=> "Peter"}

Use a custom schema name e.g. to represent a client as contact. Assumes you also
have a schema named contact.json

    client_hash = SchemaTools::Hash.from_schema(peter, class_name: 'contact')
    #=> "contact"=>{"id"=>12, "name"=> "Peter"}

Use a custom schema path

    client_hash = SchemaTools::Hash.from_schema(peter, path: 'path-to/json-files/')
    #=> "client"=>{"id"=>12, "name"=> "Peter"}

## Parameter cleaning

Hate people spamming your api with wrong object fields? Use the Cleaner to
check incoming params.

For example in a client controller

    def create
      SchemaTools::Cleaner.clean_params!(:client, params[:client])
      # params[:client] now only has keys defined as writable in client.json schema
      #..create and save client
    end

## Object attributes from Schema

The use-case here is to add methods, defined in schema properties, to an object.
Very usefull if you are building a API client and don't want to manually add
methods to you local classes .. like people NOT using JSON schema

    class Contact
      include SchemaTools::Modules::Attributes
      has_schema_attrs :client
    end

    contact = Client.new
    contact.last_name = 'Rambo'
    # raw access
    contact.schema_attrs

## Test

Only runs on Ruby 1.9

    bundle install
    bundle exec rake spec


Copyright 2012-1013, Georg Leciejewski, MIT License
