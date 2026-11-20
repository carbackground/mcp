---
name: dealer-inventory-photos
description: Process a whole dealer inventory of car photos in bulk through the carbackground.ai REST API. Use when a user wants to batch-edit vehicle photos from a stock list, CSV, inventory feed or DMS export, automate car photo background replacement, or integrate car photo editing into dealer or marketplace software.
---

# Dealer inventory photos in bulk

Run every photo in a stock list through carbackground.ai with one background, so the dealer's whole inventory looks the same. For a handful of photos use the `car-listing-photos` skill with the MCP server; for hundreds, write a script against the REST API as described here.

- API reference: https://api.carbackground.ai/openapi.yaml
- Summary: https://api.carbackground.ai/llms.txt

## Setup

1. The user creates an API key at https://app.carbackground.ai/settings/api-keys. Read it from an environment variable such as `CARBACKGROUND_API_KEY`. Never write the key into a file that is committed.
2. Every request sends `Authorization: Bearer cbg_<key>`.
3. Check the balance with `GET /v1/credits` and compare it with the number of photos. One credit per finished photo; failed generations are not charged.
4. Pick the background once with `GET /v1/backgrounds` and reuse its `id` for the whole run.

## One photo

```bash
curl -s https://api.carbackground.ai/v1/generations \
  -H "Authorization: Bearer $CARBACKGROUND_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "image_url": "https://example.com/stock/1042/front.jpg",
    "background_id": "<id from /v1/backgrounds>",
    "hide_license_plate": true,
    "aspect_ratio": "4:3"
  }'
```

The response has `status`, the result's `image_url` (a signed link valid for 7 days) and `credits_charged`. A request can take up to a minute, so set the client timeout to at least 150 seconds.

## The batch

1. Build the work list: one row per photo with the vehicle id (stock number or VIN), the photo's position in the listing and its URL.
2. Send the photos in parallel, up to 30 at a time. That is the same batch size the carbackground.ai app uses, and it finishes a whole car in about the time one photo takes.
3. Handle each response by status:
   - `200`: download the result straight away and save it under the vehicle id and position.
   - `429`: wait for the `Retry-After` header, then retry.
   - `503`: temporary, retry after a short wait.
   - `422`: this photo cannot be processed (unreadable image, or no result). Log it and move on; do not retry unchanged.
   - `402`: out of credits. Stop the run and tell the user.
4. Record every finished photo, so a run that stops can resume without paying twice.
5. Finish with a summary: photos done, photos failed with the reason, credits used.

## Options per photo

`hide_license_plate` with `license_plate_style` (`plain`, `blur` or `logo`), `aspect_ratio`, `logo`, `center_car` and `clean_car` work as in the API reference. Keep them the same for every photo in the run.

## Good to know

- Photos are sent as public URLs or as base64 in `image_base64` (JPEG, PNG or WebP).
- `POST /v1/plates` hides license plates only. It uses no credits and is included with a subscription.
- A dealer's own scene (their real showroom, brand colours) is created once with `POST /v1/backgrounds` and then used like any other background.
- Check which aspect ratio the target marketplace uses in the `marketplace-photo-formats` skill.
