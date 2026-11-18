# Product Requirements Document (PRD)

## Project Overview

**Project Name**: AWS Bucket Manager
**Date**: 06/11/2024
**Author**: @llm_san

This project is a web application that helps developers manage their AWS s3 buckets directly from the browser in a nice and
intuitive way. Users will be able to register into the system, login and start attaching buckets to their account. After a bucket
is attached, they will be able to see the contents of the bucket, create, delete, update objects (CRUD)

### Problem Statement

Developers often struggle to manage their AWS s3 buckets. They have to use the AWS console, which is not very intuitive and
requires a lot of clicks. Apart from that, sometimes they work in teams where they don't have access to the same AWS account, but
still they have the AWS credentials. The only option left for this developers is to use the AWS CLI, but this requires a lot of
terminal actions and is not very friendly. AWS Bucket Manager solves this problem by providing a nice and intuitive UI to manage
their AWS s3 buckets in a very easy and fast way.

### Target Users

- Primary: Full Stack Developers and Cloud Engineers

  - Developers who need to manage S3 buckets frequently
  - DevOps engineers managing cloud storage resources
  - Engineers with AWS credentials but limited AWS Console access

- Secondary: Project Managers and Team Leads

  - Managers who need to monitor team's cloud resource usage
  - Team leads coordinating bucket access across teams
  - Technical leads overseeing multiple projects' storage needs

- Tertiary: System Administrators and Support Staff

  - IT administrators managing storage infrastructure
  - Technical support staff assisting with bucket access issues
  - System operators monitoring storage usage and costs

### Project Goals

1. Primary goal:

   - Simplify and accelerate S3 bucket management by providing a user-friendly interface that reduces bucket operation time by 70% compared to AWS Console

2. Secondary goal:

   - Provide a secure and scalable solution for storing and managing sensitive data
   - Increase team collaboration through integrated communication features

## Core Functionalities

### 1. Authentication & Authorization

##### 1.1 User Registration

This feature allows users to register for your app through a registration form hosted on a Next.js page. The process involves user data validation, password hashing, secure token generation, sending verification emails via Mailgun, and handling the verification flow. Additionally, there’s a scheduled cleanup process for inactive user data.

Detailed Steps of the Sign-Up Process:

1. User Registration Page (Next.js):

- Page: A dedicated registration page built using Next.js.
- Form Implementation: The form uses React Hook Form for efficient form handling, validation, and submission.
- Fields: The form collects essential user information such as:
  - Email
  - Password
  - Full Name (optional but recommended for personalization)

2. Client Side Form Validation:

- Validation Rules: Basic client-side validation rules, such as checking for a valid email format and enforcing password complexity. These are the requirements:
  - Email must be a valid email format and is the unique identifier
  - Password must be at least 8 characters long, contain at least 1 uppercase letter, 1 number and 1 special character
  - Full Name is optional but recommended for personalization
- Feedback: Inline error messages and feedback provided by React Hook Form. If any request fails after sending the form,
  inform the user using React Toastify.

3. Backend Form Submission:

- Request from the frontend is sent with axios to the backend.
- Express.js backend server receives form data.
- Validate the data using Joi.

4. Password Hashing:

- The bcrypt library in Node.js is used for hashing the password.
- Bcrypt is used for hashing the password with a salt of 10.
- The hashed password, not the plain text, is stored in MongoDB Atlas.
- The hashed password is generated with a pre hook in the Mongoose schema, before creating a new user.

// The bcrypt algorithm is a separate library and cannot be directly used with the crypto library in Node.js.
// Instead, use the bcrypt library for password hashing.

- Email (unique identifier)
- Password (min 8 chars, 1 uppercase, 1 number)
- Full Name
- Role (Developer/Manager/QA)

- Validation rules: [List all validation rules]

  Example implementation:

  - Email format validation
  - Password strength requirements
  - Rate limiting (max 5 attempts per hour)

##### 1.2 User Login

- Authentication method: [Specify your auth method]

  Example implementation:

  - JWT-based authentication
  - OAuth 2.0 integration (GitHub, GitLab)

- Session management: [Specify your session management approach]

  Example implementation:

  - Token expiration: 24 hours
  - Refresh token rotation
  - Multiple device support

### 2. Data Management

##### 2.1 Create Operations

- [Specify create operations]

##### 2.2 Read Operations

- [Specify read operations]

##### 2.3 Update Operations

- [Specify update operations]

##### 2.4 Delete Operations

- [Specify delete operations]

### 3. User Interface

##### 3.1 Layouts

- [List main layouts]

##### 3.2 Components

- [List reusable components]

## Technical Requirements

### Technology Stack

#### 1. Main Technologies:

- Frontend: Next.js, TypeScript, Shadcn/UI, TailwindCSS
- Backend: Node.js, Express
- Database: MongoDB free tier (M0 free cluster)
- Authentication: JWT
- Hosting: Hetzner

#### 2. Libraries:

- Axios: for API requests
- React Hook Form: for form handling
- React Query: for data fetching, caching, and synchronization
- React Toastify: for notifications
- Mailgun: for email sending
- Express.js: for backend
- Bcrypt: for password hashing and storing in MongoDB Atlas
- Joi: for data validation on the Express.js backend

### External Dependencies

- [List major third-party services/APIs]
- [List major libraries/frameworks]

### Documentation

- [List documentation]

## File Structure
