# Know Your Rights — Truth Teller Justice Library 

A free, state-by-state app with scripts for what to say during a traffic
stop, street stop, or home visit. Georgia is open as a free preview; other
states unlock with an email (feeds the Truth Teller email list).

## Deploy to Cloudflare Pages

1. Push this repo to GitHub (see steps below if you're new to that).
2. In the Cloudflare dashboard: **Workers & Pages → Create → Pages →
   Connect to Git** → select this repo.
3. Build settings:
   - **Build command:** `npm run build`
   - **Build output directory:** `dist`
4. Deploy. Cloudflare gives you a `*.pages.dev` URL immediately.
5. In the same project, go to **Custom domains → Add** and enter
   `rights.truthtellernews.com` (or whichever subdomain you want).
   Since your DNS already lives in Cloudflare, this connects automatically —
   no manual CNAME needed.

## Before real users see it

- Swap the placeholder Volume 4 link in `src/RightsApp.jsx` if it changes.
- Wire up the `TODO` in `handleUnlock()` to send captured emails to your
  actual email list provider (Klaviyo, Mailchimp, etc.) so signups feed
  your nurture sequence.
- Have an attorney in each state spot-check the scripts before wide launch —
  see the companion Legal Research Reference doc.
