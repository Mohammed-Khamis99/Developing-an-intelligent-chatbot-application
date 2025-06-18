#Build an Intelligent Chatbot Application
Develop an intelligent chatbot application leveraging the latest advancements in conversational AI and large language models. This project integrates generative AI technologies to create a sophisticated and responsive chatbot experience.

![Image](https://github.com/user-attachments/assets/8b82095c-8dd3-4423-a89d-4d117e64e0d3)

Solution deployment
I start by deploying the application infrastructure through the CloudFormation template.
This crucial decision aims to optimize infrastructure provisioning and significantly reduce deployment times.

![Image](https://github.com/user-attachments/assets/251bc036-5a81-4e57-a50a-a0dc81fe1b53)

##Configure and Deploy the Frontend:
Provision all necessary AWS services. Then, configure your frontend application and deploy the files.
##Configure Frontend Application Files
Navigate to the …/frontend/src/configs folder. There, find two essential configuration files:
•	aws-exports.ts: Configure application authentication using data from the Cognito User Pool. This file requires four configuration variables: 
o	AWS_PROJECT_REGION: Specify the AWS region where you deployed your solution.
o	AWS_COGNITO_REGION: Define your Cognito User Pool region (this matches AWS_PROJECT_REGION).
o	AWS_USER_POOLS_ID: Input the ID of your user pool.
o	AWS_USER_POOLS_WEB_CLIENT_ID: Enter the client ID of your Cognito User Pool application.
•	configs.tsx: Define the API URL for your application. This file contains one configuration variable: 
o	CONFIG_API_URL: Set the API endpoint.
Access these values from the Outputs section of your CloudFormation template. Follow these steps:
1.	Go to the CloudFormation console.
2.	Click on your stack.
3.	Select the Outputs tab from the right menu.
4.	Save the files after updating them.

![Image](https://github.com/user-attachments/assets/dcf16527-1245-4a06-89ec-48c5012d201f)

Your configs.tsx and aws-exports.tsx files should look like this:
configs.tsx
TypeScript
export const API_URL = '[ApiUrl CloudFormation Output]';
aws-exports.tsx
TypeScript
export const amplifyConfig = {
    aws_project_region: 'AWS Region code where you deployed your application. E.g.: us-west-2',
    aws_cognito_region: 'AWS Region code where you deployed your application. E.g.: us-west-2',
    aws_user_pools_id: '[UserPoolId CloudFormation Output]',
    aws_user_pools_web_client_id: '[ClientId CloudFormation Output]',
};
##Build the Frontend Application
After updating the configuration files, initiate the build process to convert the code into a web-optimized bundle. If you use npm as your package manager, install dependencies and start the build process:
Bash
npm install
npm run build
The build process generates a folder (typically build/ or dist/) within the root directory of the frontend application. This folder contains the files for upload to your S3 bucket.
Deploy to S3
Copy the build folder to S3 by following these instructions:
1.	Locate the S3 bucket you created earlier using the CloudFormation template. The bucket's name starts with frontend-chapter-7- followed by a random sequence of characters (e.g., frontend-chapter-7-XXXXX).
2.	Click on the bucket's name to open it.
3.	Within the S3 bucket, locate and click the Upload button to transfer files from your local machine to the S3 bucket.
4.	Click Add Files, navigate to the dist folder on your local machine, select all files inside the folder, and confirm. (You should find index.html and penguin.png at the root of the dist folder.)
5.	Next, click Add folder, locate the dist folder, select the assets folder, and confirm.
6.	Verify the presence of the following files (filenames may differ due to automatic generation during the build process): 
o	index.html
o	assets/index-XXXXXX.css
o	assets/index-XXXXXX.js
o	Any static files (e.g., images) you included in your application
7.	Scroll to the bottom of the page and click Upload.

![Image](https://github.com/user-attachments/assets/9d3a066e-4798-4b34-a31f-60d33fddaf39)

You have now completed the deployment and configuration process for the application. Proceed to the chatbot setup.
________________________________________
##Configure and Build Amazon Lex
Most Amazon Lex configurations are pre-configured by the CloudFormation template. You only need to integrate the Lambda function to perform actions based on the conversation. Finalize the setup by following these steps:
1.	Go to the Amazon Lex console in the region where you deployed the CloudFormation stack.
2.	Select Bots from the left menu.

![Image](https://github.com/user-attachments/assets/740af126-2728-4340-b331-b239e5873f8d)

3.	In the Bots list, select MeetyBot.

![Image](https://github.com/user-attachments/assets/5b95a522-b1fc-467b-aab6-bbbd60f9d38e)

4.	Under the Deployment section, select Aliases and click on TestBotAlias.

![Image](https://github.com/user-attachments/assets/5fb302b2-4172-4668-b269-0cf57499742c)

5.	From the Languages list, select English (US).
6.	In the Lambda function configuration menu, select bot-function-meety as the source and $LATEST as the version.

![Image](https://github.com/user-attachments/assets/d3f93db8-aa21-4a35-8a43-91baa267ed77)

The final step is to configure the triggering of the function when the BookMeeting intent is fulfilled. For that, do the following:
1.	On the left menu, select Intents under the English (US) menu.
2.	Click on the BookMeeting intent.

![Image](https://github.com/user-attachments/assets/091e1af9-0af1-4a7d-ac08-cfd3fea7e168)

3.	Scroll down to the Fulfillment section and click the toggle button to activate it. This ensures the Lambda function triggers every time this intent is fulfilled.

![Image](https://github.com/user-attachments/assets/4a32eede-886f-4c46-b429-5e742da79af9)

4.	Click Save Intent in the bottom right.
5.	Select Build in the top right.
The chatbot build process takes approximately two minutes. Wait until you see a green banner with a success message.

![Image](https://github.com/user-attachments/assets/7152e593-d6dd-4b19-a871-f13b76b2b25e)

Customize or add more utterances to identify the StartMeety intent. After any changes, ensure you save the intent and rebuild the chatbot.

 ![Image](https://github.com/user-attachments/assets/617744cd-3081-4cd8-b605-f70ca1489f06)
 
Perform the same exercise for the BookMeeting intent. Amazon Lex expects different utterances for this intent and includes five slots to collect necessary data:
•	FullName: The name of the attendee.
•	MeetingDate: The desired meeting date.
•	MeetingStime: The start time of the meeting.
•	MeetingDuration: The duration of the meeting (either 30 or 60 minutes).
•	AttendeeEmail: The email for contact.
Explore each slot and the prompt used by the chatbot. Customize prompts and slots as desired.
After familiarizing yourself with the chatbot's configurations, proceed to test its behavior in your application by initiating a conversation to book a meeting.
________________________________________
##Manage Meeting Requests (Admin Portal)
To access the admin portal and manage meeting requests, follow these steps:
1.	From your application’s homepage, click on Sign In.
2.	Fill out the Username and Password fields with the values from the email you received and confirm.
3.	Set up a new password and proceed.
4.	Select your email and click Verify. You will now receive an email with a temporary code.
5.	Copy the verification code from your email and paste it into the confirmation form.

![Image](https://github.com/user-attachments/assets/93db720e-8755-4c06-9e79-76cc9c566aa0)

![Image](https://github.com/user-attachments/assets/90938782-cd2c-46dc-b6a5-98e27507dcbe)

![Image](https://github.com/user-attachments/assets/aee5d2e9-1572-48b8-ab82-c59a2d937f38)
