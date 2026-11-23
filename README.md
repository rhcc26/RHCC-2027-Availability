# Ryhill & Havercroft CC — 2027 Availability App

This is a free-to-host mobile-friendly web app, not a paid native App Store app. It uses GitHub Pages and Supabase's free tier (subject to their current usage limits and policies).

## Setup (one time)
1. Create a Supabase project or select the correct existing project. **Do not run schema.sql against an existing project without checking for conflicting tables/policies.**
2. In Supabase SQL Editor, run `schema.sql`, then run `fixtures.sql` ONCE. Running fixtures.sql twice will duplicate fixtures.
3. Under Supabase Authentication > Providers, enable Email (OTP / magic links). Under Authentication > URL Configuration, set Site URL to your GitHub Pages address, e.g. `https://rhcc26.github.io/RHCC-availability/`, and add that exact address to Redirect URLs. Ensure email delivery is configured and working.
4. In Project Settings / Connect, copy the **project URL** and **publishable (or anon) key**. Put them in `config.js` (not the secret/service_role key). Supabase project URL is `https://YOUR-PROJECT.supabase.co`, not the dashboard URL.
5. Upload `index.html` and `config.js` to the root of a GitHub Pages repository. GitHub repository Settings > Pages > Deploy from branch > main / root. You can also upload README and SQL files, but the app itself only needs index.html and config.js.
6. Open the public GitHub Pages URL, request a sign-in link and follow it. Then in Supabase SQL Editor run:
   `update public.profiles set role='admin', approved=true where lower(email)=lower('YOUR_EMAIL_HERE');`
   Replace the placeholder with your actual admin sign-in email. Reload the app.
7. Players sign in via email; new registrations appear as Pending for you to approve.

## Roles
- Admin: manage fixtures, player approval and roles; see everyone's availability.
- Captain: see squad and all responses; submit own availability.
- Player: view fixtures and submit/view only own availability.

## Important caveats
- This package is ready to configure but **is not a live working deployment until you connect Supabase, configure email redirects and publish it**. It has not been tested against your actual Supabase project.
- The player **Remove** action removes the app profile and responses, not their Supabase Auth identity. To fully remove an identity, also delete it in Supabase Authentication > Users. If the same account signs in again after removal, an admin must restore its profile using Supabase tools.
- Manual player creation requires an invitation/sign-in first or manual account creation in Supabase Auth; this is not a full one-click admin invite feature.
- The fixture photo has been transcribed as best possible. Check opponent names and venues against the league's official fixtures before publishing.
- `fixtures.sql` contains 22 entries: 18 league matches and 4 cup placeholders.
- The app needs internet access. It uses Supabase JavaScript library from an external CDN.
- Supabase free-tier email sending may have rate limits; for a larger squad consider configuring a supported email provider.
- Do not put passwords, database secrets or service_role keys in GitHub files.
