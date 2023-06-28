First, let's configure Filebeat to gather the Nginx logs:

1. Install Filebeat: Download and install Filebeat from the Elastic website (https://www.elastic.co/downloads/beats/filebeat).
2. Configure the input: Create a new configuration file for Filebeat by opening the filebeat.yml file in a text editor. Add the following configuration to the file:

```
filebeat.inputs:
- type: log
  enabled: true
  paths:
    - /var/log/nginx/*.log
```

In this configuration, we're telling Filebeat to watch the /var/log/nginx/ directory for any .log files and to read those files as input.

1. Configure the output: Next, we need to set up the output for Filebeat. We can use Logstash as the output by adding the following configuration:

```
output.logstash:
  enabled: true
  hosts: ["logstash:5044"]
```

This configuration sets up Filebeat to send the data to Logstash at the address "logstash" on port 5044.

1. Start Filebeat: Finally, start Filebeat by running the following command:

```
sudo systemctl start filebeat
```

And that's it! Filebeat is now configured to gather Nginx logs and send them to Logstash.

Now, let's configure Logstash to receive the logs from Filebeat:

1. Install Logstash: Download and install Logstash from the Elastic website (https://www.elastic.co/downloads/logstash).
2. Create a configuration file: Create a new file called nginx.conf in the /etc/logstash/conf.d/ directory. Add the following configuration to the file:

```
input {
  beats {
    port => 5044
  }
}

filter {
  if [fields][type] == "nginx_access" {
    grok {
      match => { "message" => "%{IPORHOST:http_host} %{USER:remote_user} \[%{HTTPDATE:timestamp}\] \"%{WORD:http_method} %{DATA:http_request} HTTP/%{NUMBER:http_version}\" %{NUMBER:response_code} %{NUMBER:body_bytes_sent} \"%{DATA:http_referer}\" \"%{DATA:http_user_agent}\" \"%{DATA:http_x_forwarded_for}\"" }
    }
  }
}

output {
  elasticsearch {
    hosts => ["localhost:9200"]
    index => "nginx_access-%{+YYYY.MM.dd}"
  }
}
```

In this configuration, we're telling Logstash to listen on port 5044 for incoming Filebeat data. We're also setting up a filter to parse the Nginx access logs using a grok pattern. Finally, we're sending the parsed data to Elasticsearch.

Note that we're assuming that you have Elasticsearch installed on the same machine as Logstash and that it's running on the default port (9200).

1. Start Logstash: Finally, start Logstash by running the following command:

```
sudo systemctl start logstash
```

And that's it! You've now configured Filebeat to gather Nginx logs and Logstash to receive and parse them. The parsed data will be sent to Elasticsearch for storage and analysis.