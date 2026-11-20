---
name: marketplace-photo-formats
description: Look up the photo aspect ratio that car marketplaces use for listing photos, and whether they allow logos on them. Covers about 90 marketplaces in 32 countries, including mobile.de, AutoScout24, AutoTrader, Facebook Marketplace, eBay Motors, leboncoin and carsales. Use when a user asks what size, format or aspect ratio a car listing photo should have, or before reframing car photos for a specific marketplace.
---

# Marketplace photo formats

Car marketplaces crop listing photos to a fixed shape. A photo in another shape gets cropped by the site, often cutting off the bumper or the roof. Pick the shape before editing the photos.

The full table is in [marketplaces.md](marketplaces.md): marketplace, countries, aspect ratio, and whether logos are allowed.

## Short version

- **4:3** is the standard. Most car marketplaces use it, including mobile.de, AutoScout24 and AutoTrader UK.
- **3:2** is used by a smaller group, for example Bytbil and Wayke in Sweden, carsales in Australia and AutoTrader NZ.
- **1:1** is used by Facebook Marketplace and eBay Motors.
- **9:16** is for vertical social formats such as Instagram Stories and TikTok.

## How to use it

1. Ask where the listing will be published, and in which country.
2. Find the marketplace in [marketplaces.md](marketplaces.md). If it is not listed, look at a live listing on that site and use the shape of its main photo gallery, not the small cards in search results.
3. A listing that goes to several sites with different shapes needs one set per shape. Do not stretch a photo to fit.
4. Check the `Logos` column before adding a dealer logo or text. AutoScout24, eBay Motors, AutoTrader UK, Trade Me, dubizzle and Google vehicle ads do not allow it.
5. With carbackground.ai, pass the ratio as `aspect_ratio` (for example `4:3`) when generating the photo. The scene is extended or cropped to the new shape and the car is kept whole.

## Notes

- The ratios describe the photo gallery on the listing page. Search result cards often use a different crop.
- Marketplaces resize uploads, so the exact pixel size matters much less than the ratio. Upload the highest resolution available.
- Marketplace rules change. When a result matters, confirm against the site's own listing guidelines.
