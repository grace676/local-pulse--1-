# Implementation: Delivery Confirmation & Dispute Flow

## Steps

### Step 1: Database Schema Updates ✅

- [x] Add `delivered_at`, `dispute_window_end` columns to `orders` table
- [x] Add `reason_category` column to `disputes` table
- [x] Create `order_status_log` table for audit trail
- [x] Add `reason_category` enum type
- [x] Add indexes for `order_status_log`

### Step 2: Backend - `transactions.routes.js` Enhancements ✅

- [x] Auto-set `delivered_at` + `dispute_window_end` when status → `delivered`
- [x] Log every status change to `order_status_log`
- [x] Freeze disputed orders (no transitions allowed)
- [x] Sequential transition enforcement (already existed)

### Step 3: Backend - `disputes.routes.js` Enhancements ✅

- [x] Accept `reasonCategory` in POST, validate against allowed categories
- [x] Enforce dispute window check (reject if past `dispute_window_end`)
- [x] On resolution: map outcome to order status (`favor_buyer`→`cancelled`, `favor_artisan`→`completed`)
- [x] Log order status change on resolution

### Step 4: Backend - Evidence Upload Endpoint ✅

- [x] Add `POST /disputes/:disputeId/evidence` endpoint with multer
- [x] Handle multipart file upload, store in `/uploads/dispute-evidence/`
- [x] Append file URL to `evidence_urls` on dispute
- [x] File type validation (images, videos, PDFs, docs)
- [x] 10MB file size limit
- [x] Reject evidence on resolved disputes

### Step 5: Backend - Auto-Confirm on Expiry ✅

- [x] Add `POST /disputes/auto-confirm-expired` endpoint (admin-only)
- [x] Query for delivered orders past `dispute_window_end`
- [x] Auto-confirm them with `delivery_confirmed_at` preserved

### Step 6: Frontend - `constants/index.js` Updates ✅

- [x] Add `REASON_CATEGORIES` constant with display labels
- [x] Add `REASON_CATEGORIES_MAP` for lookup
- [x] Add `DISPUTE_WINDOW_HOURS` constant (72h)

### Step 7: Frontend - `OrdersPage.jsx` Enhancements ✅

- [x] Buyer: Show "Confirm Receipt" + "Report a Problem" buttons on Delivered
- [x] Inline dispute filing modal with reason category dropdown
- [x] Freeze actions when disputed
- [x] Evidence URL upload in dispute form

### Step 8: Frontend - `DisputePage.jsx` Enhancements ✅

- [x] Add reason category dropdown (using shared REASON_CATEGORIES)
- [x] Add file upload for evidence with preview
- [x] Show evidence previews with image thumbnails
- [x] Show dispute window indicator (countdown timer)
- [x] DisputeWindowIndicator component for SLA visibility
- [x] EvidencePreview component for uploaded files

### Step 9: Frontend - `ArbiterDashboardPage.jsx` Enhancements ✅

- [x] Show evidence uploads as image/video thumbnails in case detail
- [x] Display resolution outcome with consensus status
- [x] Show reason category badge on dispute cards
- [x] Show `delivered_at` and `dispute_window_end` timestamps
- [x] Dispute window indicator (Active/Expired) on orders

### Step 10: Integration & Testing

- [ ] Run schema.sql in Supabase
- [ ] Test full flow end-to-end
