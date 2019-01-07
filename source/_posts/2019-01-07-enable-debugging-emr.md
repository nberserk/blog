---
title: enable debugging option on aws EMR
tags: 
- emr
- aws
---

aws web에는 EMR을 띄울때 enable debugging하는 옵션이 있는데 boto3
에서는 [해당 옵션이 없어서](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/emr.html#client) 찾아보니 아래처럼 custom step 으로 해줘야 한다고..


```python
client = boto3.client('emr', region_name='us-west-2', verify=False)

response = client.run_job_flow(
    Name="launch test cluster",
    LogUri=S3_LOG_URI,
    ReleaseLabel='emr-5.19.0',
    Instances={
        'MasterInstanceType': 'm4.xlarge',
        'SlaveInstanceType': 'r3.xlarge',
        'InstanceCount': 3,
        'KeepJobFlowAliveWhenNoSteps': True,
        'TerminationProtected': False,
        'Ec2SubnetId': EC2_SUBNET,
        'Ec2KeyName': EC2_KEY,
    },
    VisibleToAllUsers=True,
    JobFlowRole='EMR_EC2_DefaultRole',
    ServiceRole='EMR_DefaultRole',
    Applications=[
        {
            'Name': 'Spark'
        },
        {
            'Name': 'Zeppelin'
        }
    ],
    Steps=[
        {
            'Name': 'Setup Debugging',
            'ActionOnFailure': 'TERMINATE_CLUSTER',
            'HadoopJarStep': {
                'Jar': 's3://us-west-2.elasticmapreduce/libs/script-runner/script-runner.jar',
                'Args': ['s3://us-west-2.elasticmapreduce/libs/state-pusher/0.1/fetch']
            }
        }
    ]
)

cluster_id = response["JobFlowId"]

print('launching...')

waiter = client.get_waiter('cluster_running')
waiter.wait(ClusterId=cluster_id)

print('launch cluster id: {0}'.format(cluster_id))

```