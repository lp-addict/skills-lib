# Vinted Vinyl Archive — storage specification v1

Store reusable listings and original photos in Google Drive. Use one folder per exact pressing, one text variant per language/grade pair, and one photo set per physical copy. Keep everything after a sale so another copy can reuse the text.

## Folder and filename contract

The root is Vinted Vinyl Archive, containing this archive-specification.md and Releases.

| Relative path under Releases | Purpose |
| --- | --- |
| `<release folder>/release.json` | Exact pressing, verified facts, provenance, child folder IDs |
| `…/Listing.md` | Copyable Vinted text for this release |
| `…/listings/en-GB__M-M/title.txt` | One final buyer-facing title, UTF-8 |
| `…/listings/en-GB__M-M/description.txt` | Complete final description, UTF-8, original line breaks |
| `…/listings/en-GB__M-M/listing.json` | Language, grades, revision, readiness, text file IDs |
| `…/listings/en-GB__M-M/price.json` | Optional dated prices and supporting evidence |
| `…/copies/copy-001/copy.json` | This physical item's grades, status, ordered photo IDs |
| `…/copies/copy-001/images/01-front.jpg` | Original front photo |
| `…/copies/copy-001/images/02-back.jpg` | Original back photo |
| `…/copies/copy-001/images/03-label-a.jpg` | Original label photo |

These are patterns, not empty sample listings. Create only files needed for actual records. Preserve actual image extensions/MIME types; never convert merely to match an example. Continue numbering for label B, surface, runout and defects; preserve the user's chosen order when supplied.

Name each release folder `Release title — Artist [Catalogue]`, e.g. `Correction — Mark Archer [FLATDEEP 003]`. Use the familiar archived release title, retain readable spelling and omit unknown or redundant parts. For an unknown title, use the grounded label/catalogue or existing descriptive identification. For colliding pressings, append the known pressing detail or, if needed, a short stable key suffix. A draft keeps its pending UUID in release.json. Identify releases across both layouts by manifest key and Drive ID. Rename the existing folder in place when correcting its display name.

Keep language/grade folder tokens stable (e.g. en-GB__VGplus-VGplus). For every release save one UTF-8 `Listing.md` directly next to `release.json`. Use separate Title and Description headings with plain-text fenced blocks containing the exact saved wording and line breaks. Keep price, research, photo references and archive notes outside the copyable text. When there are multiple variants, group each pair under its own language, media/sleeve grades and variant or copy discriminator. Keep their order stable and retain all variants when editing one. For legacy drafts, show only available fields and label missing, partial, reformatted or transcribed text outside the blocks. When both fields are missing, provide a short status note. Preserve the original language when migrating.

Treat Markdown as a view of the current saved listing text. Keep the JSON manifests, individual text files and copy-specific photo sets. On saves, find the existing Markdown by parent and name, compare contents, and update it in place. Reconcile independent user edits before replacing them. Verify its parent and text against all intended variants. A read-only request makes no changes.

## Common JSON rules

Use UTF-8 JSON with schema_version 1, integer revision starting at 1, RFC3339 created_at and updated_at in UTC. Use ISO dates for date-only events; do not invent historical precision. Unknown scalar facts and stock counts are null, never guessed zero. Empty arrays mean no recorded entries. Preserve unknown additive fields; ask before incompatible major migrations.

A file reference is {file_id, name, mime_type} using observed Drive metadata. A folder reference is {folder_id, name}. Save observed URLs if returned. Do not store credentials, tokens, temporary download URLs or buyer identity/address.

## release.json

Require release_key, identity_status (verified/draft), discogs_release_id (integer/null), artist, title, catalog_number, label, year, format, country, variant_notes, genres, tracklist, provenance, listings_folder_id, copies_folder_id, plus common fields.

Use discogs-<id> as verified release_key; otherwise retain the pending UUID. Optional unknowns remain null/empty arrays. Each track records {position, artist, title}; never guess tracks or positions. Provenance entries record {fields, source_type, source_url, observed_at, note}; source_type is discogs, official_source, seller_statement or own_photo. Research links stay here, outside buyer-facing text. Distinguish repress, color, promo and sleeve variants.

## listing.json

Require release_key, language, media_grade, sleeve_grade, condition_basis (user_default/seller_statement/mixed), status (draft/ready), title_file, description_file, source_copy_id (nullable), price_file (nullable), plus common fields. Ready means complete title/description read back for the stated grades; it does not imply photos or a current price.

For new or rewritten listings, follow listing-format.md: English title and description; uppercase release header; plain TRACKLIST, DETAILS and CONDITION headings; `-` or `*` bullets; no emojis, intro filler or generic sales sentences; relevant hashtags. Save the final approved text exactly. Preserve existing language variants; do not rename Dutch folders or translate stored text during exact retrieval. Default new English variants to en-GB. Keep prices and sources elsewhere. Title contains only the title. Each language/grade combination has its own folder. For materially different same-grade wording, add __variant-02 and variant_note instead of overwriting copy-specific edits.

Retain source_copy_id to trace condition-dependent wording. Before reusing for another item, remove/adapt defects, test claims and sealed claims unless true of that item. M/M is Dave's drafting default, not inspection evidence.

## price.json

Require currency, asking_price, quick_sale_price, recorded_at, market_checked_at (nullable), price_basis (researched/seller_selected/imported), media_grade, sleeve_grade, confidence (low/medium/high/unknown), evidence, plus common fields.

Store numeric EUR amounts excluding shipping and buyer fees, or null. Preserve another known currency without treating it as EUR; conversion requires a checked rate/date. Do not invent quick-sale discounts. Evidence records available source, date, currency, grades and sold-versus-asking type. Saved values are historical; return the recording date and state they were not rechecked. Missing price data do not block text archiving.

## copy.json

Require copy_id, release_key, status (unknown/available/listed/sold/reserved), media_grade, sleeve_grade, condition_basis, condition_notes, quantity (integer/null), images_folder_id, images_status (missing/partial/complete), images, package_status (draft/text_only/partial/ready), listing_variant_folder_id (nullable), sold_at (nullable), sold_price (nullable), plus common fields.

Allocate copy IDs monotonically within each release after checking existing IDs. Quantity is 1 for an actually grounded physical item; otherwise null. Do not infer remaining stock from “a couple” or merge several physical items into one record. Condition_basis uses the same values as listing.json. Known defects remain attached to their item.

Each image records order, file_id, name, original_name, mime_type, sha256 (nullable when bytes unavailable), role, capture_copy_id, reuse_confirmed_for (copy-ID array), and provenance_note. Roles include front, back, label-a, label-b, vinyl, runout, defect, other. Save original bytes. A link alone does not count as saved bytes. SHA-256 deduplicates retries within a photo set, not physical items. Retain capture_copy_id even if the user confirms reuse for a new item.

Complete images means all supplied/requested images were saved and verified, not guaranteed visual coverage. Ready package means complete text for its condition plus verified usable photos. Text without photos is text_only; photo-only/other incomplete sets are partial. Missing content is reported, never filled with placeholder files.

## Lookup, updates and recovery

Match exact release IDs in the manifests. Use artist, catalogue and title to find candidates in Releases, and scan remaining manifests when a name search misses. Read release.json before returning a match; names/catalogs alone are not unique. Follow pagination and current Drive IDs, not names alone. No global index is required; folder manifests remain authoritative.

Save text/photos first, manifests last. Recheck modifiedTime before updates; retain original file IDs with in-place replacement. After an ambiguous write, read back before retrying. Reuse verified partial uploads and report unresolved duplicates rather than deleting automatically. Keep sharing permissions unchanged.

Read-only retrieval never alters prices, stock or sales. A sold item stays archived. A new copy gets its own photos or explicit confirmation archived photos represent it accurately. Market refresh occurs only on request. Setup does not import historical chats or images automatically.

## Record text prepared for posting

When Dave requests a Vinted title and description to copy, post or relist, including a short release/grade follow-up in that context, record the final text in the listing register before delivering it. “Just the title and description” limits the reply to those two fields. Browsing, historical inspection, a preview, or an explicit no-save/read-only request skips the register write. Apply this to retrieved, newly drafted and revised posting text, once per completed listing in a batch.

Use the configured archive account and the existing Vinted Sales Ledger selected by the archive owner. Resolve the listing-register tab from the owner’s verified configuration and check its current headers. If the ledger or tab is ambiguous, ask for its location before writing. Read the populated rows before writing. Map fields by header:

| Header | Value |
| --- | --- |
| Release | Human-readable release name |
| Vinted titel | Exact title delivered to Dave |
| Vinted beschrijving | Exact description delivered, with original line breaks |
| Conditie | Grades or condition supplied for this listing, retaining uncertainty |
| Archief | Verified Drive link for the physical copy if identified, otherwise its listing variant or release folder |
| Prijsnotitie | An explicitly supplied asking price or an applicable saved asking price with its date and basis. Preserve a relevant existing note; leave unknown prices blank. |

Match by archive ID and the applicable copy or language/condition variant. Accept an older release-folder link when it resolves to the same release and the text, language and grades identify the same listing. Reuse its row. Another identified physical copy or distinct variant gets its own row with its own archive link. If a legacy row could belong to either copy, leave it intact and use a distinct verified copy link for a genuinely new copy. Defer an ambiguous match if the intended entry cannot be identified. Never match on title alone.

Read the matching row again before a change. Skip identical rows. Update only the changed listing fields, preserving manual edits, formulas, notes and other columns; reconcile conflicting wording before replacing it. On a miss, rescan the key and append one row using literal cell strings so punctuation or an initial equals sign cannot become a formula. Verify the unique match and read back the exact title and description, condition and archive link. Recheck after an uncertain write before retrying. If a duplicate or a failed write remains, give Dave the usable text and a short, accurate warning outside the copyable blocks.

A register row means text was prepared. Publication, inventory and transactions require separate evidence. Leave Orders, Order Items, Events, Item Prices and financial totals untouched during this operation. A normal retrieval-to-post reads the existing archive; creating a new physical copy or editing the archive still follows the applicable archive workflow.

## Example requests

- “Bewaar dit zoekertje en deze foto's.”
- “Haal Pauper 09 uit mijn archief.”
- “Ik heb nog een exemplaar: geef de opgeslagen titel en beschrijving.”
- “Deze is VG+/VG+: pas alleen de conditie aan.”
- “Voeg deze foto's toe aan copy-002.”
- “Controleer alleen de opgeslagen prijs opnieuw.”
