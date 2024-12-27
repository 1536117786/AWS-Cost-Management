AWS Cost Management Dashboard
=============================

Overview
--------

The **AWS Cost Management Dashboard** is a comprehensive tool designed to help users monitor, analyze, and optimize their AWS cloud spending. It combines real-time notifications, intelligent insights, and forecasting capabilities into an intuitive web application. The app leverages AWS services like Cost Explorer, CloudWatch, and SNS, along with modern frontend and backend technologies, to provide a seamless user experience.

* * * * *

Features
--------

### 1\. **Real-Time Cost Alerts**

-   **Purpose**: Notify users when their spending exceeds defined thresholds or when unexpected spikes occur.
-   **Implementation**:
    -   Uses **AWS CloudWatch Alarms** to monitor the **EstimatedCharges** metric in the AWS Billing namespace.
    -   Sends alerts via **Amazon SNS** to subscribed email addresses or phone numbers.
-   **Example Use Case**:
    -   You set an alert for $100. If your AWS spending exceeds this amount, you receive an immediate notification.

* * * * *

### 2\. **Cost Optimization Suggestions**

-   **Purpose**: Provide actionable recommendations to reduce AWS costs.
-   **Implementation**:
    -   Queries **AWS Trusted Advisor** for cost optimization recommendations.
    -   Common suggestions include resizing underutilized EC2 instances, deleting idle resources, or using Reserved Instances.
-   **Example Use Case**:
    -   You run multiple EC2 instances, but Trusted Advisor detects that one instance has been idle for weeks. The app suggests terminating it to save costs.

* * * * *

### 3\. **Cost Forecasting**

-   **Purpose**: Predict future AWS costs based on historical usage data.
-   **Implementation**:
    -   Fetches historical cost data from **AWS Cost Explorer**.
    -   Uses **Linear Regression** with `scikit-learn` to analyze trends and project future costs.
-   **Example Use Case**:
    -   If your costs have been increasing by $50 monthly, the app forecasts spending for the next three months.

* * * * *

### 4\. **User-Friendly Dashboard**

-   **Purpose**: Display all cost-related insights in an interactive, visual format.
-   **Implementation**:
    -   Built with **React** and **Chart.js**.
    -   Includes multiple widgets in a 2x2 grid layout:
        -   Historical spending trends.
        -   Cost breakdown by service.
        -   Forecasted costs.
        -   Customizable placeholder widgets for additional insights.

* * * * *

Project Architecture
--------------------

### 1\. **Frontend**

-   **Framework**: React
-   **Key Components**:
    -   **DashboardPage.js**: Displays cost trends, breakdowns, and forecasts in a 2x2 grid.
    -   **AlertsPage.js**: Allows users to create and view spending alerts.
    -   **SuggestionsPage.js**: Displays cost-saving suggestions.
    -   **Navbar.js**: Provides navigation across different sections of the app.
-   **Libraries**:
    -   `chart.js`: For visualizing cost data.
    -   `axios`: For making API calls to the backend.

* * * * *

### 2\. **Backend**

-   **Framework**: Flask
-   **Endpoints**:
    -   `/alerts/create`: Create a new CloudWatch alarm.
    -   `/suggestions/billing`: Retrieve Trusted Advisor suggestions.
    -   `/forecast`: Fetch historical spending and predict future costs.
    -   `/dashboard-data`: Aggregate data for the dashboard (cost trends, breakdowns, and forecasts).
-   **Technologies**:
    -   `boto3`: AWS SDK for Python, used to interact with AWS services.
    -   `scikit-learn`: Machine learning library for forecasting.
    -   `numpy`: Numerical computations for cost analysis.

* * * * *

### 3\. **AWS Services**

The app integrates with the following AWS services:

1.  **Cost Explorer**: Fetches historical and forecasted AWS costs.
2.  **CloudWatch**: Monitors the **EstimatedCharges** metric and triggers alarms.
3.  **SNS (Simple Notification Service)**: Sends notifications to users when alarms are triggered.
4.  **Trusted Advisor**: Provides cost optimization recommendations.

* * * * *

How It Works
------------

### 1\. **Real-Time Cost Alerts**

1.  User creates an alert through the Alerts page.
2.  The app calls `/alerts/create`, passing the threshold and alarm name.
3.  Backend creates a CloudWatch alarm that monitors **AWS/Billing -> EstimatedCharges**.
4.  If the threshold is breached, CloudWatch triggers the SNS topic to send an email or SMS notification.

* * * * *

### 2\. **Cost Optimization Suggestions**

1.  The user navigates to the Suggestions page.
2.  The app calls `/suggestions/billing` to retrieve recommendations from AWS Trusted Advisor.
3.  Backend fetches suggestions, such as resizing instances or deleting idle resources, and displays them in the UI.

* * * * *

### 3\. **Cost Forecasting**

1.  User navigates to the Forecast page.
2.  The app calls `/forecast`.
3.  Backend:
    -   Queries AWS Cost Explorer for the last 6 months of spending data.
    -   Uses Linear Regression to predict the next 3 months of costs.
4.  The frontend displays historical and forecasted costs as a line chart.

* * * * *

### 4\. **User-Friendly Dashboard**

1.  User navigates to the Dashboard page.
2.  The app calls `/dashboard-data` to fetch:
    -   Historical costs.
    -   Cost breakdown by service.
    -   Forecasted costs.
3.  The data is displayed in a 2x2 grid:
    -   Line chart for historical costs.
    -   Pie chart for service breakdown.
    -   Line chart for forecasts.
    -   Placeholder widget for custom insights.

* * * * *

Setup Instructions
------------------

### Backend

1.  **Clone the repository**:

    bash

    Copy code

    `git clone https://github.com/your-repo/aws-cost-management.git
    cd aws-cost-management/backend`

2.  **Install dependencies**:

    bash

    Copy code

    `pip install -r requirements.txt`

3.  **Set up environment variables**: Create a `.env` file with:

    bash

    Copy code

    `AWS_ACCESS_KEY_ID=YourAWSAccessKeyID
    AWS_SECRET_ACCESS_KEY=YourAWSSecretAccessKey
    AWS_REGION=us-east-1
    SNS_TOPIC_ARN=arn:arn:aws:cloudwatch:us-east-1:221082170150:alarm:BillingThresholdAlarm_800`

4.  **Run the backend**:
Put all your credentials in the respective strings at the top of the app.py file as detailed below: 
`AWS_ACCESS_KEY_ID=YourAWSAccessKeyID
    AWS_SECRET_ACCESS_KEY=YourAWSSecretAccessKey
    AWS_REGION=us-east-1
    SNS_TOPIC_ARN=arn:aws:sns:us-east-1:123456789012:CostAlerts`
    bash

    Copy code

    `python app.py`

### Frontend

1.  **Navigate to the frontend folder**:

    bash

    Copy code

    `cd ../frontend`

2.  **Install dependencies**:

    bash

    Copy code

    `npm install`

3.  **Start the frontend server**:

    bash

    Copy code

    `npm start`

* * * * *

Testing the App
---------------

1.  **Create a Cost Alert**:

    -   Navigate to the Alerts page.
    -   Enter an alert name and threshold.
2.  **View Optimization Suggestions**:

    -   Navigate to the Suggestions page.
    -   Review actionable recommendations.
3.  **Analyze Costs**:

    -   Use the Dashboard to view historical spending trends and forecasts.

* * * * *

IAM Policy for the App
----------------------

Attach this custom policy to the IAM user for minimal required permissions:

json

Copy code

`{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ce:GetCostAndUsage",
                "ce:GetCostForecast",
                "ce:GetDimensionValues",
                "sns:Publish",
                "sns:ListTopics",
                "sns:Subscribe",
                "cloudwatch:PutMetricAlarm",
                "aws-portal:ViewBilling",
                "aws-portal:ViewUsage"
            ],
            "Resource": "*"
        }
    ]
}`
