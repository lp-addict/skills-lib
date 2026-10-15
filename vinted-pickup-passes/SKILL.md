---
name: vinted-pickup-passes
description: "Find Vinted purchases awaiting collection in connected Outlook email, exclude parcels before a user-defined arrival cutoff and those subsequently collected, deduplicate carrier notifications, extract tracking and pickup codes, and create a verified shareable PDF with ORIGINAL carrier-issued QR codes when available. Update the PDF when the user supplies an original QR/pass PDF. Supports Vinted Go, Mondial Relay and InPost notifications in multiple languages. Never fabricate a QR or a parcel status."
---

# Vinted Pickup Passes

Turn connected email notices into an accurate **ready-to-collect list and one shareable PDF** for a friend or other authorized collector. A short user request is sufficient; do not start with a form. Search first, reconcile all status updates, and deliver the finished artifact when the required tools are available.

## Activation and defaults

Invoke for "Vinted pickup", "packages waiting at a drop point", "make pickup QR PDF", "collect my parcels", or `$vinted-pickup-passes`.

- **Mailbox:** use the connected Outlook account when the user requests Outlook; do not search Gmail or another mailbox unless requested.
- **Cutoff:** honor the user's exact condition. For the recurring "since Monday after 6 pm" workflow, use **18:00 on the most recent Monday that has already reached 18:00**, in the user's local timezone (normally `Europe/Brussels`). Resolve the actual calendar date every run; never reuse a prior run's date. If no cutoff was given at all, propose this recurring default briefly or use it while explicitly stating the assumption.
- **Meaning of cutoff:** include parcels that **arrived / became collectable after the cutoff**, not merely reminders received after it. Prefer the carrier's stated arrival/ready time; use the first unambiguous ready notification timestamp if the actual arrival time is unavailable. Mark timing uncertainty rather than silently treating reminder time as arrival time.
- **Output:** a concise list in the user's language and an A4 PDF, titled with its preparation date, showing each distinct ready parcel, its collection details and any authentic carrier-issued QR.
- **Privacy:** pickup codes, tracking numbers and QR images are temporary access credentials. Use them only for the requested pickup pass. Never commit the live data, code, QR images, original emails or the resulting personal PDF to a skill repository, issue or public page.

## 1. Discover candidate incoming parcels

1. Get the current local date/time when relative dates or timezones matter, then calculate an explicit cutoff in local time and UTC for filtering.
2. Search connected Outlook over a window that captures the arrival notices and any newer status updates. Use broad Vinted searches plus carrier and notification-specific searches. Search both notification senders and subjects/body:
   - Vinted / Vinted Go: `ready for pickup`, `waiting for you`, `collect your parcel`, `order is ready`, `pick-up code`, `parcel has arrived`, `colis disponible`.
   - Mondial Relay / InPost: `votre colis ... est disponible`, `code de retrait`, `Point Relais`, `Punto Pack`, `ophaalcode`, `klaar om af te halen` and local-language equivalents.
   - Search `collected`, `picked up`, `retrieved`, `received your order`, `confirmation needed`, `delivered`, `retourné`, `returned`, `annulé`, `cancelled` across the relevant period too.
3. Fetch full message bodies for plausible candidates. A subject or body preview alone may hide the tracking ID, QR image URL, expiry date, bundle contents or latest state.
4. Distinguish the user's **purchases to collect** from their outgoing Vinted sales: messages such as "on its way to the buyer", seller shipping labels and proof of seller drop-off are not pickup candidates.
5. Search enough surrounding history to connect shipment notifications to order receipts and item/bundle contents, but never include an old parcel simply because you searched its history.

Do not interpret a search result count as a parcel count.

## 2. Reconcile identity, timing and collection status

Create an internal record per **physical parcel**, keeping evidence message IDs/URLs and timestamps. Capture when supported:

| Field | Rule |
| --- | --- |
| `tracking_number` | Exact carrier-issued value, preserving letters and leading zeros |
| `pickup_code` | Exact code as issued; do not substitute a tracking number |
| `carrier` | Vinted Go, Mondial Relay, InPost or other verified carrier |
| `items` | Full known item/bundle contents; one bundle in one shipment = one pickup |
| `pickup_point` | Exact shop name and street/address; separate similarly named points |
| `ready_since` | Actual pickup-ready timestamp/date with source and timezone confidence |
| `collect_by` | Carrier-issued deadline; do not guess a date |
| `hours` | Only hours found in the current source, with no "open now" claim |
| `qr_source` | Original carrier message image or user-supplied pass PDF/image; may be absent |
| `latest_status` | Ready, collected, returned, expired/unverified, in transit or ambiguous |

- Match statuses **first by tracking number or order/conversation ID**; use item name, seller, carrier, pickup location and chronology to corroborate. Never merge two shipments solely because they have the same pickup address or a generic item title.
- Deduplicate Vinted reminders, carrier emails and the original pickup notice into a **single parcel**.
- Look for **later confirmation of collection** from Vinted and/or the carrier, even if it does not use the word "pickup" in the subject. A confirmed collected/received parcel must be excluded. An ambiguous "delivered" status is not automatically a pickup confirmation without order context.
- Exclude parcels whose confirmed **arrival/ready time** precedes the cutoff. When timing evidence cannot distinguish sides of the cutoff, flag for verification rather than presenting as a definite match.
- Exclude parcels still in transit, cancelled or returned. If the stated collect-by date has passed and no fresh ready evidence exists, do not silently mark it ready; separate it as **expired / status unverified**.
- No pickup-confirmation email found means **"no matching confirmation found in the checked mailbox"**, not proof that nobody collected the parcel.
- If a notification indicates a bundle, look up the purchase receipt/order details and list **every verified item in that same parcel**, without duplicating the parcel count.
- Distinguish an **arrival date** from a **reminder date** and a **purchase date**. Preserve the source's date precision.

## 3. Use only real pickup QR codes

**Never generate a supposed carrier pickup QR by encoding a tracking number, six-digit code, carrier image URL, or redirect.** A QR pointing to a URL is *not* necessarily an actual pickup credential.

Preferred source order:

1. User-supplied original pickup-pass PDF or image.
2. Original QR image embedded in the carrier/Vinted email, if it can be accessed and verified.
3. Carrier-issued downloadable pass available via a safe, valid link.

When an original pass PDF is supplied:

- Verify its printed tracking number and pickup code match the candidate parcel **before** assigning its QR.
- Inspect/render the source page. Extract the **actual QR image** or safely crop it without introducing blur, clipping its quiet zone, stretching it or substituting a regenerated QR. Preserve readable contrast.
- Embed that original QR in the PDF card; also show the human-readable pickup and tracking numbers.
- Verify that the output displays the *original* QR at adequate size and visually matches the source. Decode/scan-check when a reliable decoder is available; do not claim a successful scan test unless one was actually run.
- If the QR image is inaccessible or cannot be verified, show the code and tracking number with an explicit **"original QR unavailable"** note. Do **not** make a fake QR to fill the space.

A PDF may contain parcels with QR codes alongside code-only parcels. Never invent a QR for Mondial Relay/InPost if the email supplied just a six-digit retrieval code.

## 4. Produce the pickup list and PDF

First give a concise, scannable list grouped by pickup location, covering **each parcel**: carrier, contents, point/address, exact pickup code, exact tracking number, collect-by date and whether an original QR is available. State the cutoff checked, and note any excluded/uncertain items only when it helps explain the result.

For the PDF:

- Prefer a compact, mobile-readable **A4 portrait layout**, using one card per physical parcel and as many pages as needed; never clip overflow to force one page.
- Group or sort by pickup address and earliest confirmed collection deadline. Make six-digit codes and the real QR visually prominent; keep long tracking values intact.
- Show provider, complete bundle contents, pickup point name + address, collection deadline, tracking number and pickup code. If any field is genuinely unavailable, omit it or label it unavailable; no fictitious placeholder values.
- Include brief collection instructions: scan/show the **original** QR where present, otherwise provide the actual pickup code. For collection by someone else, say the carrier/shop **may** request ID or authorization; do not promise delegation is accepted.
- Label data as "based on notifications, not a live carrier availability check" and include preparation date. Do not present old opening hours as live opening information.
- Use the available PDF tooling. Follow its **render -> inspect -> generate/edit -> re-render/verify** workflow. Check layout, unicode, code readability, QR quiet zone and that every included parcel appears exactly once.
- Save a real output PDF, verify the exact filesystem path exists, and link it in the final answer. If artifact creation or a required source fails, report the limitation rather than fabricate a download link.

## 5. Update an existing PDF when the user supplies QR material

When the user says "here is the original QR, update the PDF":

1. Read the new file, identify the parcel by exact tracking/pickup code and validate the correspondence.
2. Start with the most recent pickup list/PDF from this workflow; preserve all **other** verified parcels and their codes, tracking details and bundle contents.
3. Replace a former link-to-QR, missing-QR label or temporary image with the **original carrier-issued QR** itself. Avoid duplicate, conflicting QR images.
4. Re-render and inspect; verify QR size/readability and that the other parcel cards have not changed unintentionally.
5. Deliver the **new** PDF, clearly labeled updated. Do not claim that a generated URL QR is now the real carrier QR.

## 6. Final checks and error handling

Before delivery:

- [ ] Cutoff resolved from today's local date and applied to **arrival**, not only message receipt.
- [ ] Every included shipment has supported ready-to-collect evidence and no matching later pickup confirmation found.
- [ ] Outgoing sales, in-transit shipments, returns, expired/unverified arrivals and old parcels are not mislabeled ready.
- [ ] All duplicates collapsed by reliable parcel identity, with bundles represented as one parcel.
- [ ] Codes, tracking numbers, locations and deadlines checked against full source messages.
- [ ] Only authentic QR images shown; source PDF and decoded/printed identifiers agree.
- [ ] The PDF and chat list agree; PDF visually inspected; exact file path verified.
- [ ] No personal QR, tracking number, customer address or other secret uploaded to the GitHub skill library.

If mail access is unavailable, say so and ask for exported notifications or attachments; do not claim to have searched. If matches are ambiguous, give a small **needs verification** section rather than guessing. If no candidates pass the filter, say that clearly and do not produce an empty PDF unless requested.
