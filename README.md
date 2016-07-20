# Logstash Debug Env

A helpful environment which can be used to test logstash and plugins.

## Running your unpublished Plugin in Logstash

### Run in a local Logstash clone

- Edit Logstash `Gemfile` and add the local plugin path, for example:
```ruby
gem "logstash-filter-awesome", :path => "/your/local/logstash-filter-awesome"
```
- Install plugin
```sh
bin/plugin install --no-verify
```
- Run Logstash with your plugin
```sh
bin/logstash -e 'filter {awesome {}}'
```

- A better way to test with configs
```
# make sure the following is in the config:
# output {
#    stdout {codec => rubydebug}
#    file {
#         codec => "json_lines"
#         path => "../debug-filters.json"
#     }
# }
bin/logstash --config ../configs -l ../logstash-debug.log
```

At this point any modifications to the plugin code will be applied to this local Logstash setup. After modifying the plugin, simply rerun Logstash.

### Run in an installed Logstash

You can use the same **2.1** method to run your plugin in an installed Logstash by editing its `Gemfile` and pointing the `:path` to your local plugin development directory or you can build the gem and install it using:

- Build your plugin gem
```sh
gem build logstash-filter-awesome.gemspec
```
- Install the plugin from the Logstash home
```sh
bin/plugin install /your/local/plugin/logstash-filter-awesome.gem
```
- Start Logstash and proceed to test the plugin
