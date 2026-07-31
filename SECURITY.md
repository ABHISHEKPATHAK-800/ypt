# Security policy

## Current status

DuoTrack is a personalized prototype for trusted users. It is not currently suitable for public account creation, sensitive data, or untrusted multi-user deployment.

The repository contains only frontend code. Visitors can inspect and modify that code in their browser, so frontend checks must never be treated as a security boundary.

## Important prototype limitations

- Login records and passwords are handled in client-side code or the shared database rather than a real authentication provider.
- Administrative/settings protection is enforced in the client.
- The frontend connects directly to Firebase Realtime Database.
- The Groq API key is entered in the browser, stored in shared group data, and used directly by the browser.
- Full backups and chat exports may contain private user content and configuration.
- Profile/chat images are stored as data URLs and can increase database exposure and usage.

Do not use a password from any other account with this prototype. Do not place confidential, financial, identity, or other sensitive information in chat or backups.

## Required changes before wider deployment

1. **Rotate exposed credentials.** Replace any credentials that have ever been committed to the public repository or stored in readable shared data.
2. **Use Firebase Authentication.** Map `auth.uid` to an allowed group membership instead of comparing passwords in JavaScript.
3. **Lock down Realtime Database Rules.** Deny access by default and allow only authenticated group members to access their own permitted paths.
4. **Move AI requests to a backend.** Store the Groq key in server-side environment configuration and proxy narrowly validated requests.
5. **Separate roles.** Implement administrator/member authorization in trusted rules or backend code; do not rely on a hidden frontend password.
6. **Validate writes server-side.** Restrict field types, sizes, message lengths, image sizes, and allowed state transitions.
7. **Limit exports and restores.** Require recent authentication and an administrator role for destructive restore operations.
8. **Add abuse controls.** Apply rate limits and quotas to account registration, messages, images, AI calls, and signaling writes.
9. **Review data retention.** Define how long chat, images, call signaling, and historical analytics are kept.
10. **Add dependency controls.** Pin dependencies, add a Content Security Policy, and consider self-hosting critical static assets.

## Firebase configuration

A Firebase web API key is an application identifier and is normally visible in browser code. It is not a substitute for authentication or authorization. Protect the database with Firebase Authentication, restrictive Security Rules, App Check where appropriate, and usage alerts.

Never commit:

- Firebase service-account JSON;
- private keys;
- server tokens;
- unrestricted third-party API keys;
- production backup files;
- exported chat histories.

## Reporting a vulnerability

Do not open a public issue containing a password, API key, private chat, personal information, or an exploitable database path. Contact the repository owner privately with:

- a short description of the issue;
- the affected feature;
- safe reproduction steps using test data;
- the likely impact;
- a suggested fix, if known.

After remediation, rotate any potentially exposed credential and review Firebase access logs/usage before publishing technical details.
