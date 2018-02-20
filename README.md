### Guide to set up image resizing application on AWS

#### step 1
Login to you AWS account and change your Region to Sinagpore
#### step 2
Go to https://console.aws.amazon.com/iam/home#/roles and create a role
 1. Click on **Create role** button
 2. Select **Lambda** and then click on **Next: Permissions** button 
 3. Type **AmazonS3FullAccess** in search box you will see 1 item that is AmazonS3FullAccess, click on checkbox
 4. Again type **CloudWatchLogsFullAccess**  on same search box and you will see 1 item that is CloudWatchLogsFullAccess, click on  checkbox and then click on **Next: Review** button
 5. Type **image-resize-lambda-role** in Role name box or you may choose whatever you like name. This will be used while creating Lambda function
#### step 3

Create S3 bucket where we will save images, Go to https://s3.console.aws.amazon.com/s3/home

 1. Click on **Create bucket** button
 2. Type bucket name **image-resizing** or whatever you like
 3. Choose Region **Asia Pacific(Singapore)** or whatever you like but if you choose another then you must choose that region for all services bellow
 4. Click on Create button
### step 4
Create Lambda function, Go to https://ap-southeast-1.console.aws.amazon.com/lambda/home?region=ap-southeast-1#/functions 
 5. Click on Create function
 6. Type Name **image-resizing** or whatever you like and then select Runtime **Node.js 6.10** and then select **Choose an existing role** in Role and then select role created in step 1 that is **image-resize-lambda-role** and then click on Create function button.
 7. Select **Upload a .zip file** in Code entry type(inside Function code) and then select Archive.zip file send by me and then scroll down up to **Environment variables** and enter **BUCKET_NAME** in key and  **image-resizing** (bucket name created in 2nd step) in value  and then  scroll down up to  **Basic settings**, and then choose Memory(MB) 1536 MB by scrolling right and then make Timeout 0 min, 30 sec(default is 0 min, 3 sec) and then again scroll up to top of the page and then click on **save** button( may take some time because zip file is around 25 mb)
### step 5
Create API Gateway proxy, Go to https://ap-southeast-1.console.aws.amazon.com/apigateway/home?region=ap-southeast-1#/apis/create 
 1. Select **New API** option   and then api name **image-resize** or whatever you like and then select **Edge Optimise** in Endpoint Type and then click on **Create API** button
 2. Select **Create Resource** in **Actions** dropdown and then put Resource Name **app** and then Resource Path **{app}** and then click on **Create Resource** button
 3. Click on **/{app}** and then Select **Create Resource** in **Actions** dropdown and then put Resource Name **image** and then Resource Path **{image}** and then click on **Create Resource** button
 4. Click on **/{image}** and then Select **Create Method** in **Actions** dropdown and then select GET option bellow /{image} and save by clicking on icon 
 5. Now select  Lambda Function  in Integration type and mark checked on Use Lambda Proxy integration and select ap-southeast-1 in Lambda Region(assuming that you have created Lambda function in Singapore region) and and then select Lambda function name created in step 4 that image-resizing and then click on Save button and then Ok in popup
 6.  Click on GET(bellow /{image} and then Select **Deploy API** in **Actions** dropdown and then select **[New Stage]** in Deployment stage and **live** in Stage name and then click on Deploy button, you will see invoke url, copy that url( Url may something like https://ec4djaejm8.execute-api.ap-southeast-1.amazonaws.com/live )


### We have done set up, let's test

Create a folder **app1** in S3 bucket created in step 2 that is **image-resizing** and then upload a pic in that folder(lets say name is abc.png) and give permission as public now you can access

https://ec4djaejm8.execute-api.ap-southeast-1.amazonaws.com/live/app1/abc.png
https://ec4djaejm8.execute-api.ap-southeast-1.amazonaws.com/live/app1/abc.m.png
https://ec4djaejm8.execute-api.ap-southeast-1.amazonaws.com/live/app1/abc.s.png

etc.

where https://ec4djaejm8.execute-api.ap-southeast-1.amazonaws.com/live is api gateway url in last step
