# Income-Expenses Tracker

A simple, single-page web application for tracking income and expenses. This application allows users to manage multiple accounts, record transactions, get AI-powered category suggestions for transactions, analyze spending habits, and export data to CSV. It uses Firebase for data persistence and (optional) AI features via the Gemini API.

## Features

*   **Multi-Account Management:** Create and switch between different financial accounts.
*   **Transaction Tracking:** Add income and expense transactions with descriptions, amounts, and categories.
*   **Balance Calculation:** Automatically calculates and displays the current balance for the selected account.
*   **Transaction History:** View a list of all transactions for the selected account, sorted by date.
*   **Dark Mode:** Toggle between light and dark themes for user preference.
*   **AI-Powered Category Suggestion:** Get automatic category suggestions for your transactions using the Gemini API.
*   **AI-Powered Spending Analysis:** Receive an analysis of your spending habits and tips for saving money, powered by the Gemini API.
*   **CSV Export:** Export transaction data for the selected account to a CSV file.
*   **Firebase Backend:** Securely stores data in Firestore, with user data isolated.
*   **Local Fallback Mode:** If Firebase is not configured, the app can still be used locally, but data will not be saved. AI features will also be disabled.

## Setup and Configuration

This application is designed to be a single `index.html` file that can be hosted on any static web hosting platform or run locally in a browser. However, for full functionality (data persistence and AI features), you need to configure Firebase and provide a Gemini API key.

The application expects certain JavaScript variables to be defined in the global scope *before* the main script in `index.html` runs. This is typically handled by an injection mechanism in the hosting environment (like Firebase Hosting with environment variables, or a CI/CD pipeline).

### 1. Firebase Configuration

You need to create a Firebase project and set up Firestore.

*   **`__firebase_config`**: A global JavaScript object containing your Firebase project's configuration.
    ```javascript
    // Example: This should be injected into the HTML before the main script
    // const __firebase_config = {
    //   apiKey: "YOUR_FIREBASE_API_KEY",
    //   authDomain: "YOUR_PROJECT_ID.firebaseapp.com",
    //   projectId: "YOUR_PROJECT_ID",
    //   storageBucket: "YOUR_PROJECT_ID.appspot.com",
    //   messagingSenderId: "YOUR_MESSAGING_SENDER_ID",
    //   appId: "YOUR_FIREBASE_APP_ID",
    //   measurementId: "YOUR_MEASUREMENT_ID" // Optional
    // };
    ```
*   **`__app_id`**: A string representing a unique identifier for this application instance, used to namespace data in Firestore. This helps if you deploy multiple instances of this tracker for different purposes.
    ```javascript
    // Example:
    // const __app_id = 'my-personal-tracker';
    ```
*   **`__initial_auth_token`** (Optional): If you want to use Firebase custom token authentication, provide the token via this variable. Otherwise, the application will use anonymous sign-in.
    ```javascript
    // Example:
    // const __initial_auth_token = 'YOUR_CUSTOM_AUTH_TOKEN';
    ```

If `__firebase_config` is not found, the application will run in "Local Mode":
*   Data will not be saved.
*   User ID will display "Firebase Not Configured (Local Mode)".
*   All features requiring Firebase (saving/loading transactions/accounts, AI features that need API key via same mechanism) will be disabled.

### 2. Gemini API Key Configuration

For the AI-powered features (spending analysis and category suggestion) to work, you need to provide a Gemini API key.

*   **`__gemini_api_key__`**: A string containing your Gemini API key.
    ```javascript
    // Example:
    // const __gemini_api_key__ = 'YOUR_GEMINI_API_KEY';
    ```

If `__gemini_api_key__` is not defined or is empty:
*   The spending analysis feature will show an error message indicating the API key is missing.
*   The category suggestion feature will show an error message indicating the API key is missing.
*   These features will be disabled, but the rest of the application will function normally (assuming Firebase is configured).

## Running Locally

1.  Ensure you have a modern web browser.
2.  Open the `index.html` file directly in your browser.
3.  To test with Firebase and Gemini:
    *   You would typically not hardcode your keys directly into the `index.html` for security reasons if you plan to commit/share the file.
    *   For local development, you could temporarily add script tags in `index.html` (before the main module script) to define `__firebase_config`, `__app_id`, and `__gemini_api_key__`. **Remember to remove these before committing or deploying.**
    *   A better approach for local development is to use a local web server that can inject these variables, or use browser developer tools to set these global variables before the script executes.

## Data Storage

User data (accounts and transactions) is stored in Firestore with the following path structure:
`artifacts/{appId}/users/{userId}/accounts/{accountId}`
`artifacts/{appId}/users/{userId}/transactions/{transactionId}`

This ensures that data is namespaced by the application instance (`appId`) and then by the authenticated user (`userId`).

## Code Display Feature

The application includes a section at the bottom to "Show/Hide Code" and "Copy Code". This feature displays the full HTML source of the `index.html` page itself within a textarea and allows copying it. This might be useful for debugging or if users want to quickly grab the entire self-contained application code.
