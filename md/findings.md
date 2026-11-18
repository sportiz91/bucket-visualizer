# Table of Contents

- [Cursor Learnings](#cursor-learnings)
- [General Learnings](#general-learnings)
- [Best Cursor Workflow that no one talks about...](#best-cursor-worfklow)

## Cursor Learnings

To ensure that your rules take effect in your Cursor IDE settings, you should use the .cursorrules.json file format

The .json extension indicates that the file is in JSON format, which is a standard for configuration files and is likely what the IDE expects.

Here are a few points to consider:

File Naming: Keep the file named as .cursorrules.json. The .json extension is important for the IDE to recognize the file format and parse it correctly.

Location: Make sure that the .cursorrules.json file is placed in the correct directory where the Cursor IDE looks for configuration files. This is typically the root of your project or a specific configuration directory.

Validation: After creating the file, check if the Cursor IDE provides any feedback or validation for the rules defined in the JSON file. This can help ensure that your rules are correctly formatted and recognized.

4. Documentation: Refer to the Cursor IDE documentation for any specific requirements or examples regarding the configuration file. This can provide additional insights into how to structure your rules effectively.
   By following these guidelines, your .cursorrules.json file should work as intended in your Cursor IDE settings.

## General Learnings

The notes still has to be taken from chat interfaces like ChatGPT, Claude, etc. That's because the UX to do that is much more better and user friendly.

Also, we won't be using requests from cursor, which are sacred.

Q: Based on the information I've provided, can you explain me which is a secure way to do a user registration feature? Describe the feature

A:

Implementing a secure user registration feature for the AWS Bucket Manager requires careful handling of user data, authentication, and authorization to ensure safety and compliance with best practices. Here’s how to approach it:

Feature: User Registration

Objective: Allow users to create an account in the AWS Bucket Manager web application to manage their S3 buckets securely.

Key Security Considerations:

Data Encryption: Ensure all data transmitted between the client and server is encrypted using HTTPS (TLS/SSL).

Password Management:

Secure Password Storage: Use a strong hashing algorithm (e.g., Argon2, bcrypt) for storing user passwords.

Password Strength Validation: Enforce complex password requirements (e.g., minimum length, combination of upper/lowercase letters, numbers, and special characters).

Email Verification: Implement an email verification step to confirm user ownership of the email address.

Rate Limiting and CAPTCHA: Prevent automated bots and brute force attacks by implementing rate limiting and CAPTCHAs.

Data Validation: Sanitize and validate all input data to prevent injection attacks.

Multi-Factor Authentication (MFA): Encourage or require users to enable MFA for additional security.

Secure Token Generation:

Use cryptographically secure random values for session and password reset tokens.

Ensure tokens are short-lived and have proper scopes (e.g., password reset tokens only allow password reset).

Steps to Implement User Registration:

User Form Interface:

Fields: Include fields such as email, password, full name, and optionally, a username.
Validation: Perform client-side checks for basic field validation and provide error feedback.

Backend Processing:

Input Validation: Verify all input data server-side (e.g., validate email format, check password strength).

Hash Password: Hash passwords using a strong, adaptive hashing algorithm (e.g., bcrypt or Argon2) before saving them in the database.

Save User Data: Store user data, including the hashed password and a unique user ID, in a secure database.

Email Verification:

Generate Verification Token: Create a secure, time-limited token and send it to the user's email as a verification link.

Verification Endpoint: Handle the link redirection to activate the account after verification.

Confirmation and Notifications:

Notify the user that an account verification email has been sent.

Implement security notifications to alert the user if a registration attempt occurs using their email.

User Authentication and Session Management:

After successful registration and verification, log the user in by generating a secure session or JWT (JSON Web Token).

Implement secure session management practices, such as HttpOnly and Secure flags for cookies.

Email Service: Integrate with a secure and reliable email service (e.g., AWS SES, SendGrid) for email verification

Security Best Practices:

Use Environment Variables: Store secrets like database credentials, JWT secret keys, and email service credentials securely using environment variables.

Regular Audits: Perform regular security audits and code reviews to identify vulnerabilities.

Updates: Keep dependencies and libraries up to date to prevent known vulnerabilities.

User Experience Flow:

User visits the registration page and submits their details.

They receive an email with a verification link.

Clicking the link confirms the account, allowing the user to log in and start managing their AWS S3 buckets.

By following these guidelines, the registration feature in AWS Bucket Manager will be secure, ensuring user data is protected and the application remains trustworthy.

Q: What I don't understand is, when the user registers, we're saving that data on the backend? (user details with the hashed password) or only after he confirms his account when receiving the email?

A:

In most secure implementations, user data, including their hashed password, is saved in the backend when they first submit the registration form, even before they confirm their account via the verification email. However, their account status
should be marked as "unverified" until they confirm their email.

Here’s how it works step-by-step:

User Registration Submission:

The user fills out the registration form with their details (e.g., email, password, name).

The backend receives this data and:

Validates the input.

Hashes the password securely (e.g., with bcrypt or Argon2).

Saves the user data in the database, including a field for the verification status (e.g., isVerified: false) and a unique verification token.

Email Verification:

The backend generates a secure, time-limited token and sends it to the user’s provided email as a verification link.

The user's account remains in an "unverified" state at this point, meaning they cannot log in or use the app until the email is confirmed.

User Confirms Account:

When the user clicks the verification link, they are redirected to an endpoint on the backend.

The backend validates the token and, if valid:

Updates the user’s account in the database, changing the isVerified field to true.

Optionally logs the user in by generating a session or token.

Why Save Data Before Email Confirmation?

Security and Flow Integrity: Saving user data initially ensures that the account creation process is consistent and provides a record for the unverified user.
If the user never verifies, you can still track unverified attempts.

Token Association: Storing the verification token alongside the user data allows the backend to match the token to the right user during verification.

Better User Experience: Users only need to confirm their email, without re-entering their registration details, ensuring a smoother onboarding process.

Additional Considerations:

Token Expiry: Ensure that the verification token expires after a set time (e.g., 24 hours) for security. If the token expires before the user verifies, they may need an option to resend the verification email.

Data Cleanup: Implement a cleanup mechanism to remove unverified accounts that have remained in the database for an extended period (e.g., older than 30 days).

This method balances user experience with security, making it easy for users to register while maintaining control over access until their email is verified.

Q: So when the user registers in the page, we save the user details along a token. Which kind of token or which are the best practices for this token?

A:

When a user registers, you need to generate a verification token to confirm their email address.

This token ensures that only the user with access to the registered email can complete the registration process.

Here’s how to approach creating this token and best practices for its management:

Token Type and Best Practices:

Token Type:

Use a secure, randomly generated string for the verification token. This string should be long enough to prevent brute force attacks and be unique for each user.

The token should be cryptographically secure to avoid predictability. Tokens generated using libraries that rely on cryptographically secure random functions are ideal.

Token Generation Best Practices:

Length: Ensure the token is sufficiently long (e.g., at least 32 characters) to make guessing infeasible.

Complexity: The token should contain a combination of characters (e.g., alphanumeric and special characters) to increase randomness.

Secure Generation Libraries:

Node.js: Use crypto.randomBytes(32).toString('hex') to generate a 64-character hexadecimal string.
Python: Use secrets.token_urlsafe(32) for a URL-safe, random token.

Uniqueness: Ensure that each generated token is unique by verifying against existing tokens in the database if necessary.

Token Storage:

Store the token in the database, associated with the user’s record.

Hash the Token: For added security, consider hashing the token before storing it in the database (e.g., using SHA-256). This way, even if the database is compromised, the original token remains protected.

Example: Store hash(token) in the database, and when the user clicks the verification link, hash the token they provide and compare it to the stored hash.

Token Expiry:

Set an Expiry Time: Tokens should expire after a set period (e.g., 24 to 72 hours) to limit the window for an attacker to use them.

Automatic Cleanup: Implement a scheduled task to remove expired tokens from the database to maintain database hygiene.

Token Usage:

The token should be included as a query parameter in the verification link sent to the user’s email (e.g., https://yourapp.com/verify?token=abcdef123456).

The backend should match the received token (or its hash) with the stored token and check the expiry before verifying the user’s account.

Token Format:

UUID: While UUIDs (e.g., uuidv4) can be used for tokens, they are generally predictable and not recommended for security-sensitive tokens.

Random String: Prefer random strings from secure sources (as mentioned in token generation libraries) for cryptographic strength.

Token Management Workflow:

Generate Token: Upon registration, generate a random, secure token and associate it with the user record in the database.

Send Verification Email: Include the token in the verification link sent to the user’s email.

Verify Token:

When the user clicks the link, the backend checks the token against the database (or its hash) and verifies it has not expired.

If valid, update the user record to set isVerified to true.

Handle Expired Tokens:

If the token is expired, prompt the user to request a new verification email.

Remove Token After Use: Delete or invalidate the token in the database once it has been successfully used.

const crypto = require('crypto');

function generateVerificationToken() {
return crypto.randomBytes(32).toString('hex'); // Generates a 64-character token
}

Q: Is not possible to use a JWT for this token? Where should I use JWT?

A:

Yes, you can use a JWT (JSON Web Token) for email verification, but it’s essential to understand when and how to use it securely. Here's an overview of where using JWT makes sense and how to apply it correctly:

When to Use JWT:

Session Management: JWT is commonly used for authentication and maintaining sessions.

After a user logs in successfully, a JWT is issued and used for validating subsequent requests without requiring constant re-authentication.

Email Verification: JWT can be used as a verification token to confirm user email addresses during registration, but it comes with some caveats.

Using JWT for Email Verification:

If you choose to use a JWT for email verification, it can be embedded in the verification link sent to the user's email. The backend then verifies the token when the user clicks the link. Here’s how to do it securely:

Q: For the Token generation on the verification email, we are not going to use JWT, we are going to use a secure token using the crypto library on a Node.js express server. We will have a MongoDB database and we will delete unverified users after 30 days on inactivity. The token to validate the email will expire after 24 hours. If the user clicks on the link of an email with an expired token, we will redirect them to a page that prompts them to resend the verification email.

Do you think we can use MongoDB for free?
Can we use mailgun for free? If not, which is an email service provider we can use for free?

A:

Implementing a user registration system with email verification requires both a database to store user information and an email service to send verification emails.

Here's how you can utilize MongoDB and Mailgun for free:

1. MongoDB:

You can use MongoDB's free tier, known as the M0 cluster, which offers:

Storage: 512 MB
RAM: Shared RAM environment
Data Transfer: 10 GB in and 10 GB out per rolling seven-day period

This free tier is suitable for development, testing, and small-scale production applications. It provides essential features like high availability, end-to-end encryption, and a web-based interface for managing your database.

2. Mailgun:

Mailgun offers a free plan with the following limitations:

Email Sending: Up to 100 emails per day
Custom Domains: 1 custom domain
Data Retention: 1 day for logs and events
Support: Ticket-based support

This plan is suitable for low-volume email sending, such as sending verification emails during user registration.

If your application requires sending more than 100 emails per day, you might need to consider upgrading to a paid plan or exploring alternative email service providers.

Alternative Email Service Providers:

If Mailgun's free tier doesn't meet your needs, consider the following alternatives:

SendGrid: Offers a free plan that includes sending up to 100 emails per day.
Amazon SES (Simple Email Service): Provides a free tier of 62,000 emails per month when sending from an application hosted in Amazon EC2. However, this requires AWS account setup and may involve additional costs if not managed carefully.
Mailjet: Offers a free plan that allows sending up to 200 emails per day.

Conclusion:

For your user registration system with email verification, MongoDB's free tier should suffice for database management. Mailgun's free plan can handle up to 100 verification emails per day. If your email sending needs exceed this limit, consider alternatives like SendGrid or Mailjet, which offer comparable free tiers.

What does the Free plan offer?
Link: https://help.mailgun.com/hc/en-us/articles/203068914-What-does-the-Free-plan-offer

Overview

Mailgun's Free plan requires no credit card and offers the following:

100 messages per day
1 custom domain can be added to your account, verified, and utilized to send email
1 day of data retention for Logs and the Events API
1 inbound route
2 API keys
Email analytics and reporting
Ticket support

If these limits are not enough for your needs, take a look at our other plans via our pricing page. You'll find a full list of features and attributes to help inform your decision-making process.

If you don't find what you're looking for there, let's consider a custom contract! Our Sales representatives are here to find the best possible fit for your needs.

Need help upgrading your account? Check out this article.

I’ve Tested and Compared SendGrid vs Mailgun: Here’s What I Found
Link: https://mailtrap.io/blog/sendgrid-vs-mailgun/

Let’s cut to the chase—SendGrid vs Mailgun. What are the differences between these email sending platforms and which one should you choose?

I aim to answer these questions and provide you with an in-depth comparison of SendGrid and Mailgun.

Disclaimer: The features, prices, and ratings are up-to-date as of writing this article. But, they could change in the future, so they might be different when you’re reading.

SendGrid vs Mailgun: quick summary

SendGrid for businesses who are looking to send basic transactional emails while having additional marketing features.

Mailgun for tech-savvy users who are looking to add email-sending functionality to their apps without having a plethora of extra features.

SendGrid vs Mailgun: overview

Criteria SendGrid Mailgun
Infrastructure SMTP, Web API (REST) SMTP, Web API (RESTful)
Easy setup ✅ ❌
Pricing Could get pricey Affordable

# of official libraries 7 5

In-depth analytics ✅ ✅
Dedicated IPs ✅ ✅
Email logs ❌ ✅
Email validation ✅ ✅
Customer support Ticket, chat, email, and phone Ticket, chat, email
Email templates A wide variety of options Limited selection
Testing services Email preview, spam test, link validation Email preview, email validation

Founded back in 2009, SendGrid is one of the oldest names in the industry. It was acquired by Twilio in 2018 and since then has evolved from a platform focused on transactional email sending into an email marketing service.

With SendGrid, you can send both transactional and bulk emails with the added benefit of having various marketing features provided by Twilio.

Notable features:

Integration with Twilio
Onboarding services
Easy-to-use editor
Plethora of templates
Seamless list management

Mailgun was founded in 2010, and similarly to SendGrid, was acquired by another company, Pathwire, in 2021.

Since its inception, Mailgun has been geared towards developers and marketers who don’t have an issue working with code. Nowadays it also offers other messaging services provided by its parent company.

Notable features:

High delivery rates for all plans
Flexible and affordable pricing plans
A free tier with lots of features
Reliable customer support
Email validation

Email infrastructure comparison

Deliverability

When you’re on the lookout for a new ESP, one of the first things you should check is the deliverability rate they offer, or, the rate that your emails land in your recipients’ inboxes.

Every ESP offers different features for achieving higher rates, and in the case of SendGrid and Mailgun, it’s a close race.

SendGrid

According to SendGrid’s website, the platform promises a deliverability rate of 97% (with Expert Services) . It offers various advanced features that can help you increase your deliverability (more on those later), with the most notable ones being ISP outreach, IP warm-up, and Adaptive Communication Engine.

Having said that, keep in mind that you need to pay for a higher plan to unlock additional deliverability features.

Mailgun

Mailgun boasts a 97.4% deliverability rate. It achieves that by focusing on email validation, real-time monitoring, and other features that help you increase your odds of reaching your recipients’ inboxes.

Moreover, if you’re willing to pay up, you can get help from a dedicated expert by opting for Mailgun deliverability services. They will be able to help you with onboarding and provide you with custom deliverability strategies and guidance so you can get the most out of the platform.

Deliverability features

Regardless of the deliverability rate a provider promises, most of it is in your hands and how you use your tools. To learn more about it, check out our Deliverability Guide. ⬅️

When it comes to SendGrid and Mailgun, both platforms provide the following features:

Email validation
Event webhooks
SPF record authentication
Insightful analytics
Deliverability insights
Dedicated IP addresses
Inbox placement testing
Email preview
Reputation monitoring
Spam trap monitoring

However, some features are locked at lower-tier plans, especially in the case of SendGrid. For example, to unlock email validation or dedicated IPs, you need to switch to the most expensive plan they offer.

Scalability

What we talk about when we talk about scalability is how reliably an ESP can ensure businesses can grow their operation needs and send a high volume of emails to their customers.

That is, without compromising the quality of the service or encountering performance issues.

In short, your provider should allow you to send your emails to 1,000,000 recipients just as well as you would to 100.

SendGrid

G2: 8.8 🌟

From what I’ve seen, the platform has a reliable infrastructure and has quite flexible, although a bit pricey, plans that allow you to upscale your sending needs.

If you’re into statistics and you’re up for some more reads, check out this article by SendGrid. In it, they describe how they processed 4+ billion emails on Black Friday and Cyber Monday back in 2019.

Mailgun
G2: 9.1 🌟

Mailgun offers the Rapid Fire Delivery SLA, which guarantees 99% attempted delivery for up to 15 million messages within the first five minutes of sending.

The pricing plans also allow you to scale your operations easily as you can purchase more email sends without having to switch to a higher tier plan.

Reliability

Both SendGrid and Mailgun boast minimal downtime.

SendGrid

On a monthly basis, SendGrid delivers more than 80 billion emails to its users, some of which include Uber, Spotify, and Airbnb among others.

Link to SendGrid status page

Mailgun

Mailgun provides email services to 225,000 businesses worldwide, such as Wikipedia, DHL, and others. According to its website, the platform delivers more than 400B emails every year.

Link to Mailgun status page

User experience and pricing comparison

Ease of use
Now, I’ll go over how well SendGrid and Mailgun work with other tools and how easy they are to use, which can be a considerable factor when you’re choosing an ESP.

SendGrid

G2: 8.2 🌟 Capterra: 4.1 🌟

SendGrid has a smooth domain verification process and crystal-clear guidelines for sending mail.

After some clicking around, you soon get used to the SendGrid UI and realize you have every tool in one place. This is a big plus, as you don’t have to be a coder to learn the ropes.

Mailgun

G2: 8.8 🌟 Capterra: 4.3 🌟

With Mailgun, you’ll have to be at least comfortable with coding or have a team member who is well-versed in it.

On the positive side, its UI is clear, provides all the information you need at hand, and after some research, I wasn’t able to find any customer complaints regarding the interface.

Customer support

Customer support is one of the most important aspects I take into account when reviewing ESPs, and incidentally, it’s an area where SendGrid and Mailgun differ quite a bit.

SendGrid

G2: 7.6 🌟 Capterra: 3.8 🌟

SendGrid has a round-the-clock support team that you can contact via ticket, chat, email, and phone. However, phone support is available only for customers with a Pro or higher account.

Additionally, the response times for your inquiries depend on the pricing plan you choose.

Mailgun

G2: 8.3 🌟 Capterra: 3.9 🌟

Mailgun has a dedicated support team that also works around the clock. You can reach out to them via ticket, chat, or email, but you can’t do it via phone, unfortunately.

Higher-tier plans also get dedicated and prioritized support, whereas free users don’t have access to ticket support and have to rely on help center documentation.

Pricing

When considering the pricing of an ESP, you should take into account whether they cut back on important aspects such as deliverability or features like email logs. An ESP should also match your business’ size and offer the best value for your money.

Overall, I’ve found that SendGrid is on the pricey end as you need to pay up for additional features. Whereas Mailgun pricing is based on the number of emails you want to send, with some advanced benefits reserved for the highest tier plans.

In the following section, I’ll cover both free and paid plans separately, so you can have a better idea of what to expect.

Free plans

SendGrid:

Up to 5,000 free emails per month
Email APIs and SMTP relay
Tracking, analytics, webhooks
1-day log retention
Mailgun

100 free emails per day
Email APIs and SMTP relay
1 teammate permission
1 event webhook
Dynamic template editor
Email analytics

Paid plans

Keep in mind that both SendGrid and Mailgun have sliders on their respectable pricing pages, which you can use to increase the emails you can send per month for all plans.

SendGrid:

Essentials: $19.95/month
Up to 50,000 emails per month
APIs, SMTP relay
1 teammate permission
Features such as analytics, deliverability insights, etc.
No access to email validation, dedicated IPs, sub-user management, single sign on (SSO)
Pro: $89.95/month
Up to 150,000 emails per month
Access to all features, including the ones not available for Essential tier
Premier: custom pricing
More than 1,500,000 emails per month
All features available
Mailgun:

Foundation: $35/month
50,000 emails per month
APIs, SMTP relay
Tracking, analytics, 5-day log retention, ticket support
Extra emails for $1/1,000 emails
Scale: $90/month
100,000 emails per month
Upgraded Foundation tier features
30-day log retention, dedicated IPs, etc.
Extra emails for $0.80/1,000 emails
Enterprise: custom pricing
More than 2,5000,000 emails per month
All features from lower tiers included
Dedicated support
Proactive account monitoring

Q: Is MongoDB M0 Free Cluster a Mongo Atlas? What does it means Shared RAM environment and 10 GB in and 10 GB out per rolling seven-day period?

A:

Yes, the M0 Free Cluster is a tier within MongoDB Atlas, which is MongoDB's cloud database service.

It allows you to deploy and manage MongoDB databases in the cloud without worrying about infrastructure setup or maintenance.

Clarifying the Details:

1. Shared RAM Environment:

Meaning: In a shared RAM environment, your database instance shares computing resources, such as RAM and CPU, with other free-tier clusters.

This means you won’t have a dedicated amount of RAM or processing power, which can affect performance during peak usage times when multiple users are accessing shared resources.

Implications: This type of environment is suitable for development, testing, or small-scale projects. However, it might not handle high loads or intensive queries as efficiently as dedicated resources.

2. 10 GB In and 10 GB Out per Rolling Seven-Day Period:

Definition: This refers to the data transfer limits for the M0 Free Cluster.

"10 GB In": You can upload (write) up to 10 GB of data into your cluster over any rolling seven-day period.
"10 GB Out": You can download (read) up to 10 GB of data from your cluster over any rolling seven-day period.

Rolling Period: This means that the limit resets continuously based on the last seven days. For example, if you start using 2 GB of your quota today, it counts for today and the next six days unless data usage changes during that time.

Implications: This limit is generally sufficient for development and light usage but may become a bottleneck for larger applications or those with higher data transfer needs.

Summary:

M0 Free Cluster in MongoDB Atlas is great for small projects, learning, and development with a shared environment and capped data transfer limits.

The shared RAM means your database may experience variable performance, while the 10 GB data transfer limit is fine for low to moderate traffic but not for data-heavy applications.

If your application begins to scale up or if you need more stable performance and greater data capacity, you might need to upgrade to a higher-tier MongoDB Atlas plan.

Deploy a Free Cluster
Link: https://www.mongodb.com/docs/atlas/tutorial/deploy-free-tier-cluster/

Atlas Free clusters provide a small-scale development environment to host your data. Free clusters never expire, and provide access to a subset of Atlas features and functionality.

Paid clusters provide full access to Atlas features, configuration options, and operational capabilities. For more information on paid clusters, including deployment instructions, see Create a Cluster.

You can deploy only one Free cluster per Atlas project.

Procedure

You can create Free clusters through the Atlas CLI, Atlas User Interface, and Atlas Administration API. Select the appropriate tab based on how you would like to create the Free clusters.

To create one cluster, load sample data, add your IP address to your project IP access list, and create a MongoDB user for your cluster using the Atlas CLI, run the following command:

## best-cursor-worfklow

Instead of giving Cursor a simple instruction to build a whole web application, you need to know how to write the best documentation
to communicate an align with Cursor:

It starts with a # Product Requirements Document (PRD)

Then it has a ## Project Overview.

In here he's writting generally about the project, the problem it's solving, the users it's solving for, etc.

Then it has a ## Core Functionalities.

Then, for every Core Functionality, it has a ### that is the title of the functionality. It can then be divided into multiple sections.

For example:

### 1. Subreddit Management

##### 1.1 Create Subreddit

##### 1.2 Delete Subreddit

### 2. Post Management

##### 2.1 Create Post

##### 2.2 Delete Post

etc..

Then it has a ## File Structure.

Then it has a ## Documentation and Code Examples.

The code examples of the documentation you are using are part of the subtitles of the ## Documentation and Code Examples section.

For example:

### 1. Fetching Reddit Posts with Snoowrap

Process of writing Cursor PRD:

- Draft PRD
- Research for Packages
- o1 / Claude 3.5 Sonnet designing the project structure.
- o1 / Claude 3.5 Sonnet writing the final PRD.

In order to create all this file he creates an instructions.md file.

For the packages you can ask ChatGPT or Claude to know which are the best ones.

Then, we you have the documentation of the packages you'll use, you can add them to Cursor in the Cursor Settings.

Then, on the Composer, he asks the documentation for fetching reddit posts with snoowrap, in order to give the documentation to Cursor
after.

The prompt he used is:

"Help me build a simple typescript file of fetching recent reddit posts data from past 24 hours under 'ollama', including title, content, score, num comments, date using Snoowrap."

For the File Structure he's using the tree library. It helps to get a snapshot of the project structure.

If you do "tree" in the terminal, you'll see the file structure.

But that is not neccesary what we need, we have to do:

tree -L 2 -I "node_modules|.git"

After this he has all the neccesary draft PRD in order to pass it to o1 to write the final PRD.

He copy pastes all the draft PRD into the o1 prompt.

When he has the PRD ready, he starts building the project with Cursor Composer (CTRL + I). Cursor Composer is the
multi file editor.

In the composer he passes as context the instructions.md file. And he says "Build the section 1.1 Create Subreddit".

Then, he asks for the next section 1.2 Delete Subreddit.

And he continues doing this for every section in the PRD.

He's using Helicone to track the requests and LLM observability to track the LLM calls.

Later, in order to add the database, he adds a new .md file. He details the requirement on a prompt to o1 preview model.

Then o1 writes the entire documentation file, with all the tables and such.
