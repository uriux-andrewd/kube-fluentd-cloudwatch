# kube-fluentd-cloudwatch (URIUX Branch Only)
Collecting Docker Log Files with Fluentd and AWS CloudWatch

This will create a Container that will run on any kubernetes cluster to collect logs and send them to CloudWatch. No other configuration is necessary.


## How to deploy

**Add Log Policy to Your EC2 Instances**

If you don't do this, you won't see any logs appear in cloud watch.


1.	Services / EC2
1.	Select a node instance
1.	Under properties, click ‘IAM role’ link
1.	Click ‘Attach Policies’
1.	Add CloudWatchLogsFullAccess
1.	Click ‘Attach’



**Create a secrets file**

    cp aws-secret.yaml.template aws-secret.yaml


Add your credentials, taking care to encode your values in base 64. 

**example**

    echo -n 'AKIAICQ4CEXI5DFETE' | base64
    echo -n 'JwWRJiY5jg9U9RABT/hSEA/DUIYRESW' | base64
    echo -n 'us-west-2' | base64



        
**edit fluentd-configmap.yaml with log_group**

**example**

    <match kubernetes.**>
      @type cloudwatch_logs
      log_group_name sockshop
      log_stream_name_key container_name
      auto_create_stream true
      put_log_events_retry_limit 20
    </match>


>note that log_group_name can be used with any text string instead of using log_group_key


**deploy**

    kubectl apply -f ./fluentd-ds.yaml
    kubectl apply  -f ./fluentd-configmap.yaml



