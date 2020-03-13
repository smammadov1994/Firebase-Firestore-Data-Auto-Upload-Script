## Firebase-Firestore-Data-Auto-Upload-Script
This script allows a user to upload JSON data to their Firebase Firestore Collection.

## JSON

## Note: JSON file must have a key such as “users” when loading the file. This is because Firebase Cloud Firestore must have a Collection in the root of the of your project database.

file.json
{
  "file": [
    {
      "id": "1",
      "firstName": "foo",
      "lastName": "foo",
      "occupation": "foo",
      "count": "4",
      "score": "3",
    }
  ]
}


##. Firebase Setup

Firebase:
Go to the Firebase Console
Click “Add Project”
Enter “Project Name” and “Accept Terms.”

Project:
Now that the Firebase Project is added, we will want to link it to the project in our code editor. Click on the “</>” option on the Firebase Console landing page.
Create the file config.js in in your project (ADD THIS FILE TO YOUR GITIGNORE).
Copy the necessary data to the fields in bold (See below). We will need this file when we import.
config.js


// Firebase Config


const firebaseConfig = {
  apiKey: "API_KEY_HERE",
  authDomain: "AUTH_DOMAIN_HERE",
  databaseURL: "DATABASE_URL_HERE",
  projectId: "PROJECT_ID_HERE",
  storageBucket: "STORAGE_BUCKET_HERE",
  messagingSenderId: "MESSAGING_ID_HERE"
}


// Exports

module.exports = firebaseConfig;


## Cloud Firestore Setup
Navigate to the Left Sidebar and click on “Database.”
Click “Create Database.”
Choose “Test Mode” because we will need to read/write to your database.
Click “Enable.”


## Firebase Service Account
Navigate to the Left Sidebar and click on “Settings.”
Choose “Users and Permissions.”
Navigate to the “Service Accounts” tab.
Choose Node.js, which should be the default option.
Click “Generate New Private Key.”
Click “Generate Key” on the popup. A JSON file should have downloaded to your computer.
Rename the file to serviceAccount.json. We will need this file when we import.
serviceAccount.json



{
  "type": "service_account",
  "project_id": "PROJECT_ID_HERE",
  "private_key_id": "PRIVATE_ID_KEY_HERE",
  "private_key": "PRIVATE_KEY_HERE",
  "client_email": "CLIENT_EMAIL_HERE",
  "client_id": "CLIENT_ID_HERE",
  "auth_uri": "AUTH_URI_HERE",
  "token_uri": "TOKEN_URI_HERE",
  "auth_provider_x509_cert_url": "https://www.googleapis.com/oauth2/v1/certs",
  "client_x509_cert_url": "CLIENT_CERT_URL_HERE"
}


## NPM Dependencies

We will using a library to import the JSON data, so we will have initialize our project as an npm repository.
Initilialize NPM
npm init
Install Dependencies
npm install firestore-export-import
6. Example Code
This example will be using 3 files:
config.js
serviceAccount.json
import.js
config.js


// Firebase Config


const firebaseConfig = {
  apiKey: "API_KEY_HERE",
  authDomain: "AUTH_DOMAIN_HERE",
  databaseURL: "DATABASE_URL_HERE",
  projectId: "PROJECT_ID_HERE",
  storageBucket: "STORAGE_BUCKET_HERE",
  messagingSenderId: "MESSAGING_ID_HERE"
}

// Exports

module.exports = firebaseConfig;
serviceAccount.json


{
  "type": "service_account",
  "project_id": "PROJECT_ID_HERE",
  "private_key_id": "PRIVATE_ID_KEY_HERE",
  "private_key": "PRIVATE_KEY_HERE",
  "client_email": "CLIENT_EMAIL_HERE",
  "client_id": "CLIENT_ID_HERE",
  "auth_uri": "AUTH_URI_HERE",
  "token_uri": "TOKEN_URI_HERE",
  "auth_provider_x509_cert_url": "https://www.googleapis.com/oauth2/v1/certs",
  "client_x509_cert_url": "CLIENT_CERT_URL_HERE"
}


import.js

// Imports


const firestoreService = require('firestore-export-import');
const firebaseConfig = require('./config.js');
const serviceAccount = require('./serviceAccount.json');

// JSON To Firestore


const jsonToFirestore = async () => {
  try {
    console.log('Initialzing Firebase');
    await firestoreService.initializeApp(serviceAccount, firebaseConfig.databaseURL);
    console.log('Firebase Initialized');

    await firestoreService.restore('./data-clean/firebase/users.json');
    console.log('Upload Success');
  }
  catch (error) {
    console.log(error);
  }
};

jsonToFirestore();
7. Import
Run the following command to execute the function to import your JSON data to Firebase Cloud Firestore:
node import.js
It will take some time depending on the size of your file, but your data is now uploading to Firebase Firestore and has a home in the cloud now.
