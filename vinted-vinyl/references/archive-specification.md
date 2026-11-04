# Vinted Vinyl Archive — storage specification v1

Store reusable listings and original photos in Google Drive. Use one folder per exact pressing, one text variant per language/grade pair, and one photo set per physical copy. Keep everything after a sale so another copy can reuse the text.

## Folder and filename contract

The root is Vinted Vinyl Archive, containing this archive-specification.md and Releases.

| Relative path under Releases | Purpose |
| --- | --- |
| `discogs-<release-id>__<catalog-slug>__<artist-title-slug>/release.json` | Exact pressing, verified facts, provenance, child folder IDs |
| `…/listings/nl-NL__M-M/title.txt` | One final buyer-facing title, UTF-8 |
| `…/listings/nl-NL__M-M/description.txt` | Complete final description, UTF-8, original line breaks |
| `…/listings/nl-NL__M-M/listing.json` | Language, grades, revision, readiness, text file IDs |
| `…/listings/nl-NL__M-M/price.json` | Optional dated prices and supporting evidence |
| `…/copies/copy-001/copy.json` | This physical item's grades, status, ordered photo IDs |
| `…/copies/copy-001/images/01-front.jpg` | Original front photo |
| `…/copies/copy-001/images/02-back.jpg` | Original back photo |
| `…/copies/copy-001/images/03-label-a.jpg` | Original label photo |

These are patterns, not empty sample listings. Create only files needed for actual records. Preserve actual image extensions/MIME types; never convert merely to match an example. Continue numbering for label B, surface, runout and defects; preserve the user's chosen order when supplied.

Normalize display-name slugs to lowercase ASCII with hyphens; manifests and IDs determine identity. Replace + with plus in grade path tokens: VG+/VG+ becomes nl-NL__VGplus-VGplus; retain real grades in JSON. Use pending-<uuid>__<slug> if exact identity is unresolved, with draft status. Never use a Discogs master ID as a release ID. Resolve a pending folder by renaming/updating it in place, preserving its ID.

## Common JSON rules

Use UTF-8 JSON with schema_version 1, integer revision starting at 1, RFC3339 created_at and updated_at in UTC. Use ISO dates for date-only events; do not invent historical precision. Unknown scalar facts and stock counts are null, never guessed zero. Empty arrays mean no recorded entries. Preserve unknown additive fields; ask before incompatible major migrations.

A file reference is {file_id, name, mime_type} using observed Drive metadata. A folder reference is {folder_id, name}. Save observed URLs if returned. Do not store credentials, tokens, temporary download URLs or buyer identity/address.

## release.json

Require release_key, identity_status (verified/draft), discogs_release_id (integer/null), artist, title, catalog_number, label, year, format, country, variant_notes, genres, tracklist, provenance, listings_folder_id, copies_folder_id, plus common fields.

Use discogs-<id> as verified release_key; otherwise retain the pending UUID. Optional unknowns remain null/empty arrays. Each track records {position, artist, title}; never guess tracks or positions. Provenance entries record {fields, source_type, source_url, observed_at, note}; source_type is discogs, official_source, seller_statement or own_photo. Research links stay here, outside buyer-facing text. Distinguish repress, color, promo and sleeve variants.

## listing.json

Require release_key, language, media_grade, sleeve_grade, condition_basis (user_default/seller_statement/mixed), status (draft/ready), title_file, description_file, source_copy_id (nullable), price_file (nullable), plus common fields. Ready means complete title/description read back for the stated grades; it does not imply photos or a current price.

Save the existing approved description format: uppercase release header and emoji; 1–2 intro lines; 💿 TRACKLIST; 🔊 DETAILS; ✨ CONDITIE; relevant hashtags. Preserve approved text exactly. Keep prices and sources elsewhere. Title contains only the title. Each language/grade combination has its own folder. For materially different same-grade wording, add __variant-02 and variant_note instead of overwriting copy-specific edits.

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

Search exact release ID first, then catalog/title within Releases. Read release.json before returning a match; names/catalogs alone are not unique. Follow pagination and current Drive IDs, not names alone. No global index is required; folder manifests remain authoritative.

Save text/photos first, manifests last. Recheck modifiedTime before updates; retain original file IDs with in-place replacement. After an ambiguous write, read back before retrying. Reuse verified partial uploads and report unresolved duplicates rather than deleting automatically. Keep sharing permissions unchanged.

Read-only retrieval never alters prices, stock or sales. A sold item stays archived. A new copy gets its own photos or explicit confirmation archived photos represent it accurately. Market refresh occurs only on request. Setup does not import historical chats or images automatically.

## Example requests

- “Bewaar dit zoekertje en deze foto's.”
- “Haal Pauper 09 uit mijn archief.”
- “Ik heb nog een exemplaar: geef de opgeslagen titel en beschrijving.”
- “Deze is VG+/VG+: pas alleen de conditie aan.”
- “Voeg deze foto's toe aan copy-002.”
- “Controleer alleen de opgeslagen prijs opnieuw.”
