# API-Hole-PoC
🔎 What is CVE-2025-13606

CVE-2025-13606 affects the WordPress plugin Export All Posts, Products, Orders, Refunds & Users (versions up to and including 2.19). 
CVE Details
+2
SecurityVulnerability.io
+2

The vulnerability is classified as a CWE-352 — a Cross-Site Request Forgery (CSRF) issue. 
OpenCVE
+1

The root cause: the plugin’s “export” functionality (the function named parseData) lacks correct “nonce validation”. In other words, it fails to verify that export requests come from a legitimate user session. 
SecurityVulnerability.io
+1

⚠️ Why it matters / What’s the impact

Because of this flaw:

An attacker — even if they are not authenticated — could trick a site administrator (or any user with sufficient privileges) into triggering a request (for example, via a crafted link). If that user clicks the link, the plugin will treat it as a valid export request. 
SecurityVulnerability.io
+2
OpenCVE
+2

As a result, the attacker could export sensitive data from the WordPress site: user data, email addresses, password hashes, and e-commerce data (orders, products, refunds, etc.). 
CVE Details
+1

The exported data could then be saved to a file path controlled by the attacker, giving them direct access to downloaded site data. 
SecurityVulnerability.io
+1

Because the vulnerability needs no authentication (just a click by an admin), it's relatively easy to trigger — especially if combined with phishing or social-engineering. 
OpenCVE
+1

In security-rating terms:

The CVSS v3.1 base score is 6.5 (Medium). 
OpenCVE
+1

Confidentiality impact is rated High (because private user / order data can be exposed). 
OpenCVE
+1

Integrity and availability impacts are rated low/none (the site doesn’t necessarily get defaced or taken down). 
CVE Details
+1

So this vulnerability is not about defacing the site — it’s about silent data exfiltration, which is extremely dangerous for e-commerce, user privacy, and compliance (e.g. data protection laws).

🧠 How it works (technical breakdown)

The plugin offers a feature “Export All Posts, Products, Orders, Refunds & Users” — presumably to let admins export their site data.

Normally, secure WordPress plugins use a “nonce” (a one-time token) to ensure that export requests come from a valid session/form, not a forged request.

In this plugin version, the function handling export (named parseData) does not verify the nonce (or does so incorrectly). That means any request to “export” — even from an unauthenticated external site — might be accepted. 
SecurityVulnerability.io
+1

An attacker creates a link or form on their own site, targeting the export endpoint of an affected site.

If an admin of that site clicks that link (or visits that page while logged in), the export runs and site data is compiled and sent — potentially to a location the attacker controls.

The attacker then retrieves the exported data: user lists, email addresses, order history, perhaps password hashes, etc.

Because this requires only a single admin click, and no prior authentication, it's a serious vulnerability even if “only” medium severity on paper.

✅ What this CVE does not do (by itself)

It does not execute code on the WordPress server (not a remote code execution). It’s about data export / leak, not code execution.

It does not necessarily break site availability or integrity — the site may continue functioning normally.

It does not require the attacker to have credentials, but it does require some user interaction (click by admin). 
CVE Details
+1

🎯 Who is at risk

This vulnerability is especially dangerous for:

Sites using the vulnerable plugin version (≤ 2.19).

E-commerce sites using WooCommerce or similar, since order, product, and user data may be exposed.

Any site with many users or customers — leakage of email addresses, order history, or personal data can defraud users, violate privacy laws, damage reputation.

Organizations under data-protection regulations (GDPR, etc.), because unauthorized data leaks may cause legal consequences.

🔧 What should site owners or plugin authors do (in principle)

Update the plugin immediately if a patched version is released.

If no patch exists yet, consider disabling or uninstalling the plugin until fixed.

Review recent export logs — check for unusually large exports or unknown file exports.

Ensure that export functionality (or any admin-level action) is protected by nonce + capability checks.

Restrict export endpoints to trusted IPs, trusted sessions, or authenticated users only.
