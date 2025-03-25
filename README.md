# AWS Lambda with API Gateway, Versioning, and Aliases

This project demonstrates how to deploy an AWS Lambda function with API Gateway, implement versioning, and use aliases to manage different environments (Dev, Pre-Prod, Prod) with distinct endpoints.

---

## Prerequisites
- AWS account
- AWS CLI installed and configured
- Python 3.x
- Basic knowledge of AWS Lambda and API Gateway

---

## Step 1: Create a Lambda Function
1. Go to **AWS Lambda** in the AWS Console.
2. Click **Create Function**.
3. Choose **Author from scratch**.
4. Enter **Function name**: `myLambdaFunction`.
5. Select **Runtime**: `Python 3.x`.
6. Select **Execution Role**: Create a new role with basic Lambda permissions.
7. Click **Create Function**.

---

## Step 2: Write the Lambda Function Code
1. Scroll down to the **Code source** section.
2. Replace the existing code with:
   ```python
   import json
   
   def lambda_handler(event, context):
       return {
           'statusCode': 200,
           'body': json.dumps({'message': 'Hello from Ritik! This is the initial version.'})
       }
   ```
3. Click **Deploy**.

---

## Step 3: Create an API Gateway
1. Go to **API Gateway** in the AWS Console.
2. Click **Create API**.
3. Select **HTTP API**.
4. Click **Build**.
5. Enter **API name**: `MyAPI`.
6. Click **Next**.
7. Choose **Lambda integration** and select `myLambdaFunction`.
8. Click **Next** → **Create**.

---

## Step 4: Deploy API Gateway
1. In API Gateway, go to **Deployments**.
2. Click **Create Stage**.
3. Enter **Stage name**: `Dev`.
4. Click **Deploy**.
5. Note the **Invoke URL** (e.g., `https://xyz.execute-api.us-east-1.amazonaws.com/Dev`).

---

## Step 5: Test API Gateway
1. Open a browser or use Postman.
2. Enter the **Invoke URL** in this format:
   ```
   https://xyz.execute-api.us-east-1.amazonaws.com/Dev
   ```
3. You should receive:
   ```json
   { "message": "Hello from Ritik! This is the initial version." }
   ```

---

## Step 6: Enable CORS (Optional but Recommended)
1. In API Gateway, go to **Resources**.
2. Select **ANY** method.
3. Click **Enable CORS**.
4. Deploy the API again.

---

## Step 7: Create a New Lambda Version
1. Go to **AWS Lambda** → `myLambdaFunction`.
2. Click **Actions** → **Publish new version**.
3. Enter a **Version description**: `Updated message`.
4. Click **Publish**.

---

## Step 8: Create an Alias for Different Environments
1. Go to **AWS Lambda** → `myLambdaFunction`.
2. Click **Aliases** → **Create alias**.
3. Enter **Alias name**: `Prod`.
4. Select the newly created **Version**.
5. Click **Create**.
6. Repeat the process to create `Pre-Prod` and `Dev` aliases.

---

## Step 9: Update API Gateway to Use Alias
1. Go to **API Gateway** → **MyAPI**.
2. Select the **ANY** method.
3. Change the **Integration request** to use `myLambdaFunction:Prod`.
4. Deploy the API again.

---

## Step 10: Test API with New Version
1. Call the API again:
   ```sh
   curl -X GET "https://xyz.execute-api.us-east-1.amazonaws.com/Dev"
   ```
2. Verify that the response now shows the updated message from the new Lambda version.

---

## Step 11: (Optional) Roll Back to a Previous Version
1. Go to **AWS Lambda** → `myLambdaFunction`.
2. Click **Aliases** → `Prod`.
3. Change the **Version** to the previous one.
4. Click **Save**.
5. Test the API again to confirm the rollback.

---

## Conclusion
You have successfully set up an AWS Lambda function with API Gateway, implemented versioning, and used aliases for different environments. 🚀

---

## Troubleshooting
### Issue: `Missing Authentication Token`
- Ensure you are using the correct HTTP method.
- Recreate API Gateway methods if necessary.
- Check API Gateway logs in **CloudWatch**.
- Test Lambda manually using:
  ```sh
  aws lambda invoke --function-name myLambdaFunction --payload '{}' response.json
  ```

### Issue: API Not Reflecting Changes
- Re-deploy API Gateway.
- Check if the alias is pointing to the correct Lambda version.

Enjoy coding! 🎉

