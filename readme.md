# Gym Management System

This is a comprehensive gym management system designed to streamline various aspects of running a fitness center. From managing members to scheduling classes and tracking fitness progress, this system provides tools to enhance both administrative efficiency and member experience.

---

## Features

* **Member Management**: Easily add new members, view their details, update their status (active, inactive, suspended), and remove them from the system. Member data is persisted in **Firebase Firestore** and cached in **local storage**.
* **Class Scheduling**: Schedule various types of fitness classes with specific dates and times.
* **Fitness Goal Tracking**: Members can set and monitor their personal fitness goals, helping them stay motivated.
* **Exercise Logging**:
    * **Outdoor Exercises**: Record activities like running, cycling, with details such as duration and calories burned.
    * **Indoor Exercises**: Log structured workouts, including reps, sets, and calories.
    * **Gym Exercises**: Similar to indoor exercises, specifically for gym-based routines.
* **Activity Tracking**: A general section to log any other physical activities with duration and calorie expenditure.
* **Diet & Nutrition**: Keep a record of meals consumed, including meal type, description, and calorie intake.
* **User Authentication**: Secure user access via **Firebase Authentication**, ensuring only authorized personnel can access the dashboard.
* **Theme Toggle**: Switch between a **light and dark theme** for personalized visual comfort, with preference saved in local storage.
* **Collapsible Sections**: Organize the dashboard by collapsing or expanding individual sections for better readability.
* **Data Persistence**: All critical data, including member information, schedules, and fitness logs, is securely stored and retrieved from **Firebase Firestore**.

---

## Technologies Used

* **HTML5**: Provides the foundational structure of the web application.
* **CSS3**: Styles the application, offering a clean and intuitive user interface with support for theme switching.
* **JavaScript (ES6+)**: Powers the dynamic functionalities, DOM manipulation, and interaction with Firebase services.
* **Firebase**:
    * **Firebase Authentication**: Handles user registration, login, and session management.
    * **Firebase Firestore**: Serves as the primary NoSQL cloud database for storing and synchronizing all gym-related data.
* **Web Storage API (`localStorage`)**: Used for client-side caching of theme preferences and member data, enhancing performance and user experience.

---

## Getting Started

To set up and run this gym management system, follow these steps:

### 1. Firebase Project Configuration

1.  **Create a Firebase Project**: Navigate to the [Firebase Console](https://console.firebase.google.com/) and initiate a new project.
2.  **Enable Firestore Database**: Within your Firebase project, go to the **Firestore Database** section and create a new database. For development, you can start in "test mode" to quickly get going, but remember to implement secure rules for production.
3.  **Enable Authentication**: Access the **Authentication** section in your Firebase project. Enable the **Email/Password** sign-in method under the "Sign-in method" tab.
4.  **Obtain Firebase Configuration**: From your project settings (accessible via "Project overview" -> "Project settings" -> "General"), locate the "Your apps" section and select "Web". Copy the Firebase configuration object (e.g., `const firebaseConfig = { ... };`).

### 2. Code Integration

1.  **Insert Firebase Config**: Open your `dashboard.html` file. Locate the `<script>` block in the `<head>` section and replace the placeholder `firebaseConfig` object with the actual configuration you copied from the Firebase Console.
    ```html
    <script>
        const firebaseConfig = {
            apiKey: "YOUR_API_KEY",
            authDomain: "YOUR_AUTH_DOMAIN",
            projectId: "YOUR_PROJECT_ID",
            storageBucket: "YOUR_STORAGE_BUCKET",
            messagingSenderId: "YOUR_MESSAGING_SENDER_ID",
            appId: "YOUR_APP_ID"
        };
        firebase.initializeApp(firebaseConfig);
        const auth = firebase.auth();
        const db = firebase.firestore();
    </script>
    ```
2.  **Authentication Flow**: This dashboard expects a user to be authenticated. You will need a separate `index.html` (or a similar login/signup page) that handles user registration and login. Upon successful authentication, this page should redirect the user to `dashboard.html`.

### 3. Firebase Security Rules

For secure data handling, it is crucial to define rules in your Firebase Firestore. Go to **Firestore Database** -> **Rules** in the Firebase Console and implement rules that restrict access to user-specific data. A common rule for authenticated users to access only their own data is:

```firestore
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{userId}/{collection=**} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }
  }
}
```

### 4. Running the Application Locally

1.  **Save Files**: Ensure all the provided HTML, CSS, and JavaScript code is saved within a single `.html` file (e.g., `dashboard.html`).
2.  **Open in Browser**: Open `dashboard.html` directly in your web browser.
3.  **Local Server (Recommended)**: For a more robust development environment, especially for testing Firebase functions that require a secure context, consider using a local server. Tools like the "Live Server" VS Code extension or Python's built-in `http.server` can serve your files.

---

## Usage Guide

* **Theme Management**: Click the theme toggle button (labeled "Dark Mode" or "Light Mode") to switch between themes. Your preference will be remembered for future sessions.
* **Navigating Sections**: Click on any section header (e.g., "Outdoor Exercises", "Member Management") to expand or collapse its content, improving dashboard readability.
* **Adding Data**: Input the relevant information into the fields within each section and click the corresponding "Add" or "Set" button to save your entry.
* **Deleting Entries**: Each entry displayed will have a "Delete" button. Click it to permanently remove the entry.
* **Member Actions**: In the Member Management section, you can add new members. For existing members, "Delete" buttons allow removal, and an "Left" button allows changing an active member's status to inactive.
* **Logging Out**: Click the "Logout" button to securely sign out of your session and return to the application's login page.

---

## Project Structure (Assumed)

```
.
├── index.html        # Your login/signup page (required for authentication flow)
└── dashboard.html    # The main fitness tracker dashboard (this code)
```

**Note**: For larger projects, it's best practice to separate CSS into a `.css` file and JavaScript into `.js` files. This enhances code organization, maintainability, and browser caching.

---

## License

This project is open-source and available under the MIT License.

---