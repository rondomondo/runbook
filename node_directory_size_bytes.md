# node_directory_size_bytes  

`node_directory_size_bytes` is bash script. It takes a list of one or more directory names.
 For each of these directory names it calculates some metrics around disk usage and emit
 this to stdout.

The format of the metrics will be consistent with the prometheus node_exporter. We use this
 custom script solution when the metrics we need are not provided natively by node_exporter
 or we have restricted access to them via node_exporter configuration directives.

<br>

*See the usage/help for more info*
```

    Usage :  node_directory_size_bytes.sh [-h|-o] dirname,...

    dirname:  One or more directory names to generate a disk usage metric for
        
    Examples:
        ./node_directory_size_bytes.sh /opt/proc/kafka /tmp/kafka_uploader

        Emits:
            node_directory_size_bytes{path="/opt/proc/kafka"} 123912
            node_directory_size_bytes{path="/tmp/kafka_uploader"} 148880
    
    Options:
        -h  Display this message
        -o  Output the result as json [not implemented]      
```


---

## Quickstart  

#### `Show the help`
```bash
# display the help/usage for this command
./node_directory_size_bytes.sh -h
```
  

#### `Emit a disk space metric for /var/log`
```bash
./node_directory_size_bytes.sh /var/log
```
  
#### `Emit a disk space metric for /tmp/kafka_uploader and /var/log`
```bash
# more than one directory etc....
./node_directory_size_bytes.sh /tmp/kafka_uploader /var/log
```

## Misc  

### `Output`
All output goes to STDOUT or STDERR, so capture it (via sponge possible) and pipe to file etc.  

### `Execution`
This just a bash script so run on command line or by cron or perhaps another orchestration etc...