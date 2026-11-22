# Mathematics Department Learning Hub — Setup Guide

## What this version does
- Public visitors can view Books, Notes and Question Bank links.
- You sign in through the Admin Dashboard to add, edit and delete links.
- You can update department name, YouTube channel name/link, logo URL and profile photo URL.
- Changes are stored in Supabase, so they are shared with every visitor (not just your phone).
- Visitors cannot write or delete content. Admin permission is controlled by the database.

## Important: it is not live until you connect and publish it
This package is the website code. To make shared online updates work, you need a Supabase project and a static website host (for example, Netlify or GitHub Pages). These services have their own account/setup steps.

## Step 1 — Create the database
1. Create a project at https://supabase.com/ and keep its project dashboard private.
2. Open SQL Editor → New query.
3. Copy all of `supabase_setup.sql` into the editor and run it.

## Step 2 — Configure the website
1. In Supabase, open Project Settings → API.
2. Copy the Project URL and the `anon` / publishable public key.
3. Open `index.html` in a code editor.
4. Find:
   `const SUPABASE_URL = "PASTE_YOUR_SUPABASE_PROJECT_URL";`
   `const SUPABASE_ANON_KEY = "PASTE_YOUR_SUPABASE_ANON_KEY";`
5. Replace those two placeholder values with your own values.
6. Never paste a `service_role` key, database password, or private secret into `index.html`.

## Step 3 — Create your own admin account
1. Run the website locally or publish it temporarily.
2. Open Admin Dashboard → enter your email and a strong password → Create account.
3. Verify your email if Supabase asks you to.
4. In Supabase Dashboard → Authentication → Users, find your account and copy its UUID.
5. Open SQL Editor and run this statement, replacing the example UUID:
   `insert into public.admin_users(user_id) values ('YOUR_AUTH_USER_UUID');`
6. Now sign in through Admin Dashboard. Only a user whose UUID is in `admin_users` can add/edit/delete website content.
7. Do not add other users to `admin_users` unless you want them to be administrators.

## Step 4 — Publish the website
Upload `index.html` to a static hosting service such as Netlify or GitHub Pages. Keep `supabase_setup.sql` and this guide private; they are setup documents, not website pages. Your public website URL will be provided by the hosting service.

## How you will control it from your phone
Open your website → Admin Dashboard → sign in.
- Books: choose Books when adding a resource.
- Notes: choose Notes.
- Question Bank: choose Question Bank.
- Edit/Delete: use the buttons beside each resource.
- Website settings: update the channel link/name, department name, logo URL and photo URL.
Use direct HTTPS links for PDFs/files. For logo/photo URL fields, use publicly accessible image URLs.

## Current image note
The logo and profile photo you supplied are embedded in the initial page. The Admin settings can replace them with public image URLs. A later upgrade can add direct image upload/storage.

## Security notes
- Do not share your admin password.
- Keep Supabase `service_role` keys and database passwords private.
- The public `anon` key is designed to be used in a frontend, but database Row Level Security (RLS) is essential; the SQL in this package enables RLS and limits writes to IDs listed in `admin_users`.
- Use a strong unique password and enable email verification.
