# Cotopia — Privacy Policy

**Effective date:** October 2025  
**Contact:** **Support@cotopia.org**

Introduction
------------
Cotopia ("we", "us", "our") provides the Cotopia QR & SKU Maker / Scanner web application and related services (the "App"). This Privacy Policy explains what personal data we collect, how we use it, share it, protect it, and your rights. This Policy applies to data collected when you use the App, our partner portal, or interact with us as a Distributor, Customer, or visitor.

1. Data we collect
------------------
We collect data necessary to provide the App, support partners and Customers, and comply with legal obligations. Data categories include:

- **Account & contact data:** name, company name, role/title, email address, phone, billing address, payment method metadata (payments processed via third-party gateways), tax identifiers (where applicable), and other information supplied during registration or onboarding.  
- **Customer & distributor data:** company identifiers, reseller relationships, Product Agreement metadata, Territory, records of purchases, invoices and transaction history.  
- **Usage & telemetry:** anonymized or pseudonymized logs, feature usage (e.g., QR generation counts, SKU import activity, file chunking events), API request metadata, timestamps, IP addresses, device/browser user agent strings, and error/exception traces. We use usage data to operate, improve, and secure the App.  
- **Uploaded files & payloads:** when using the App, users may upload product files, images, logos, CSVs or other data which may contain personal data (e.g., contact info embedded in vCards). Uploaded content is stored per your selected workflow (client-file-only, host on Distributor server, or Cotopia-hosted storage depending on Product Agreement and integration).  
- **Support & communications:** support tickets, emails, recordings or transcripts of support calls, and troubleshooting logs provided to support teams.  
- **Payment & billing:** payment transaction records, invoices and related billing communications. We do not store full payment card numbers — payment processing is handled by third-party payment processors (see Section 7).  
- **Legal & compliance:** KYC/AML, tax documentation, W‑9/W‑8 or other forms provided by distributors or recruited distributors as required by Cotopia or law.

2. How we collect data
----------------------
- **Directly from you:** when you register, sign a Product Agreement, upload files, contact support, or use the App’s UI controls (e.g., CSV SKU import).  
- **Automatically:** logging and telemetry capturing usage events, device details, IP addresses and error reports.  
- **From third parties:** payment processors, analytics providers, or data you direct us to import (for example when you provide a CSV of SKUs or contact data for onboarding).

3. Purpose & lawful bases for processing
---------------------------------------
We process personal data for the following purposes:

- **Service provision:** to provide, operate, maintain, and improve the App. (Legal basis: performance of contract.)  
- **Billing & collections:** to invoice and collect fees, comply with tax obligations and refunds. (Legal basis: performance of contract and legitimate interest.)  
- **Support & troubleshooting:** to respond to inquiries, provide Included Tech Support to Distributor personnel, and resolve incidents. (Legal basis: performance of contract and legitimate interest.)  
- **Security & fraud prevention:** to detect and prevent abuse, unauthorized access and attacks. (Legal basis: legitimate interest and legal compliance.)  
- **Legal compliance:** to respond to lawful requests, enforce agreements, retain records as required by law. (Legal basis: legal obligation.)  
- **Analytics & product improvement:** to analyze usage patterns and improve product features. (Legal basis: legitimate interest; we seek to pseudonymize or aggregate telemetry where feasible.)  
- **Marketing & communications:** to send service-related notices and (with consent where required) promotional communications. You can opt out of marketing emails. (Legal basis: consent for marketing; performance of contract for service notices.)

4. Data retention
-----------------
We retain personal data as follows:

- **Account & billing records:** retained for the duration of the contractual relationship and as required for tax, accounting and legal compliance (typically 7 years unless otherwise required by law).  
- **Usage logs & telemetry:** retained for operational and improvement purposes — retention period varies (e.g., 90–540 days) depending on data type and troubleshooting needs; aggregated telemetry may be retained longer.  
- **Support records:** retained for the life of the account plus a period for quality and legal purposes (typically 2–7 years).  
- **Uploaded files & manifests:** retention depends on hosting option and Product Agreement. If Cotopia stores files on behalf of a Distributor or Customer, retention is per the selected storage policy; you may request deletion per Section 9.

5. Data sharing & disclosures
-----------------------------
We do not sell personal data. We may share data as follows:

- **With service providers:** payment processors, hosting/CDN providers, analytics and monitoring tools, email delivery services, and support tooling providers. We require these providers to process data only per our instructions and under security controls.  
- **With Distributors & Customers:** where permitted by your Product Agreement, uploaded files, manifests or SKU lists may be shared with authorized Distributor personnel or Customers as part of delivery workflows. Distributors are responsible for their Customers’ data and must comply with applicable law and the DPA where executed.  
- **For legal reasons:** to respond to lawful requests, to comply with subpoenas, court orders or government requests, to protect the safety of users, or to enforce our agreements.  
- **Business transfers:** in connection with a merger, sale, or corporate reorganization, provided we give notice or use other lawful means to protect personal data.

6. International transfers
--------------------------
Cotopia is based in the United States (Wyoming). Data we collect may be stored and processed in the U.S. or other countries where our service providers operate. When transferring personal data from the EEA/UK or other jurisdictions with data transfer restrictions, we will use appropriate safeguards (e.g., EU Standard Contractual Clauses) or rely on an adequate transfer mechanism where applicable. Distributors processing data must ensure lawful transfer mechanisms for their Customers' data.

7. Payment processing
---------------------
Payments and card data are processed by third-party payment processors (e.g., Stripe, PayPal) under separate terms and privacy practices. We do not store full card numbers on our systems. Billing metadata and transaction records are stored by Cotopia for invoicing and accounting.

8. Security measures
--------------------
We implement reasonable technical and organizational measures to protect personal data, including HTTPS/TLS encryption in transit, encryption at rest where appropriate, access controls, secure development practices, regular vulnerability testing, logging and monitoring. However, no internet service is completely secure; we cannot guarantee absolute security. If we become aware of a data breach affecting personal data, we will notify affected parties and regulators as required by law and the Product Agreement.

9. Your rights & choices
------------------------
Subject to applicable law, you may have the right to:

- **Access:** request a copy of personal data we hold about you.  
- **Rectification:** request correction of inaccurate data.  
- **Deletion:** request deletion of your personal data (subject to retention obligations and limitations where necessary for legal compliance or contractual performance).  
- **Restriction & portability:** request restrictions on processing or portability of data you provided.  
- **Objection:** object to processing where we rely on legitimate interests (you may request restriction instead).  
- **Marketing opt-out:** you can opt out of marketing emails via the unsubscribe link or by contacting Support@cotopia.org.

To exercise rights, contact Support@cotopia.org. We may require identity verification and may need to refuse requests that are manifestly unfounded, excessive, or conflict with legal obligations.

10. Data for uploaded files, manifests & chunking
-------------------------------------------------
The App supports embedding small files in QR payloads and creating manifests + ZIPs or chunked parts for larger files. Important privacy considerations:

- If you embed personal data within QR payloads, that data may be publicly readable by anyone who scans the QR unless you implement encryption or access controls. Do not embed sensitive personal data in public QR payloads.  
- If files are hosted by Cotopia on behalf of a Distributor or Customer, Cotopia will store them per the selected hosting/storage settings and Product Agreement. Distributors must ensure they have necessary consents to upload personal data and must inform Customers how their data is stored and shared.  
- Chunked parts and manifests: parts are binary slices of files; `manifest.json` contains metadata such as SHA‑256 checksums. Manifests do not by default contain personal data unless included in the original files or metadata provided by you.

11. Data Processing Addendum (DPA)
----------------------------------
If Cotopia processes personal data on your behalf (controller → processor relationship), the Parties will execute a DPA (Exhibit E) that sets out processing details, subprocessors, security measures, international transfers, and deletion/return obligations. Contact **Support@cotopia.org** to request the DPA.

12. Minors
----------
The App is not directed to children under 16. We do not knowingly collect personal data from minors. If we learn that we have collected personal data of a minor without consent, we will take steps to delete it.

13. Third‑party services & links
-------------------------------
The App may include links to third-party sites, or integrate third‑party services and open-source components. These third parties have their own privacy practices; Cotopia is not responsible for their content or policies.

14. International partners & distributors
-----------------------------------------
Distributors who recruit downstream distributors or host customer data across jurisdictions must ensure compliance with applicable data protection laws (e.g., GDPR, UK GDPR, CCPA/CPRA). Cotopia will cooperate to provide information required by applicable law to the extent possible; Distributors remain responsible for Customer communications and lawful bases for processing unless otherwise agreed in a DPA.

15. Changes to this policy
--------------------------
We may update this Privacy Policy to reflect changes in our services, legal requirements, or business practices. For material changes, we will provide notice via the App or email where practicable. Continued use after notice constitutes acceptance of the updated policy.

How to contact us
-----------------
For privacy inquiries, data subject requests, DPA requests, or other questions contact: **Support@cotopia.org**

Mailing address: Cotopia, 30 Gould St Ste R, Sheridan, WY 82801

_Last updated: October 2025 · © Cotopia_