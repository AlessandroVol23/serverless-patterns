# Cloudwatch customise alarm email

This CDK template shows you how to customise a cloudwatch alarm. A standard approach for monitoring with cloudwatch is to receive email notifications in case an alarm starts. The default cloudwatch alarm notifications don't look really nice and are not very adaptable.

To be able to add a logo and more information like links to cloudwatch, ITSM tools it is a good approach to use HTML templates via SES. In SES you have to verify your destionation and source email first. SNS takes normally care of that for you. But SNS cannot send HTML emails.

If you just want to cusotmize your text it is also possible via SNS only, but it still dosn't look nice ;-)

Important: this application uses various AWS services and there are costs associated with these services after the Free Tier usage - please see the [AWS Pricing page](https://aws.amazon.com/pricing/) for details. You are responsible for any AWS costs incurred. No warranty is implied in this example.

## Requirements

* [Create an AWS account](https://portal.aws.amazon.com/gp/aws/developer/registration/index.html) if you do not already have one and log in. The IAM user that you use must have sufficient permissions to make necessary AWS service calls and manage AWS resources.
* [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html) installed and configured
* [Git Installed](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
* [CDK Installed](https://docs.aws.amazon.com/cdk/latest/guide/getting_started.html)


## Deployment Instructions

1. Create a new directory, navigate to that directory in a terminal and clone the GitHub repository:
    ``` 
    git clone https://github.com/aws-samples/serverless-patterns
    ```
2. Change directory to the pattern directory:
    ```
    cd cloudwatch-customise-alarm-email/src/cloudwatch-customise-alarm-email
    ```

3. Define source and destination email address in your environment variable:

```bash
export DESTINATION_EMAIL=destination@gmail.com
export SOURCE_EMAIL=src@gmail.com
```

4. Verify these email adresses in the AWS console in SES.
5. `npm install`: To install all modules
6. `cdk bootstrap`: Prepare S3 bucket and upload lambda code
7. `cdk deploy`: To deploy all of your changes


## How it works

When a cloudwatch alarm goes into the state **Alarm** it triggers an `SNS` event. This event can be subscribed by different consumers. In our case it is subscribed by a `lambda` function. The lambda function takes an event, parses it arguments and sends an email via `SES`. 

The lambda fills an HTML template and sends the HTML file via email to the destination email which was configured in the environment variable `DESTINATION_EMAIL`. 

## Testing

The whole process can be tested by setting an alarm into the state ALARM and check if an email arrives. 

1. Note down the name of the cloudwatch alarm after `cdk deploy` or note it down from the cloudwatch console.
2. `aws cloudwatch set-alarm-state --alarm-name ALARM_NAME --state-value ALARM --state-reason "Testing" `
3. Check if the email arrives
![https://github.com/AlessandroVol23/cloudwatch-custom-email-cdk/blob/main/final.png]


If the email arrived, everything worked as expected. If not check if your email address is verified. If yes, check the lambda logs.




## Cleanup

```bash
cdk destroy
```
