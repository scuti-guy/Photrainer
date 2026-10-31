# Photrainer — Email-only mobile PWA

Your Supabase URL and **publishable** key are already in `config.js`. Google and Apple login have been removed. Existing camera, gallery, missions, XP, Credits, profiles, studio, shop and photo editor remain.

## REQUIRED: Supabase configuration (one time)

1. In Supabase open **Authentication → Providers → Email**. Enable Email and **turn OFF “Confirm email”**. Save. This lets a new account sign up and enter immediately. Do not enable this for high-risk apps without reviewing abuse prevention and email ownership implications.
2. Open **SQL Editor**, paste and run `supabase/schema.sql` once on this project. It creates account records, photo records, private storage and per-user access policies. If you've already run this SQL on this project, don't run it again; policies may already exist.
3. Under **Authentication → URL Configuration**, set Site URL to your actual deployed HTTPS URL (not the Supabase URL).
4. Upload this project's files to an HTTPS static host such as GitHub Pages or Netlify. For a quick local preview, run `python -m http.server 8080` from this folder, then visit `http://localhost:8080`.
5. To enable **Delete Account**, deploy `supabase/functions/delete-account/index.ts` using Supabase CLI (see below). The button will not work until deployed.
6. Open the HTTPS website in Chrome on Android and select **Install app** or **Add to Home screen**. On iPhone use Safari → Share → Add to Home Screen.

## Deploy delete-account function

```sh
supabase login
supabase link --project-ref qfvrptumbqifjbqgbznv
supabase functions deploy delete-account
```

This function verifies the signed-in user and removes their active photos/profile files and account. Supabase backups can have separate retention. Test deletion before making the app public.

## Notes

- Photos are uploaded to a private Supabase Storage bucket and are NOT intentionally saved to the phone gallery. They persist across devices as long as the account and cloud data remain, subject to storage quotas, service availability, and the provider's retention policy.
- Email confirmation is disabled for instant sign-up, so an account's email address is **not verified**. Consider rate limits, CAPTCHA, and email verification before public launch.
- The economy is a prototype: the browser calculates mission rewards and credits, so users can manipulate them. Move XP and credit transactions to server-validated logic before running public competitions.
- This app requires internet for authentication and cloud photos. It uses a service worker for the installable app shell.
- Account data is private by default under RLS. No public social profiles or sharing have been implemented yet.
