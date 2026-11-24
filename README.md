# ACSIS

ACSIS is a multi-institution academic platform that brings student, teacher, and administrator tools together in one system. It supports class and exam workflows, student performance tracking, and institution management.

## What the system does

Depending on the user’s role and institution setup, ACSIS can:

- Give students access to their enrolled classes, exams, results, and performance information.
- Help teachers manage their classes and exams.
- Provide institution administrators with tools for managing users and monitoring academic activity.
- Support multiple institutions, with platform-level tools for managing institutions.
- Allow users to sign in with Google, subject to institution email-domain settings and account access.

## Sign-in and access

Google sign-in is configured for institution email domains. Institution administrators can set the accepted domain in **Admin → Institution settings**. Faculty and institution administrators who have been added to user management can also sign in with their registered email addresses.

Access to student, teacher, and administrator areas depends on the user’s account and assigned role.

## Run locally

### Requirements

- Git
- Node.js and npm
- PostgreSQL for database-backed local development

Copy `.env.example` to `.env` in the project root and configure the required Google OAuth and session settings:

\`\`\`env
GOOGLE_CLIENT_ID=your_google_client_id
GOOGLE_CLIENT_SECRET=your_google_client_secret
GOOGLE_CALLBACK_URL=http://localhost:3001/api/auth/google/callback
ALLOWED_EMAIL_DOMAIN=your-institution.edu
SESSION_SECRET=your_long_random_secret
DATABASE_URL=your_postgresql_connection_string
\`\`\`

Add the callback URL to the authorized redirect URIs for your Google OAuth client.

Install dependencies and start the API and frontend in separate terminals:

\`\`\`bash
npm install
npm install --prefix server
npm run dev:api
npm run dev
\`\`\`

For PostgreSQL, initialize the database with `sql/acsis.sql` and apply the required migrations in order. Google sign-in also requires the Google OAuth migration and institution membership or super-admin access.

For institution-specific sign-in domains, apply `sql/migrations/017_institution_email_domain.sql` and configure the domain under **Admin → Institution settings**. When no institution domain is set, the system uses `ALLOWED_EMAIL_DOMAIN` as a fallback.
