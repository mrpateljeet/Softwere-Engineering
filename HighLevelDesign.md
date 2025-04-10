# High-Level Design Document

## Overview
Budget Minder is a web-based application designed to help users manage and forecast their monthly budgets. The application allows users to set savings goals, forecast the time required to achieve these goals, and ensures robust data security. The project consists of a React frontend and a Node.js/Express backend, with MongoDB for database management.

## Table of Contents
1. [Introduction](#introduction)
2. [System Requirements](#system-requirements)
3. [Architecture](#architecture)
4. [Components](#components)
5. [Detailed Component Descriptions](#detailed-component-descriptions)
6. [Architecture Diagram](#architecture-diagram)
7. [Activity Diagram](#activity-diagram)
8. [Sequence Diagram](#sequence-diagram)
9. [Usecase Diagram](#usecase-diagram)

## Introduction
Budget Minder is designed to provide users with tools to track their income, expenses, and savings goals. The system offers functionalities such as user registration, login, transaction management, and budget forecasting. The project leverages various open-source libraries and frameworks to deliver a seamless and secure user experience.

## System Requirements
- **Operating System**: Windows, macOS, Linux
- **Node.js**: >= 14.x
- **npm**: >= 6.x
- **Database**: MongoDB
- **Browser**: Latest versions of Chrome, Firefox, Safari, or Edge

## Architecture
The architecture of Budget Minder follows a typical Model-View-Controller (MVC) pattern:
- **Model**: Defines the data structure and database interaction.
- **View**: The user interface developed with React.
- **Controller**: Manages the application's logic and user input, developed with Express.

## Components
1. **Frontend (React)**:
   - User Interface for interacting with the application.
   - Components for registration, login, transaction management, and budget forecasting.
   
2. **Backend (Node.js/Express)**:
   - RESTful API to handle client requests.
   - Middleware for authentication and authorization.
   - Controllers for managing business logic.
   
3. **Database (MongoDB)**:
   - Stores user data, transactions, and categories.
   - Ensures data integrity and security.

## Detailed Component Descriptions

### Frontend (React)
- **Components**: 
  - **RegistrationForm**: Handles user registration.
  - **LoginForm**: Manages user login.
  - **Dashboard**: Displays user transactions and budget overview.
  - **TransactionList**: Shows a list of transactions.
  - **AddTransactionForm**: Form to add new transactions.
  - **GoalForm**: Allows users to set savings goals.
  - **ForecastForm**: Displays budget forecast based on user data.
  - **Chart**: Renders a line chart using Chart.js and react-chartjs-2.
  - **CompleteProfile**: Allows users to complete their profile by entering details.
  - **FeedbackForm**: Allows user to submit feedback.
  - **HomePage**: The main page for the Budget Minder application
  - **ParentComponent**: Sends the forecast data to the backend API and returns the response.
  - **ProfilePage**-: allows users to view and edit their profile information.
  - **TransactionForm**-:This component provides a form for adding or updating transactions.
- **State Management**: Uses React's Context API or Redux for managing global state.
- **Routing**: Utilizes React Router for navigation between different components.

### Backend (Node.js/Express)
- **Authentication Middleware**: Uses JWT for securing API endpoints.
- **Routes**:
  - **/api/register**: Endpoint for user registration.
  - **/api/login**: Endpoint for user login.
  - **/api/user**: Endpoint for fetching and updating user details.
  - **/api/transactions**: Endpoints for CRUD operations on transactions.
  - **/api/categories**: Endpoint for fetching transaction categories.
  - **/api/forecast**: Endpoint for saving, retrieving, and updating forecasts.
  - **api/feedback**: Endpoint for submitting and retrieving feedback.
  - **api/goal**: Endpoint for adding goals, forecasting goals, and retrieving goals.

- **Controllers**:
  - **authController**: Manages user authentication (registration, login).
  - **userController**: Handles user data operations (fetch, update profile).
  - **transactionController**: Manages transaction-related operations (add, edit, delete, fetch).
  - **categoryController**: Handles category data operations (fetch categories).
  - **feedbackController**: handling feedback submissions and retrieval.
  - **forecastController**: handling forecast operations.
  - **goalController**: handling goal operations

- **Models**:
  - **User**: Defines user data schema (username, email, password, etc.).
  - **Transaction**: Defines transaction data schema (userId, categoryId, amount, date, description).
  - **Category**: Defines category data schema (name, type).
  - **Goal**: Defines the schema and model for financial goals set by users.
  - **Forecast**: Defines the schema and model for forecasting financial goals.
  - **Feedback**: Defines the schema and model for feedback submitted by users.

### Database 
- **Schema Definitions**:
  - **User**: Stores user information and credentials.
  - **Transaction**: Records financial transactions.
  - **Category**: Defines income and expense categories.
  - **Goal**: stores financial goals set by users.
  - **Forecast**: stores forecasting financial goals.
  - **Feedback**: stores feedback submitted by users.
- **Relationships**:
  - **User has many Transactions**: Each user can have multiple transactions.
  - **Transaction belongs to User**: Each transaction is linked to a specific user.
  - **Category has many Transactions**: Each category can have multiple transactions.
  - **Transaction belongs to Category**: Each transaction is linked to a specific category.

## Architecture Diagram
```mermaid
graph TD;
    subgraph Frontend
        A[User Interface]
        A --> B[React Components]
        B --> C[API Calls]
    end

    subgraph Backend
        D[API Gateway]
        D --> E[Authentication Service]
        D --> F[User Service]
        D --> G[Transaction Service]
        D --> H[Category Service]
        D --> I[Forecasting Service]
    end

    subgraph Database
        J[ Database]
        J --> F
        J --> G
        J --> H
    end

   

    A --> C
    C --> D
    C --> J
```
## Activity Diagram

```mermaid
graph TD
    A[Home Page] --> B[Get Started]
    B --> C[Login]
    B --> D[Register]
    D --> E[Profile Building]
    E --> C
    D --> A
    C --> F[Transaction Page]
    C --> A
    F --> G{Manage Transactions}
    G --> H[Transaction details]
    G --> I[Set savings goals]
    G --> J[Forecast Budget]
    G --> K[Set Frequency]
```

## Sequence Diagram
### Sequence Diagram for Login
```mermaid
sequenceDiagram
    actor User
    User ->> Login: Open Login Page
    Login ->> Server: Send Login Credentials
    Server ->> Database: Verify Credentials
    Database ->> Server: Send Verification Result
    Server ->> Login: Send Authentication Token
    Login ->> User: Redirect to Transaction Page
```
### Sequence Diagram for Registration
```mermaid

sequenceDiagram
    actor User
    User ->> Register: Open Registration Page
    Register ->> Server: Send Registration Details
    Server ->> Database: Store User Details
    Database ->> Server: Confirmation
    Server ->> Register: Registration Successful
    Register ->> User: Redirect to Profile Building
    User ->> Profile_Building: Complete Profile
    Profile_Building ->> Server: Send Profile Details
    Server ->> Database: Store Profile Details
    Database ->> Server: Confirmation
    Server ->> Profile_Building: Profile Building Successful
    Profile_Building ->> User: Redirect to Login
```
### Sequence Diagram for Transaction Management
```mermaid
sequenceDiagram
    actor User
    User ->> Transaction_Page: Open Transaction Page
    Transaction_Page ->> UI: Enter Transaction Details
    UI ->> Transaction_Page: Submit Transaction
    Transaction_Page ->> Server: Send Transaction Data
    Server ->> Database: Store Transaction Data
    Database ->> Server: Confirmation
    Server ->> Transaction_Page: Transaction Successful
    Transaction_Page ->> UI: Set Frequency
    
    UI ->> Transaction_Page: Submit Frequency
    Transaction_Page ->> Server: Send Frequency Data
    Server ->> Database: Store Frequency Data
    Database ->> Server: Confirmation
    Server ->> Transaction_Page: Frequency Set Successful
```
### Sequence Diagram for Forecasting Flow with Savings Goals 
```mermaid
sequenceDiagram
    actor User
    User ->> UI: Set Savings Goal
    UI ->> Server: Send Savings Goal Data
    Server ->> Database: Insert Savings Goal Data
    Database -->> Server: Confirm Insertion
    Server -->> UI: Confirm Savings Goal Set
    UI -->> User: Display Confirmation

    User ->> UI: Request Forecast Data
    UI ->> Server: Send Forecast Request
    Server ->> Database: Retrieve Transaction and Frequency Data
    Database -->> Server: Return Data
    Server ->> External_Forecasting_API: Request Forecast Calculation
    External_Forecasting_API -->> Server: Return Forecast Data
    Server -->> UI: Return Forecast Data
    UI -->> User: Display Forecast Result
```
### Usecase Diagram
## usecase diagram for Login
```mermaid
%%{init: {'theme': 'base', 'themeVariables': {'primaryColor': '#f5f7fa', 'secondaryColor': '#4f7cac', 'tertiaryColor': '#f4d35e', 'fontFamily': 'Arial'}}}%%
graph TB
    %% Define Nodes (Use Cases and Actions)
    A["Enter Email and Password"]
    B["Submit Login Form"]
    C{"Validate Email Format"}
    D["Sanitize Inputs"]
    E["Send Login Data to Backend"]
    F{"Process Login Response"}
    G["Navigate to Dashboard"]
    H["Display Error Message"]

    %% Define Flow
    A --> B
    B --> C
    C -->|Yes| D
    C -->|No| H
    D --> E
    E --> F
    F -->|Success| G
    F -->|Failure| H
```
## usecase diagram for Registration
```mermaid
%%{init: {'theme': 'base', 'themeVariables': {'primaryColor': '#f5f7fa', 'secondaryColor': '#4f7cac', 'tertiaryColor': '#f4d35e', 'fontFamily': 'Arial'}}}%%
graph TB
    %% Define Nodes (Use Cases and Actions)
    A["Enter Username, Email, and Password"]
    B["Submit Registration Form"]
    C{"Validate Email Format"}
    D["Sanitize Inputs"]
    E["Send Registration Data to Backend"]
    F{"Process Registration Response"}
    G["Navigate to Complete Profile"]
    H["Display Error Message"]

    %% Define Flow
    A --> B
    B --> C
    C -->|Yes| D
    C -->|No| H
    D --> E
    E --> F
    F -->|Success| G
    F -->|Failure| H
```
## usecase diagram for Budget Forecast
```mermaid
%%{init: {'theme': 'base', 'themeVariables': {'primaryColor': '#f5f7fa', 'secondaryColor': '#4f7cac', 'tertiaryColor': '#f4d35e', 'fontFamily': 'Arial'}}}%%
graph TB
    %% Define Nodes (Use Cases and Actions)
    A["Fetch User Data"]
    B["Submit Forecast Form"]
    C{"User Confirms to Save Forecast?"}
    D["Save Forecast Data"]
    E{"Was the Request Successful?"}
    F["Navigate to Add Goal Page"]
    G["Display Forecast Result"]
    
    

    %% Define Flow
    A --> B
    B --> C
    C -->|Yes| D
    C -->|No| G
    D --> E
    E -->|Success| F
    E -->|Failure| G
    
   
    G -->|Display Forecast Result| B
```
## usecase diagram for adding income and Expense
```mermaid
%%{init: {'theme': 'base', 'themeVariables': {'primaryColor': '#f5f7fa', 'secondaryColor': '#4f7cac', 'tertiaryColor': '#f4d35e', 'fontFamily': 'Arial'}}}%%
graph TB
    %% Define Nodes (Use Cases and Actions)
    A["Load Transaction Page"]
    B["Enter Amount, Date, Description, and choose a category"]
    C["Submit Transaction Form"]
    
    E["Send Transaction Data to Backend"]
    F{"Was the Request Successful?"}
    H["Navigate to Dashboard"]
    I["Display Error Message"]
   

    %% Define Flow
    A --> B
    B --> C
    C --> E
  
    
    E -->|Yes| F
    E -->|No| I
   
    F-->|Success| H
    F -->|Failure| I
```

 
