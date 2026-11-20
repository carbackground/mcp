---
name: car-listing-photos
description: Turn the photos of one car into a consistent, dealership-quality listing set with carbackground.ai. Use when a user wants to improve, clean up or standardise car photos for a dealer website, a marketplace listing, a social post or an ad, replace a car photo's background, or hide license plates.
---

# Car listing photos

Edit a set of car photos so the whole listing has one look: the same background on every photo, plates hidden, and the shape the marketplace expects. The car itself is kept as photographed.

Requires the carbackground.ai MCP server (`https://api.carbackground.ai/mcp`). If its tools are not available, tell the user to connect it: https://github.com/carbackground/mcp

## Before you start

Ask for anything that is missing:

1. **The photos**, as public image URLs (JPEG, PNG or WebP). One photo per call.
2. **Where the listing goes.** This decides the aspect ratio; see the `marketplace-photo-formats` skill. If the user does not say, keep each photo's own shape.
3. **The background.** Call `list_backgrounds` and either pick the scene that fits the user's brand or show the options and ask. A dealer who already has a background should keep using it.
4. **License plates.** Hidden by default for a public listing unless the user wants them shown. Styles: a blank plate (`plain`), a blur (`blur`), or the dealer's own plate design (`logo`).

Call `get_credits` first when there are many photos: each finished photo costs one credit, and failed generations are not charged.

## Steps

1. Choose one `background_id` and use it for every photo of the car. A listing that mixes backgrounds looks worse than one that keeps the originals.
2. For each photo call `generate_background` with `image_url` and `background_id`, plus the options the user asked for:
   - `hide_license_plate: true` with `license_plate_style`
   - `aspect_ratio`, for example `4:3`
   - `logo` to draw the dealer's logo (get the `id` from `list_logos`)
   - `center_car` to centre and level the car
   - `clean_car` to show it freshly washed
3. Interior and detail shots go through the same tool with the same `background_id`. They are cleaned up so the whole listing has one look.
4. Look at each result before handing it over. If a result changed something on the car (wheels, badges, plate text, bodywork), run that photo again.
5. Return the results in the original photo order. Each `image_url` is a signed link valid for 7 days, so tell the user to download the files.

## Good to know

- A generation can take up to a minute. Send all photos of a listing in parallel when the client allows it, up to 30 at a time.
- To hide plates without changing the background, use `hide_license_plates`. It uses no credits and needs an active subscription.
- When no built-in scene fits, `create_background` makes a custom one from a description or reference photos (for example the dealer's real showroom), and `refine_background` adjusts it. Each preview costs one credit.
- Some marketplaces do not allow logos or text on listing photos. Check `marketplace-photo-formats` before adding a logo.
- When credits run out, `list_plans` shows the subscriptions and `create_checkout` returns a payment link. Never confirm a plan change without the user's agreement.
