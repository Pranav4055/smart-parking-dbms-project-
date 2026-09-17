# Smart Parking Security & Cleanup Review

## Implemented in this release

- Removed `.git` from the distributable archive so repository history is not shipped.
- Removed local `.env` files and added `.env.example` templates.
- Removed bundled Python virtual environment, Node dependencies, caches, and build output.
- Removed insecure hardcoded admin credentials/default password. Admin seeding now requires `ADMIN_EMAIL` and `ADMIN_PASSWORD`.
- Enforced a minimum 32-character JWT secret.
- Enabled secure HttpOnly/SameSite cookies by default; `COOKIE_SECURE=false` is documented only for local HTTP development.
- Added trusted-host protection and common security response headers.
- Added login and registration rate limiting.
- Added server-side validation for passwords, names, vehicle data, booking duration/type, parking configuration, and settings.
- Added safe MongoDB ObjectId validation for booking endpoints.
- Prevented parking configuration path/body vehicle-type tampering.
- Consolidated duplicate slot assignment logic into one helper.
- Removed the unused refresh-token cookie/utility.
- Trimmed payment identifiers from normal booking-history API responses.
- Added useful MongoDB indexes for email, payment orders, bookings, and parking configuration.

## Requires deployment configuration / infrastructure

- Use HTTPS in production and set `COOKIE_SECURE=true`.
- Configure `TRUSTED_HOSTS` with only the real production hostnames.
- Keep MongoDB credentials, JWT secret, Razorpay secret, and admin password outside source control.
- Rotate any credentials that were ever committed to the original repository or shared archive. Removing `.git` from this release does not invalidate previously exposed credentials.
- Dependency vulnerability scanning should be run in CI with network access (`npm audit` and a Python dependency scanner such as `pip-audit`).
- The login/register limiter is process-local; production deployments with multiple API instances should use a shared rate-limit store such as Redis or an API gateway/WAF.
- Bot protection/WAF rules are infrastructure concerns and should be enabled at the production edge.
- Database encryption at rest and TLS for MongoDB should be enabled in the managed database configuration.
