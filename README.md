
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
   - Define the **primary key** (e.g., `course_id` as the partition key).Leave the rest of the configuration.
   - Click **Create**.<br>
   !0
**Noted** - When creating a DynamoDB table, it's important to avoid using hyphens ("-") in the table name if you plan to query it using Amazon Athena. This is because Athena uses a MySQL-like query language (Presto/Trino SQL) which interprets hyphens as minus operators, leading to syntax errors. For example, a table named "my-table" would be misinterpreted as "my minus table" in a query, causing it to fail. To ensure compatibility, use underscores (_) or camelCase naming conventions instead. This avoids conflicts with SQL syntax and ensures smooth query execution in Athena.

2. **Add Sample Items**:
   - Open the newly created table and click **Explore Table Items**.
   !1
   - Click **Create Item** and add sample items in JSON format. For example:
     ```json
   !2
     {
       "course_id": {"S": "1"},
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
   !3
---
1. **Navigate to the S3 Console**:
- On the S3 dashboard, click the **Create Bucket** button.
- **Bucket Name**: `my-general-purpose-bucket-2025`
- **Region**: `us-east-1`
- **Object Ownership**: ACLs disabled
- **Block Public Access**: Enabled
- **Bucket Versioning**: Disabled
- Ensure the bucket name is unique and follows S3 naming conventions.

### **Step 2: Set Up Athena Data Source for DynamoDB**
1. **Create Athena Data Source**:
   - Go to the [Athena Console](https://console.aws.amazon.com/athena).
   - Click **Connect Data Source**.
   - Select **DynamoDB** as the data source.
   - Provide the name of your DynamoDB table (e.g., `OnlineCourses`).
   - AWS will automatically create a **Lambda function** and an **AWS Glue table** to map the DynamoDB schema.
   !4

2. **Verify AWS Glue Table and Lambda Function**:
   !5
   !6
---
### **Step 3: Query DynamoDB Data Using Athena**
1. **Run Queries in Athena**:
   !7
   !8
SELECT * 
FROM OnlineCourses 
WHERE price = 0;
   !10
3. **Permissions**:
   - Ensure the IAM role used by Athena has permissions to access DynamoDB and the Glue Data Catalog.

---
### Step 4: Navigate to Amazon QuickSight
1. In the search bar at the top, type **"QuickSight"**.
2. Click on **Amazon QuickSight** from the search results to open the QuickSight dashboard.
3. Select your **region** where you want QuickSight to be hosted.
4. **Account name**: A name for your QuickSight account.
5. **Notification email address**: Email for notifications and alerts.
6. Configure **data sources** (especially Athena)
7. Uncheck the box 
   !11 
4. Click **Finish** to complete the setup.
1. Once the setup is complete, you’ll be redirected to the **QuickSight dashboard**.
   **Set Up Permissions**:
   - Ensure QuickSight has permissions to access Lambda and S3 with **inline policy**.
   - You will see the IAM role with this name "aws-quicksight-service-role-v0" that automatically created by Athena.
     !12
   - Click **New Dataset** and select **Athena** as the data source.
   - Choose the table created by Athena.(e.g., `DataSet`).
   !13
   - Use QuickSight to create visualizations (e.g., bar charts, pie charts, dashboards) based on the DynamoDB data.
  
  !14
