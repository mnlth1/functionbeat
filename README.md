# functionbeat
Aggregate data from cloudwatch and ship it to Elastic Stack. 

This would provide overview of deploying functionbeat as lambda to receive events from Cloudwatch log groups and stream it to ElasticSearch on AWS.

## Pre-requisite
AWS account and elasticsearch setup on AWS. Also, AWS credentials are set in the environment variables.

Functionbeat distribution downloaded from 

  `https://www.elastic.co/downloads/beats/functionbeat`


## Configuration
  1. Setup the IAM role to be able to deploy functionbeat with required lambda permissions
  
      `functionbeat-iam.yaml` consists of required role policies
  
  2. Configure functionbeat lambda function with triggers 
  
      `functionbeat.yaml` contains the lambda function configurations under `functionbeat.provider.aws.functions`
     
## Deploy
Deploy from the functionbeat install location.

Since there's IAM role dependency for lambda to execute, deploy IAM role before functionbeat lambda.

#### Order of deployment:
IAM role would be created by running cloudformation stack

    aws cloudformation deploy \
	    --stack-name functionbeat-log-aggregator \
	    --capabilities CAPABILITY_IAM CAPABILITY_NAMED_IAM \
	    --template-file functionbeat-iam.yml

Once the role is successfully created, deploy the functionbeat installation. Firstly, validate the configuration of functionbeat using:

    ./functionbeat test config -e

Deploy the package using command:

    ./functionbeat -v -e -d "*" deploy fb-cloudwatch

NOTE: If you would anytime want to update the function, use `update` command instead of `deploy`
