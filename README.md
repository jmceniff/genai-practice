# HappyTable

A prototype that lets a restaurant manager describe tonight's empty tables in one sentence and turn it into a published last-minute offer for diners.

## What the first working slice does

A manager types one sentence about tonight, for example "quiet tonight, 4 tables free between 6 and 7.30, 20% off". The prototype pulls the details out of that sentence, shows them as editable fields next to what was typed, and drafts the text a diner would see. The manager corrects anything wrong, fills in anything missing, and publishes. The offer then appears in a "Live tonight" list with the time window, the discount and an expiry time.

## Opening the prototype

Live version: [REPLIT LINK]

To run it locally, open `index.html` in any browser. There is nothing to install and no configuration.

## AI status: simulated

The intended AI behaviour is that a language model reads the manager's sentence, extracts the structured offer details (date, time window, number of tables, discount, dish to feature) and writes the diner-facing description.

In this prototype that step is simulated. The extraction is done with pattern matching in JavaScript, and the description is assembled from prewritten sentence patterns. No AI model or external API is called. The draft panel tells the user this on screen.

Working now: the full user flow, the editable draft, the validation rules, publishing, and the live offer list.

Simulated: the extraction and the written description.

Not built: accounts, payments, saved data, any connection to a booking system or POS.

## Test results

| Test | Input or action | What happened | Pass, partial, or fail |
|---|---|---|---|
| Typical case | "quiet tonight, 4 tables free between 6 and 7.30, 20% off" | Extracted 4 tables, 6.00pm to 7.30pm and 20%. Description drafted correctly. Published to the live list with the right window and expiry. | Pass |
| Challenge case | "2 tables free from 8, knock a bit off the bill, want to move the salmon" | Extracted 2 tables, an 8pm start and salmon as the dish. No end time or discount found, so both were flagged as "not found" and publishing was blocked until they were filled in. It did not invent a discount from "knock a bit off". | Pass |
| Invalid or empty case | Pressed "Draft offer" with the box empty | Showed "Please describe tonight's empty tables" and no draft panel appeared. | Pass |

## Known limitations

1. **The AI step is simulated.** Extraction is pattern matching, so it only handles the phrasings it was written for. A sentence like "we're quiet from about half six" would not produce a start time, and the manager would have to type it in. The next step is to replace this step with a model call and compare the two on the same sentences.

2. **Nothing is saved.** Published offers live in the browser page only and disappear on refresh, and there is no diner-facing view beyond the list on the same screen. Storage and a separate diner page are deliberately outside this first slice.

3. **The safeguards are minimal.** Publishing is blocked when a field is missing and a discount above 40% asks for confirmation, but there is no check that the time window is in the future or that the end time is after the start time.
