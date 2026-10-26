# Frost & Scoops — PC image upload upgrade

Premium responsive ice-cream storefront with Supabase-backed product management and WhatsApp checkout. Admins can select an image from their computer, preview it, and upload it to Supabase Storage while saving a product.

## Files
- `index.html` — storefront and admin dashboard
- `css/style.css` — responsive design
- `js/app.js` — Supabase Auth, product CRUD, image upload, cart and WhatsApp checkout
- `supabase/schema.sql` — products/admin tables, RLS policies, and `product-images` Storage bucket policies

## Setup in VS Code
1. Extract the project ZIP and open its folder in VS Code.
2. Open `js/app.js`. Replace `PASTE_YOUR_SUPABASE_PROJECT_URL_HERE` and `PASTE_YOUR_SUPABASE_ANON_KEY_HERE` with the **Project URL** and **anon/public (publishable) key** from your Supabase project settings. Never use the service-role/secret key in frontend code.
3. In Supabase Dashboard, open **SQL Editor → New query**, paste all of `supabase/schema.sql`, and run it.
4. In Supabase Dashboard → **Authentication → Users**, create an admin user with an email and strong password. Copy that user's UUID.
5. In SQL Editor, run the following after replacing the placeholder with the real UUID:
   ```sql
   insert into public.shop_admins (user_id)
   values ('PASTE-AUTH-USER-UUID-HERE');
   ```
   If the user is already in `shop_admins`, do not insert it again.
6. Install the **Live Server** extension in VS Code. Right-click `index.html` → **Open with Live Server**.
7. Click **Admin login**, enter the same email/password created in Supabase, and add a product. Select **Choose image from PC** to preview a local image; when you save, it uploads to the `product-images` Storage bucket.

## WhatsApp
The current number is configured as `923266343908`, derived from the provided local number `03266343908` (digits only, country code included). Verify this is the intended number in `js/app.js` before publishing. WhatsApp opens a prefilled order; the customer must tap Send.

## Notes
- Product images must be JPG, PNG, WEBP or GIF and 5 MB or smaller.
- The bucket is public for image display, but upload/update/delete permissions are restricted to admin users by Storage policies.
- If admin sign-in fails, check that the user exists in Supabase Auth, its UUID is listed in `shop_admins`, and the project URL/key are correct.
- Without Supabase configuration, the storefront shows demo products; admin changes and online image uploads are disabled.
- This frontend sends orders through WhatsApp; it does not persist orders in a database.
