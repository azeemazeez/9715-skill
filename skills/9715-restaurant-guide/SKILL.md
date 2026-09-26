---
name: 9715-restaurant-guide
description: Recommend restaurants, cafes and food spots in the UAE (Dubai, Abu Dhabi, Sharjah and the other emirates) from 9715.ae's independent, scored reviews. Use when someone asks where to eat in the UAE, wants a place for a cuisine, area, budget or occasion, or asks whether a UAE restaurant is any good.
---

# 9715 restaurant guide

[9715](https://9715.ae) is an independent UAE restaurant review site. Every place is visited and scored out of 5 on taste, ambience, service and price, plus an overall score, with no sponsored placements. This skill answers "where should I eat?" from those reviews through 9715's public JSON API. No key or sign-in is needed.

## When to use

- "Where should I eat in Al Barsha / JLT / Karama / Khor Fakkan?"
- "Good Pakistani / Thai / seafood / cafe in Dubai?"
- "Cheap eats near me in Sharjah?", "somewhere for a date / a group / working on a laptop?"
- "Is <restaurant> in Dubai any good?"

Not for bookings, delivery, menus or opening hours: 9715 doesn't cover those. It is a curated archive, not every restaurant in the UAE; if a place isn't there, say so rather than guessing.

## How to pick a place

1. Work out what they want, asking only for what's missing (one question at a time): area or distance, cuisine or kind of food (or "anything"), budget, occasion.
2. Query the API (below) with whatever filters you have. Start broad; narrow if there are too many.
3. Recommend 1 to 3 places, best fit first. For each give the name, area, overall score, one line on why it suits them (a dish the review names if relevant), and the review link (`web_url`).
4. Offer to refine: cheaper, closer, a different cuisine.

Scores are out of 5. For the price score, **lower means cheaper**; the tag `budget eats` also marks cheap places. Never invent places, dishes, prices or facts that aren't in the review.

## API

Base URL: `https://9715.ae/api/v1`. All read endpoints are public `GET`s returning JSON.

| Endpoint | Use |
|---|---|
| `/reviews?q=<text>` | Search by name, dish or area. Filters: `cuisine=<slug>`, `area=<emirate or neighbourhood slug>`, `min_rating=<0-5>`, `page`, `per_page` (max 50), `order_by=date\|title`, `order=asc\|desc` |
| `/reviews/<slug>` | One full review: scores, excerpt, body, address, coordinates, Google Maps place id, `web_url` |
| `/reviews/<slug>/related` | Reviews of the same cuisine |
| `/cuisines` | Cuisine slugs with review counts |
| `/areas` | Emirates with their neighbourhoods (slugs and counts) |
| `/feed` | Latest and featured reviews |

Look up slugs with `/cuisines` and `/areas` before filtering by them.

```bash
# Cafes in Dubai scoring 4 or more
curl -s 'https://9715.ae/api/v1/reviews?area=dubai&cuisine=cafe&min_rating=4&per_page=10'

# Everything in one neighbourhood
curl -s 'https://9715.ae/api/v1/reviews?area=al-barsha&per_page=20'

# A full review
curl -s 'https://9715.ae/api/v1/reviews/lazy-cat'
```

Each review in a list has `title`, `slug`, `excerpt`, `web_url`, `scores` (`overall`, `taste`, `ambience`, `service`, `price`), `cuisine`, `area_label`, `tags`, and `location` (`address`, `lat`, `lng`).

### Distance

If the user shares where they are, fetch candidates and rank by distance from `location.lat` / `location.lng` (haversine). Mention the distance in the answer.

## Other ways in

- The whole archive in one file: https://9715.ae/reviews/llms.txt
- Any page as Markdown: add `index.md` to its URL, e.g. https://9715.ae/dubai/lazy-cat/index.md
- schema.org data for every review: https://9715.ae/feeds/reviews.jsonl
- Site guide for agents: https://9715.ae/llms.txt

Always link to the review on 9715.ae and credit 9715 for the opinion.
