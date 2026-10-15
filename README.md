# DTCOB — Digital Banking Customer Onboarding System

## Overview

DTCOB is a web-based banking onboarding system that allows customers to apply for Savings or Current Accounts online.

The system handles identity verification, account registration, and customer authentication, with separate application flows for Malaysian and non-Malaysian customers. This project was built to explore full-stack development, database management, API integration, and handling sensitive customer information.

---

# System Architecture

The system uses Next.js for the frontend and backend, with PostgreSQL for banking records and external services for identity verification.

## 1. UI Layer — React & Next.js

The UI handles customer interactions throughout the onboarding process.

Main responsibilities:

- Display account registration forms
- Handle Savings and Current Account applications
- Generate QR codes for mobile verification
- Display identity verification results
- Show customer account information
- Handle navigation between application steps

React Context and browser storage help maintain form information between pages.

---

## 2. Backend Layer — Next.js API Routes

The backend connects the frontend with the database and external services.

It handles:

- Processing account applications
- Checking existing customer records
- Handling identity verification requests
- Managing login and password resets
- Sending email verification codes
- Encrypting sensitive customer information
- Saving customer and account records

PostgreSQL transactions are used when saving related records to prevent incomplete applications if an operation fails.

---

## 3. Data & External Services Layer

The system uses several databases and external services.

### PostgreSQL

Stores customer information, account details, addresses, and verification records.

### Firebase Firestore

Used to simulate three Malaysian government databases:

- **JPN** — MyKad identity verification
- **JIM** — Passport and immigration verification
- **SSM** — Business registration verification

### Innov8tif eKYC API

Handles document verification, facial recognition, and liveness checks.

### Supabase Storage

Stores uploaded documents and images used during identity verification.

---

# Design Decisions

## Why use Next.js?

Next.js was chosen because it supports both frontend development and backend API routes in one project.

This made it easier to connect application forms with database operations and external services without maintaining a separate backend.

The downside is that some application logic became quite large, especially across different onboarding flows.

## Why use simulated government databases?

Accessing actual government databases was not practical for this project, so Firebase Firestore was used to simulate JPN, JIM, and SSM records.

This allowed the system to demonstrate identity and business verification without depending on real government data.

The limitation is that the simulated records cannot fully represent real-world verification scenarios.

## Why use QR codes for identity verification?

Document scanning and facial verification are easier to complete using a phone camera.

The system generates a QR code that allows customers to continue verification on their phones while keeping the process connected through a shared journey ID.

The desktop application checks the verification status and updates once the mobile process is completed.

## Why encrypt customer information?

Since the application handles personal information, sensitive data is encrypted using AES-256-GCM before being stored.

Passwords are also hashed using bcrypt, while identity numbers have separate hashed lookup values.

This adds extra work when saving and retrieving information, but avoids keeping sensitive customer data in plain text.

---

# Main Features

## Account Onboarding

The system supports:

- Malaysian Savings Account applications
- Non-Malaysian Savings Account applications
- Malaysian Current Account applications
- New and existing customer registration

## Identity Verification

Customers can complete identity checks using:

- MyKad verification
- Passport verification
- QR code-based document capture
- Facial and liveness verification
- eKYC verification results

## Customer Authentication

The system includes:

- User registration and login
- Password reset
- Email OTP verification
- Duplicate account detection

## Customer Dashboard

Registered customers can:

- View account information
- Access their profiles
- Manage account details
- Start additional account applications

---

# Development Challenges

## 1. Keeping Desktop and Mobile Verification in Sync

One of the more challenging parts was connecting the mobile verification process with the desktop application.

The system needed to know when document or face verification was completed on another device.

Using a shared journey ID and database status updates allowed both devices to follow the same verification process.

## 2. Managing Different Application Flows

Savings and Current Accounts require different information, and Malaysian and non-Malaysian customers also go through different verification steps.

Keeping these flows separate made them easier to manage, but introduced some repeated code.

A more reusable form structure could help simplify future development.

## 3. Handling Duplicate and Failed Applications

The system needed to prevent customers from registering duplicate accounts and handle situations where verification or database operations failed.

Duplicate-account checks, error handling, and database transactions were added to reduce these problems.

---

## Testing and Results

Our team carried out functional and usability testing to check the onboarding process, account features, and overall user experience.

### Functional Testing

Tested:

- Malaysian and non-Malaysian Savings Account onboarding
- Login and existing user detection
- Adding and switching accounts
- Password reset using email OTP
- Customer support form submissions

**Result:** All 48 recorded test executions passed, covering 10 different test cases.

### Usability Testing

Seven participants tested the system through different banking scenarios.

**Results:**

- 47 out of 48 tasks completed (97.9%)
- 2 tasks required assistance
- Average satisfaction rating of 4.3/5

Participants generally found the system easy to navigate, but suggested clearer error messages, a progress indicator, and better OTP instructions.

### Defects and Improvements

Two issues were recorded during testing:

- **Identity Verification:** A passport image could not be detected because it was captured too far from the frame. Verification worked after retaking the image.
- **Customer Support Form:** A backend issue affected form submission. The issue was identified and marked as fixed.

Both defects were recorded as fixed in the defect log.

---

# Technical Skills Gained

Through this project, I gained experience in:

- Next.js and React development
- TypeScript programming
- Backend API development
- PostgreSQL database integration
- Firebase Firestore
- Third-party API integration
- Encryption and password hashing
- Customer authentication
- Multi-step form management
- Hardware-independent identity verification workflows

---

# Lessons Learned

This project helped me understand how challenging it can be to connect different services in one application. Keeping customer data and verification status consistent across multiple steps required more attention than just building the individual pages.

I also learned the importance of planning database structures and handling sensitive information properly from the beginning.

---

# Future Improvements

Possible improvements include:

- Adding automated API and end-to-end testing
- Improving server-side authentication and access control
- Replacing in-memory OTP storage with shared storage
- Reducing frequent verification-status requests
- Improving uploaded document security
- Making onboarding components more reusable

---

# Technologies Used

## Frontend

- Next.js 16
- React
- TypeScript
- Tailwind CSS

## Backend & Database

- Node.js
- Next.js API Routes
- PostgreSQL
- Firebase Firestore
- Supabase Storage

## API & Security

- Innov8tif eKYC API
- Nodemailer
- bcrypt
- Node.js Crypto (AES-256-GCM)
- Postman