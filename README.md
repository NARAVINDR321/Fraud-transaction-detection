# Banking Application with ML-Powered Fraud Detection

This project is a web-based banking application built with Python and the Flask framework. It provides core banking functionalities such as user authentication, client management, and financial transactions. A key feature of this application is the integration of an XGBoost machine learning model, likely for predicting fraudulent transactions.

## Table of Contents
- [Project Overview](#project-overview)
- [Features](#features)
- [Tech Stack](#tech-stack)
- [Prerequisites](#prerequisites)
- [Installation and Setup](#installation-and-setup)
- [Database Configuration](#database-configuration)
- [Running the Application](#running-the-application)
- [Application Routes](#application-routes)
- [Machine Learning Integration](#machine-learning-integration)
- [Contributing](#contributing)

## Project Overview
This application serves as a prototype for a modern digital banking platform. It allows bank employees or administrators to manage user accounts, register new clients, and process financial transactions. The system uses a MySQL database to persist data and incorporates a pre-trained XGBoost classifier to analyze transaction data in real-time.

## Features
- **User  Authentication**: Secure user registration and login system with password hashing.
- **Session Management**: Persistent user sessions using Flask-Login.
- **Dashboard**: A central hub for authenticated users to access different features.
- **New Client Registration**: A form to add new clients to the bank's database.
- **Transaction Processing**: A form to initiate financial transfers between accounts.
- **ML-Powered Insights**: (Assumed) An underlying machine learning model (XGBClassifier) for tasks like fraud detection.

## Tech Stack
- **Backend**: Python, Flask
- **Database**: MySQL
- **ORM**: SQLAlchemy
- **Authentication & Security**: Flask-Login, Flask-Bcrypt
- **Forms**: Flask-WTF
- **Machine Learning**: Scikit-learn, XGBoost, NumPy
- **Frontend**: HTML, Jinja2 Templates (presumably with CSS)

## Prerequisites
Before you begin, ensure you have the following installed on your system:
- Python 3.8+
- pip (Python package installer)
- MySQL Server

## Installation and Setup
1. Clone the repository:
   ```bash
   git clone <your-repository-url>
   cd <repository-directory>
   ```

2. Create a virtual environment:
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows, use `venv\Scripts\activate`
   ```

3. Install the required Python packages:
   ```bash
   pip install Flask Flask-SQLAlchemy Flask-Login Flask-WTF Flask-Bcrypt scikit-learn xgboost numpy mysql-connector-python
   ```

### Machine Learning Model
Ensure you have the trained XGBoost model file saved as a .pkl file (e.g., model.pkl) in the root directory of the project. The code snippet suggests a model is being used, but the loading mechanism isn't explicitly defined in the provided app.py.

## Database Configuration
1. Start your MySQL server.
2. Create a new database. You can do this via the MySQL shell or a GUI tool like MySQL Workbench.
   ```sql
   CREATE DATABASE BANK2;
   ```

3. Update the Database URI:
   Open the app.py file and modify the SQLALCHEMY_DATABASE_URI to match your MySQL credentials.
   ```python
   # Format: 'mysql://<user>:<password>@<host>/<database_name>'
   app.config['SQLALCHEMY_DATABASE_URI'] = 'mysql://root:password123@localhost/BANK2'
   ```

4. Initialize the Database:
   The application uses `automap_base` to reflect the existing database schema. This means you need to have the client table (and potentially others) already created in your BANK2 database. The User table, however, will be created by SQLAlchemy based on the model definition.
   Run the following in a Python shell within your activated virtual environment to create the user table:
   ```python
   from app import app, db
   app.app_context().push()
   db.create_all()
   ```

## Running the Application
Once the setup is complete, you can run the Flask development server:
```bash
python app.py
```
The application will be accessible at [http://127.0.0.1:5000](http://127.0.0.1:5000).

## Application Routes
- **GET /**: Home Page - The landing page of the application.
- **GET, POST /register**: Register Page - Allows new users to create an account.
- **GET, POST /login**: Login Page - Authenticates existing users.
- **GET /dashboard**: Dashboard - Main interface after logging in. Requires authentication.
- **GET, POST /new_client**: New Client Form - Page to add a new bank client. Requires authentication.
- **GET, POST /transaction**: Transaction Form - Page to perform a new transaction. Requires authentication.
- **GET /trans_success**: Transaction Success - A confirmation page shown after a successful transaction.
- **GET /logout**: Logout - Logs out the current user and redirects to the login page.

## Machine Learning Integration
The code includes a reference to an XGBClassifier. While the app.py file doesn't show it being actively used to make predictions on new transactions, its presence implies a machine learning pipeline for a task such as Fraud Detection.

### How it likely works:
- **Training**: The XGBClassifier model was trained offline on a historical dataset of transactions, with features like transaction_type, amount, old_balance, new_balance, etc.
- **Serialization**: The trained model was saved to a file (e.g., xgboost_model.pkl) using pickle.
- **Inference**: In a complete implementation, the /transaction route would:
  - Collect the form data.
  - Preprocess it into a format that the model expects (e.g., a NumPy array).
  - Load the pickled model.
  - Use `model.predict()` or `model.predict_proba()` to assess if the transaction is fraudulent.
  - Flag suspicious transactions for review or block them outright.

## Contributing
Contributions are welcome! If you'd like to contribute, please follow these steps:
1. Fork the repository.
2. Create a new branch (git checkout -b feature/your-feature-name).
3. Make your changes.
4. Commit your changes (git commit -m 'Add some feature').
5. Push to the branch (git push origin feature/your-feature-name).
6. Open a pull request.
