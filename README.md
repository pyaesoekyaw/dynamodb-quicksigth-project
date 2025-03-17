
# Visualizing DynamoDB Data with AWS QuickSight

This project demonstrates how to visualize data stored in an Amazon DynamoDB table using AWS QuickSight. The workflow involves creating a DynamoDB table, setting up an Athena data source (which automatically creates Lambda and AWS Glue resources), querying the data using Athena, and visualizing it in QuickSight.

---

## **Architecture**
![Architecture Diagram](./screenshots/architecture_diagram.png)

---

## **Steps**

### **Step 1: Create a DynamoDB Table and Add Sample Items**
1. **Create a DynamoDB Table**:
   - Go to the [DynamoDB Console](https://console.aws.amazon.com/dynamodb).
   - Click **Create Table**.
   - Enter a table name (e.g., `OnlineCourses`).
   - Define the **primary key** (e.g., `course_id` as the partition key).
   - Click **Create**.

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

### **Step 4: Visualize Data in QuickSight**
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
