# Google Authentication

Arguz uses Google Identity Platform for user authentication. This section explains the authentication methods available and how to configure them for your organization.

## Authentication Methods

Arguz supports two authentication methods, both powered by Google Identity Platform:

### Google OAuth (Recommended)

Sign in with your existing Google account. This is the simplest method:

- No additional password to manage
- Leverages Google's security features (2FA, Advanced Protection)
- One-click sign-in if you're already logged into Google

### Email/Password

Sign in with an email address and password:

- Does not require a Google account
- Uses Google Identity Platform's secure authentication infrastructure
- Supports password reset flows
- Email verification required

## Setting Up Authentication for Your Organization

Authentication is configured at the platform level — users from any organization use the same authentication system. Individual users can choose either method when signing up.

### First-Time Login

1. Go to [app.arguz.io](https://app.arguz.io)
2. Choose **Sign in with Google** or **Sign in with Email**
3. For email sign-in:
    - Enter your email address
    - If it's a new account, you'll be prompted to create a password
    - Verify your email address
4. You'll be guided through creating your first organization

### Managing Users

Organization owners and admins can manage users from the organization settings:

- **Invite users**: Send invitations to team members
- **Assign roles**: Set each user's role (Owner, Admin, Editor, Viewer)
- **Remove users**: Revoke access when needed
- **View audit log**: See who joined and when

## Single Sign-On (SSO)

For enterprise customers, contact Arguz support about configuring custom SSO integrations.

## Security

- All authentication uses Google Identity Platform's infrastructure
- Passwords are managed by Google — Arguz never stores raw passwords
- Session management uses encrypted, HTTP-only cookies with the `__Host-` prefix
- Sessions are tied to your organization context
- Multi-factor authentication is available through Google's security settings (for Google OAuth users)

## Troubleshooting Login Issues

| Issue | Resolution |
|---|---|
| Can't sign in with Google | Ensure you're logged into the correct Google account; try an incognito window |
| Forgot email password | Use the password reset link on the sign-in page |
| Account locked | Wait a few minutes or contact your organization admin |
| Not seeing your organization | Confirm your admin has invited you with the correct email address |

## Session Management

- Sessions persist across browser restarts
- Logging out invalidates the session on the Arguz side
- Sessions are scoped to the Arguz application domain
- Admin sessions are separate from main application sessions (different subdomains)
