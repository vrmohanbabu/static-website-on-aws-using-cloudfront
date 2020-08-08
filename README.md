## Project Overview

Static web contents are taken from GitHub and send to the S3 bucket and then deployed using CloudFront Distribution. Everything automated by Jenkins.

---

## Setup the Enviroment

* Environment used is Ubuntu18 in cloud9.
* Jenkins with Blue Ocean Plugin & Pipeline-AWS Plugin.
* AWS account with IAM role created.
* tidy installed.

### Install Jenkins:

* `wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -`
* `sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ >> /etc/apt/sources.list'`
* `sudo apt-get update`
* `wget https://pkg.jenkins.io/debian-stable/binary/jenkins_2.204.6_all.deb`
* `sudo apt install ./jenkins_2.204.6_all.deb -y`
* `sudo systemctl start jenkins`
* `sudo systemctl enable jenkins`
* `sudo systemctl status jenkins`

### Set up AWS credentials in Jenkins:

* Click on the “Credentials” link from the sidebar.
* Click on "(global)" from the list, and then "Add credentials" from the sidebar.
* Choose "AWS Credentials" from the dropdown, add ID, description and fill in the AWS Key and Secret Access Key generated when the IAM role was created.

### Set up S3 Bucket:

* Create S3 Bucket, For "Set Permissions," uncheck the "Block all public access."
* Add the Bucket Policy:
```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::NAME_OF_BUCKET/*"
    }
  ]
}
```
* Replace 'NAME_OF_BUCKET' with the bucket that was just created.

### Creating Jenkins Pipeline:

* In Jenkins, Click "Open Blue Ocean" and click "New Pipeline".
* Select GitHub from the options available, a token needs to be generated. A link to https://github.com/settings/tokens/new?scopes=repo,read:user,user:email,write:repo_hook needs to be clicked to generate a token for Jenkins to use and Make sure you copy the token and save it.
* After pasting the token into the form in Jenkins, click "connect", and your account should show up.
* Next, select the repo that was created, and click "create pipeline."
* In the page where the job shows, there is a gear icon - click on it to edit the job directly. Find the "Scan repository triggers" and click on "Periodically if not otherwise run," and select an interval of 2 minutes.

### Set up CloudFront:

* For "Select a delivery method for your content", click "Get Started".
* From the CloudFront dashboard, click "Create Distribution".
* For "Select a delivery method for your content", click "Get Started".
* Under "Origin Domain Name", select the S3 bucket that you just created.
* In "Viewer Protocol Policy", choose the "Redirect HTTP to HTTPS" option.
* Copy the endpoint URL for your CloudFront distribution found in the "Domain Name" column.
* Paste the copied endpoint URL and append “/index.html” on the end in the browser.



