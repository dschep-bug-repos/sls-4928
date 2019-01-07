# https://github.com/serverless/serverless/issues/4928

```
[:tmp] master $ sls create -t aws-nodejs -n sls-4928 -p sls-4928
[:tmp] master $ cd sls-4928
[:sls-4928] master $ sls deploy
Serverless: WARNING: Missing "tenant" and "app" properties in serverless.yml. Without these properties, you can not publish the service
to the Serverless Platform.
Serverless: Packaging service...
Serverless: Excluding development dependencies...
Serverless: Creating Stack...
Serverless: Checking Stack create progress...
.....
Serverless: Stack create finished...
Serverless: Uploading CloudFormation file to S3...
Serverless: Uploading artifacts...
Serverless: Uploading service .zip file to S3 (387 B)...
Serverless: Validating template...
Serverless: Updating Stack...
Serverless: Checking Stack update progress...
...............
Serverless: Stack update finished...
Service Information
service: sls-4928
stage: dev
region: us-east-1
stack: sls-4928-dev
api keys:
  None
endpoints:
  None
functions:
  hello: sls-4928-dev-hello
layers:
  None
[:sls-4928] master 1 $ vim serverless.yml # edit to add event & api key!
[:sls-4928] master 1 $ git diff
diff --git a/serverless.yml b/serverless.yml
index ee1c332..5c18f3f 100644
--- a/serverless.yml
+++ b/serverless.yml
@@ -20,6 +20,8 @@ service: sls-4928 # NOTE: update this with your service name
 provider:
   name: aws
   runtime: nodejs8.10
+  apiKeys:
+    - testApiKey

 # you can overwrite defaults here
 #  stage: dev
@@ -61,10 +63,10 @@ functions:
 #    The following are a few example events you can configure
 #    NOTE: Please make sure to change your handler code to work with those events
 #    Check the event documentation for details
-#    events:
-#      - http:
-#          path: users/create
-#          method: get
+    events:
+      - http:
+          path: users/create
+          method: get
 #      - s3: ${env:BUCKET}
 #      - schedule: rate(10 minutes)
 #      - sns: greeter-topic
[:sls-4928] master $ sls deploy
Serverless: WARNING: Missing "tenant" and "app" properties in serverless.yml. Without these properties, you can not publish the service
to the Serverless Platform.
Serverless: Packaging service...
Serverless: Excluding development dependencies...
Serverless: Uploading CloudFormation file to S3...
Serverless: Uploading artifacts...
Serverless: Uploading service .zip file to S3 (387 B)...
Serverless: Validating template...
Serverless: Updating Stack...
Serverless: Checking Stack update progress...
................................
Serverless: Stack update finished...
Service Information
service: sls-4928
stage: dev
region: us-east-1
stack: sls-4928-dev
api keys:
  testApiKey: IytTSzuz0q8dsicm2BWoX8YoaObucYTy5DstMRRJ
endpoints:
  GET - https://d7vn4s9swf.execute-api.us-east-1.amazonaws.com/dev/users/create
functions:
  hello: sls-4928-dev-hello
layers:
  None
```
