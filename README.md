# HappyTable

HappyTable is for a front-of-house manager at a small restaurant. When a few tables are sitting empty, the manager can write one service note, check the terms and post a walk-in offer. The diner feed shows exactly what someone arriving for dinner would read.

[Prototype](https://htmlpreview.github.io/?https://github.com/jmceniff/genai-practice/blob/96652b52ee5398d284991ed857c13d724e4899a6/index.html)

[Github](https://github.com/jmceniff/genai-practice)

[Demo Video](https://drive.google.com/file/d/1osGMeDcbcIt-ihj9Z7pBQ6pYK4n-mE-u/view?usp=sharing)

To try the change a classmate asked for in my last peer review feedback, pick the service note with “half six, five tables open.”, the draft should show five tables, 6:30 pm and 15% off. Add an end time later than both the start and the current local time, then publish. Then open the newly added Diner feed to see the approved offer at the top of the updated feed.

## Where AI fits

The intended AI task is to turn a manager’s sentence into a table count, a time window, a discount, an optional dish and a short offer diners can read. In V2, JavaScript is used to match the familiar phrases and fill in a sentence template provided on the platform. The manager still has the ability to change the details and wording before publishing the offer on HappyTable.

The editing, discount checks, publishing and expiry all run in the browser. The restaurants and sample offers are fictional for this demonstration (except Traditions). Offers you publish appear in the same tab and disappear when the webpage the site is opened on refreshes.

_________________________________________________________


# HappyTable revision

## Peer Review Adjustments:

1. Make the draft easy to check: The review screen opens with the simulated-AI notice, the original service note and the fields it produced. The manager can see where a number came from before putting the restaurant’s name on it.
2. Read everyday service notes: “Five tables,” “a couple of tables” and “half six” now work. “A few tables” still needs a number from the manager. Three tables and five tables are different offers.
3. Carry corrections through to the diner: Filling a missing field clears its warning and updates the wording. A manager can also write their own copy. If its stated percentage disagrees with the discount field, they have to fix it before publishing. The diner feed shows the full approved wording.

## Response to peer feedback

| Feedback or observed issue | Decision | What changed, or why |
| --- | --- | --- |
| One reviewer couldn’t find the simulated-AI notice. Others said they saw it. A placeholder Replit link also made it unclear which copy to open. | Acted on | The notice sits above the fields on every draft. The README identifies V2 by its opening screen and links directly to its HTML file. |
| “Half six, five tables open” produced only a discount. One reviewer suggested a small number-word mapping. | Adapted | The matcher now handles that wording and “a couple.” An approximate count stays blank. A live model can wait until the review flow has been tried during service. |
| After a reviewer supplied an end time and 25% for the salmon offer, the warnings stayed on and the listing still said the discount needed to be set. | Acted on | Warnings clear on edit. Generated copy updates immediately, and the same approved text appears in both views. Custom wording stays editable. |
| A reviewer tried a backwards time window and found the large-discount confirmation useful. | Acted on | V2 checks that the end follows the start and is still ahead today. Discounts above 40% require a separate confirmation. |

The diner feed was a scope decision. I wanted to show where the offer goes after the manager presses Publish. One switch on the same page does that. Diners can read the offer and see when it ends. Accounts and bookings can wait.

## Evaluation

The cases below used fictional offers. Automated checks ran the page’s JavaScript with simulated form controls and a clock set to 5:30 pm. That made it possible to check the same evening window and its expiry on every run. These are checks of the offer flow. Usability during a real shift still needs a manager to try it.

| Case | Input or action | Expected result / success criterion | Actual result | Pass, partial, or fail | Implication or next step |
| --- | --- | --- | --- | --- | --- |
| Typical 1 | “quiet tonight, 4 tables free between 6 and 7.30, 20% off.” Publish and open the feed. | Show 4 tables, 6–7:30 pm and 20%. Remove the offer when the window closes. | Traditions appeared first with those terms and 2h remaining. At 7:30 pm it disappeared and the manager’s count returned to zero. | Pass | The manager and diner saw the same terms and expiry. |
| Typical 2 | “we’re quiet from about half six, five tables open, take 15 percent off.” Add an 8 pm end time. | Find 5 tables, 6:30 pm and 15%. Flag only the end time. | All three values appeared. Adding 8 pm cleared the warning, updated the wording and allowed publication. | Pass | The manager supplied just the detail missing from the note. |
| Challenge | “2 tables free from 8, knock a bit off the bill, want to move the salmon.” Try publishing, then enter 9 pm, 25% and “grilled salmon.” | Pick up the count, start and dish. Leave the end and discount blank. Use the corrections in the listing. | Publishing stopped at the two missing fields. After the edits, the feed showed 25% off and “Featured dish: grilled salmon.” | Pass | The diner saw the price the manager chose. |
| Invalid or empty | Press Draft offer with only spaces in the box. | Explain what to enter and leave the offer count at zero. | The form asked for a service note or an example. The review panel stayed closed and nothing published. | Pass | Someone can try an example from the same screen. |
| Risk | Set the field to 15%. Type “15% off dinner. Walk in for 50% off.” Try publishing, then choose Use matching text. | Catch the conflicting 50% even though 15% also appears. Let the manager inspect the repair. | The warning named both percentages and the 15% field. The count stayed at zero after repair. A separate Publish added one offer at 15%. | Pass | Checking only the first matching percentage would have missed the more expensive promise. |

### Before and after

The same “half six, five tables open” note was run against both local versions. V1 found 15% and left both the table count and start time empty. Its copy began “Tables free at Traditions tonight.” V2 found five tables and a 6:30 pm start. Both versions left the end time blank, which is the one detail the manager actually left out.

### What the evidence tells me

For the service notes above, a manager can correct one field and see that correction reach the diner. That was the part that broke in peer review. There are still ordinary phrases the matcher misses. In a separate check, “a few tables from 6 to 8, half price” produced the time window but left the discount blank. Asking for the table count makes sense there, but someone would reasonably expect “half price” to work. The next test should use a manager’s own notes and record what they have to retype. These examples tell us little about how long that would take or how a live model would perform.

## Risk response

A diner who reads 50% off may turn up expecting the restaurant to honour it. That is an easy mistake to overlook when the copy looks finished, especially if the manager assumes AI has already checked it. V2 keeps the original note visible, checks the percentages in the wording at publication and sends a mismatch back for correction. The risk case above caught a contradiction even with the correct figure in the first sentence. The checks run in code. A manually written time or dish can still be wrong, and the prototype has no way to know whether the kitchen has sold out.

## Remaining limits and next priorities

Offers exist only in the current tab. The feed demonstrates what a diner would see, and the screen has yet to be tested during a shift or with a diner at the door.

1. Give a manager a quiet-service scenario and let them write the note unaided. Record what they correct and whether they notice the simulated-AI label.
2. Try those notes with a model and the current rules side by side. Count missing fields separately from plausible but wrong numbers.
3. Test hand-edited times and quantities against the offer fields. A discount check alone would miss “four tables” in the text of a two-table offer.


______________________________________________________


## AI-use disclosure

I used Claude to help generate the HappyTable HTML page and JavaScript for the first prototype and the initial V2. I used ChatGPT Codex for further enhancement with the new sol 6.2 model and to overcome the pushbacks received fromm Claude when trying to improve the app UI and features.

## Two prompt excerpts

| What I asked | What came back and what changed |
| --- | --- |
| “can you help optimising the UI, it is too dark and lacks depth” | Claude had outputted an html file that was very restricting, included errors, and had dark colour shceme I didn't like'. |
| “Create a UI, for the diners using Happytable, so we can demonstrate both ends of the idea” | Clause had originally built a UI strictyly for resutarant managers as per  my istrcutions, however, with further prompting it outputted a diner feed, showing what users searching for the restaurant deals see. |

## A decision I made

Claude advised me to leave out the diner view initially, it was originaly outside of the scope, this is where I received pushback from Claude as it appeared to be out of scope. However, I still wanted to show off how invisioned both sides of the app working.

## Verification

The peer reviews gave me two concrete things to go back to, the first mentioned the missed “half six, five tables” note and the salmon offer that still said the discount needed to be set. Codex reran the first note against V1 and V2 and exercised the revised page logic for the second, while also checking conflicting discounts and expiry. The tests used simulated controls and a fixed clock, a manager still needs to try the page during service.

## AI inside HappyTable

The intended input is one sentence from a restaurant manager. The output is an editable offer with the table count, times, discount, optional dish and diner wording. The current page uses JavaScript patterns and a text template created by prompting Claude, and the review screen labels that step **Simulated AI**. The manager chooses the final terms and presses Publish after receiving various confirmation checks received from HappyTable hen creating offers.
