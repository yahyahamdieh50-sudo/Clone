# YAHIVO Store Audit — 2026-10-13

Store: `gi5nwr-rh.myshopify.com` → www.yahivo.com · Plan: Basic · Currency: AED · Market: UAE only
Access method: the authenticated Shopify Admin API connector (OAuth, scoped token). No passwords or API keys were shared or stored.

Limit of this audit: this environment's network blocks the storefront (www.yahivo.com), so pages could not be loaded, screenshotted or speed-tested. Every finding below comes from the Admin API and from reading the theme source. Test the storefront items in the checklist on a real phone.

---

## 1. Backups (done before any change)

| Backup | Where |
|---|---|
| Full source of the live theme `yahivo-luxury-theme` (#165189681411), 41 files | `shopify-backup/yahivo-luxury-theme-165189681411_2026-10-13/` (Liquid/CSS checksums match Shopify's exactly) |
| In-Shopify copy of the live theme, which also holds the fixes | Theme **"YAHIVO Fixes 2026-10-13 (staging)"** (#165296603395), unpublished |
| Products, collections and pages as they were before any edit | `shopify-backup/store-data-before-2026-10-13/products-collections-pages.json` |

The live theme was **not** edited. Shopify blocks API writes to the published theme, which is also the safer way to work.

---

## 2. What was wrong → what was fixed

### Fixed in the staging theme (you need to preview it, then publish it)
| # | Problem | Fix | File |
|---|---|---|---|
| T1 | **No menu on mobile.** Below 900px the nav was hidden with nothing replacing it, so phone users could not reach Hoodies, T-Shirts, Sweatshirts or Contact. | Added a hamburger drawer (works without JavaScript) that also links Search and Account. Hid the "Shop now" button below 480px so the header fits. | `sections/header.liquid` |
| T2 | **Contact page blank, with no form.** The page is assigned the `contact` template, which didn't exist in the theme. | Added a contact form (name, email, phone, message, plus success and error states) styled to match the theme. | `sections/main-contact.liquid`, `templates/page.contact.json` (new) |
| T3 | **No social-share tags.** Links shared on WhatsApp, Instagram or Facebook showed no image or title. | Added Open Graph and Twitter tags (title, description, image, price). | `layout/theme.liquid` |
| T4 | **Variant selector bugs.** Picking a colour didn't change the image, the URL didn't update (so a shared link opened the wrong variant), and the sale price was lost on change. | Fixed all three. | `sections/main-product.liquid` |
| T5 | Leftover collection "General Clothes example products" appeared on `/collections`. | Hidden from the collections list. | `sections/main-list-collections.liquid` |
| T6 | Theme Check: 8 errors (images without width/height cause layout jumps on mobile). | Fixed 1 here (product thumbnails). The other 7 are fixed in the local copy but **not uploaded yet** (see 3-H). | — |

All uploaded files passed Shopify Theme Check with no new errors, and each file was confirmed byte-identical after upload.

### Fixed live in the store (low risk, reversible from the snapshot)
| # | Problem | Fix |
|---|---|---|
| S1 | Footer menu held only "Search": no policies and no contact link. | Footer now has: Contact Us, Refund Policy, Privacy Policy, Search. |
| S2 | Contact page had no content. | Added email, phone and location, with a reply-time promise of one business day. |
| S3 | 30 of 30 products had no SEO title or meta description. | Wrote unique SEO titles that include colour, plus 100–130 character descriptions for all 30. |
| S4 | 30 of 30 products had no product type (the product page showed the generic "Streetwear"). | Set Hoodie (24), T-Shirt (5), Sweatshirt (1). |
| S5 | 89 product images had no alt text (hurts SEO and accessibility). | Added descriptive alt text to all 89. |
| S6 | Collections had no SEO title or description. | Added both for Hoodies, T-Shirts and Sweatshirts. |

---

## 3. Needs your approval (not changed)

**A. CRITICAL: carts over 2 kg cannot check out.** All 182 variants are in the shipping profile **"Tapstitch: Special Line"**. Its UAE rates are weight brackets from 0 to 2,000 g only (USD 4.10–14.94). Hoodies weigh 0.70–0.95 kg, so **any cart with 3 or more hoodies has no shipping rate and checkout stops**. Two more issues on the same profile:
- The free-shipping-over-AED-200 rate in your General profile never applies, because no product is assigned to that profile.
- Customers see the rate as "Special Line", which reads like supplier jargon.

Options: (1) move the products into the General profile and use the AED 25 / free-over-AED-200 rate, or (2) add a rate above 2 kg to the Tapstitch UAE zone and rename it "Standard Delivery (UAE)". Check option 1 with Tapstitch first; their app may rely on the profile. **This is a shipping-rate change, so it needs your approval.**

**B. Supplier name shown publicly.** 29 of 30 products have vendor = **"ODMPOD"**. Google Shopping, Facebook/Instagram and TikTok feeds show it as the brand. Recommendation: change it to **YAHIVO**, after confirming with ODMPOD/Tapstitch that their sync doesn't use the vendor field.

**C. Collections are scrambled.** Hoodies (30) contains every T-shirt. T-Shirts (22) contains hoodies and sweatshirts. Sweatshirts (26) contains T-shirts. Proposed fix:
- Hoodies: hoodies only
- T-Shirts: the 5 tees only
- Sweatshirts: the mock-neck plus the fleece hoodies, or the mock-neck only (your choice)

Unpublish "General Clothes example products". Nothing gets deleted.

**D. Duplicate product names.** 12 products are called "Oversize Fleeced Hoodie", 5 "Essential Heavyweight Drop-Shoulder Hoodie" and 3 "Essential Cotton T-Shirt". Five of them are Black at the same price. Customers can't tell them apart, and Google may treat them as duplicates. Recommendation: rename each one by its design (for example "Shadow Oversized Hoodie – Black"). I need you to name the designs, or approve names I draft from the photos. Or merge true duplicates into one product with colour variants (merging involves deleting products, so it needs explicit approval).

**E. Prices look like currency conversions.** Examples: AED 153.52, 131.56, 87.93, 73.30, 212.81. Recommendation: round to brand price points (149 / 129 / 89 / 75 / 209 AED). The mock-neck sweatshirt has two prices, with XL at 168.69 vs 159.62.

**F. Missing policies.** Terms of Service, Shipping Policy and Contact Information are not set. A shipping policy is required for Google Merchant Center and expected by UAE shoppers. The refund policy is generic ("Return eligibility depends on the items in your order"). I can draft all four (Settings → Policies) for your review.

**G. Checkout, payments and taxes. Verify these in admin; the API can't read them.**
- Settings → Payments: confirm a provider is active and Apple Pay / Google Pay are on. The API reports no digital wallets.
- Settings → Taxes: prices include tax and the UAE market is active. Confirm VAT 5% registration (TRN) if you are registered.
- Place one real test order on a phone (see the checklist).

**H. Theme items for later.**
- Upload the 7 remaining image-dimension fixes (house-outro, article, cart, scroll-story, product-card). They're ready locally.
- Design direction: the theme is "Luxury Bespoke", with gold serif type (Cormorant Garamond) and copy like "Bespoke Streetwear" and "The House of YAHIVO". For a masculine streetwear look I'd recommend a bold grotesk or condensed sans for headings, off-white instead of gold accents, and "Bespoke" replaced (it's print-on-demand, not bespoke). This is a visual redesign, so it needs your approval.
- Google Fonts load from an external server (Theme Check warning). Moving them to Shopify-hosted fonts improves speed.
- No favicon is set (Theme settings → Logo/Favicon).

**I. Theme clean-up.** There are 8 themes, including 3 copies named "yahivo-luxury-theme" plus Horizon, Whisper, "Updated copy of Horizon" and an Allure demo. I recommend deleting the unused ones once the staging theme is published. Deletion needs your OK.

**J. Inventory.** Variants show about 10 million stock with "continue selling" on. That's normal for print-on-demand, but low-stock badges will never appear. No action needed.

---

## 4. Links

- Preview the fixed theme: https://gi5nwr-rh.myshopify.com/?preview_theme_id=165296603395
- Edit the fixed theme: https://admin.shopify.com/store/gi5nwr-rh/themes/165296603395/editor
- Themes list (publish from here): https://admin.shopify.com/store/gi5nwr-rh/themes
- Shipping profiles: https://admin.shopify.com/store/gi5nwr-rh/settings/shipping
- Policies: https://admin.shopify.com/store/gi5nwr-rh/settings/legal
- Payments: https://admin.shopify.com/store/gi5nwr-rh/settings/payments

---

## 5. Final checklist before launch

- [ ] Preview the staging theme on a phone: open the ☰ menu, open each collection, open a product, change colour and size, add to cart.
- [ ] Contact page: send a test message and confirm it reaches yahia.h2026@hotmail.com.
- [ ] Publish "YAHIVO Fixes 2026-10-13 (staging)" (Themes → Publish).
- [ ] **Fix the shipping profile (3-A)**, then test a 3-hoodie cart through to the shipping step.
- [ ] Approve or reject: vendor → YAHIVO (B), collection clean-up (C), product renames (D), price rounding (E).
- [ ] Add Terms of Service, Shipping Policy and Contact Information policies (F), then add them to the footer menu.
- [ ] Confirm payments are active, including Apple Pay / Google Pay, and confirm VAT settings (G).
- [ ] Place one real order with a card, then refund it.
- [ ] Set a favicon. Share a product link on WhatsApp and check that the preview shows the image.
- [ ] Submit `https://www.yahivo.com/sitemap.xml` in Google Search Console.
- [ ] After launch, delete the unused themes (I).
