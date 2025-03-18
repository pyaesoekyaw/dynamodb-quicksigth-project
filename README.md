
# Visualizing DynamoDB Data with AWS QuickSight

This project demonstrates how to visualize data stored in an Amazon DynamoDB table using AWS QuickSight. The workflow involves creating a DynamoDB table, setting up an Athena data source (which automatically creates Lambda and AWS Glue resources), querying the data using Athena, and visualizing it in QuickSight.

---

### **Architecture**
![Architecture Diagram](https://github.com/pyaesoekyaw/dynamodb-quicksigth-project/blob/main/screenshots/Architecture_Diagram.png)
---
## **Steps**
### **Step 1: Create a DynamoDB Table and Add Sample Items**
1. **Create a DynamoDB Table**:
   - Go to the [DynamoDB Console](https://console.aws.amazon.com/dynamodb).
   - Click **Create Table**.
   - Enter a table name (e.g., `OnlineCourses`).
   - Define the **primary key** (e.g., `course_id` as the partition key).
   - Click **Create**.<br>
**Noted** - When creating a DynamoDB table, it's important to avoid using hyphens ("-") in the table name if you plan to query it using Amazon Athena. This is because Athena uses a MySQL-like query language (Presto/Trino SQL) which interprets hyphens as minus operators, leading to syntax errors. For example, a table named "my-table" would be misinterpreted as "my minus table" in a query, causing it to fail. To ensure compatibility, use underscores (_) or camelCase naming conventions instead. This avoids conflicts with SQL syntax and ensures smooth query execution in Athena.

2. **Add Sample Items**:
   - Open the newly created table and click **Explore Table Items**.
   - Click **Create Item** and add sample items in JSON format. For example:
     ```json
     {
       "course_id": {"N": "1"},
       "course_name": {"S": "Python for Beginners"},
       "instructor": {"S": "John Doe"},
       "platform": {"S": "Udemy"},
       "category": {"S": "Programming"},
       "duration_hours": {"N": "12.5"},
       "price": {"N": "19.99"},
       "rating": {"N": "4.6"},
       "enrollment_count": {"N": "15000"},
       "release_date": {"S": "2023-01-15"}
     }
     ```
   - Repeat this step to add more items.

---

### **Step 2: Set Up Athena Data Source for DynamoDB**
1. **Create Athena Data Source**:
   - Go to the [Athena Console](https://console.aws.amazon.com/athena).
   - Click **Connect Data Source**.
   - Select **DynamoDB** as the data source.
   - Provide the name of your DynamoDB table (e.g., `OnlineCourses`).
   - AWS will automatically create a **Lambda function** and an **AWS Glue table** to map the DynamoDB schema.

2. **Verify AWS Glue Table**:
   - Go to the [AWS Glue Console](https://console.aws.amazon.com/glue).
   - Check the **Tables** section to verify that a table for your DynamoDB data has been created.
   - Ensure the schema matches your DynamoDB table structure.

3. **Verify Lambda Function**:
   - Go to the [Lambda Console](https://console.aws.amazon.com/lambda).
   - Verify that the function created by Athena is configured correctly.
   - Ensure the Lambda function has the necessary permissions to access DynamoDB and Athena.

---

### **Step 3: Query DynamoDB Data Using Athena**
1. **Run Queries in Athena**:
   - Go back to the [Athena Console](https://console.aws.amazon.com/athena).
   - Write SQL queries to retrieve data from the DynamoDB table. For example:
     ```sql
     SELECT * FROM OnlineCourses WHERE category = 'Programming';
     ```
   - Validate the data and ensure it’s ready for visualization.

2. **Permissions**:
   - Ensure the IAM role used by Athena has permissions to access DynamoDB and the Glue Data Catalog.

---
### Step 4: Navigate to Amazon QuickSight
1. In the search bar at the top, type **"QuickSight"**.
2. Click on **Amazon QuickSight** from the search results to open the QuickSight dashboard.

#### Subscribe to Amazon QuickSight
1. If this is your first time using QuickSight, click on the **"Get Started"** or **"Sign up for QuickSight"** button.
2. Choose your **QuickSight edition**:
   - **Standard Edition**: Free for the first 30 days, then pay-as-you-go.
   - **Enterprise Edition**: Includes additional features like encryption, VPC support, and more.
3. Select your **region** where you want QuickSight to be hosted.
4. Choose your **authentication method**:
   - **IAM (Identity and Access Management)**: Use AWS IAM for user access.
   - **QuickSight users and groups**: Manage users directly within QuickSight.
5. Configure **storage**:
   - Choose whether to enable **Auto-discovery of SPICE datasets** (SPICE is QuickSight’s super-fast, in-memory calculation engine).
6. Review the pricing details and click **Subscribe**.

#### Set Up QuickSight Account
1. After subscribing, you’ll be redirected to the QuickSight setup page.
2. Provide the following details:
   - **Account name**: A name for your QuickSight account.
   - **Notification email address**: Email for notifications and alerts.
3. Configure **data sources** (optional at this stage):
   - You can connect QuickSight to AWS data sources (e.g., S3, RDS, Redshift) or third-party sources (e.g., Salesforce, Jira).
4. Click **Finish** to complete the setup.

#### Access QuickSight Dashboard
1. Once the setup is complete, you’ll be redirected to the **QuickSight dashboard**.
2. From here, you can:
   - Create datasets by connecting to your data sources.
   - Build visualizations and dashboards.
   - Share reports with other users.
### **Step 5: Visualize Data in QuickSight**
1. **Connect QuickSight to Athena**:
   - Go to the [QuickSight Console](https://console.aws.amazon.com/quicksight).
   - Click **New Dataset** and select **Athena** as the data source.
   - Choose the table created by AWS Glue (e.g., `OnlineCourses`).

2. **Set Up Permissions**:
   - Ensure QuickSight has permissions to access Athena and the underlying DynamoDB table.
   - Attach the necessary IAM policies to the QuickSight role.

3. **Create Visualizations**:
   - Use QuickSight to create visualizations (e.g., bar charts, pie charts, dashboards) based on the DynamoDB data.

---

## **Screenshots**
- DynamoDB Table: ![DynamoDB Table](./screenshots/dynamodb_table.png)
- Athena Data Source Setup: ![Athena Data Source](./screenshots/athena_data_source.png)
- AWS Glue Table: ![AWS Glue Table](./screenshots/glue_table.png)
- Athena Query: ![Athena Query](./screenshots/athena_query.png)
- QuickSight Dashboard: ![QuickSight Dashboard](./screenshots/quicksight_dashboard.png)

---

## **IAM Policies**
- Athena DynamoDB Policy: [athena_dynamodb_policy.json](./iam_policies/athena_dynamodb_policy.json)
- QuickSight Policy: [quicksight_policy.json](./iam_policies/quicksight_policy.json)

---

## **Repository Structure**
