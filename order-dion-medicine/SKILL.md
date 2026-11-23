---
name: order-dion-medicine
description: Orders medicine for Dion, Nick's Dutch Shepherd dog, via huisdierenkliniek.nl. Use this skill whenever the user mentions ordering medicine for Dion, the dog's medication, Dion's pills, or anything about placing a vet prescription order. Even if the user just says "order Dion's medicine" or "bestel medicijnen voor Dion", trigger this skill immediately.
---

## What this skill does

Fills out and submits the medication order form at huisdierenkliniek.nl for Dion (Nick's dog), using pre-set owner and pet details. Uses the Claude in Chrome MCP tools to automate the browser.

**Run this from the user's own browser (Claude in Chrome).** The form uses invisible reCAPTCHA v3; a headless or cloud browser gets scored as a bot and the submit silently fails (no POST reaches the clinic). If Chrome tools aren't available, tell the user rather than retrying.

The clinic prescribes at most 1 month of medication per order.

## Step 1: Ask which medicine(s) to order

Before opening the browser, ask the user which medicine(s) they want to order. Present the four options:

1. Carporal — 1x per day, 1 tablet
2. Enurance 50mg — 2x per day, ½ tablet
3. Gabapentine 300mg — 3x per day, 1 tablet
4. Tralieve — 3x per day, 1 tablet (strength not yet recorded; ask the user for the mg if they want it on the order)

They can select any combination, or add something else. Wait for their answer before proceeding.

## Step 2: Navigate to the form

Navigate to: https://www.huisdierenkliniek.nl/form/Bestelformulier%20medicatie/1

## Step 3: Fill in all fields

Use `form_input` with the ref IDs from `read_page`. The field names in the DOM are:

| Label | Field name (for reference) |
|---|---|
| Achternaam | data[Form][63] |
| Voorletter(s) | data[Form][1] |
| Straat en huisnummer | data[Form][2] |
| Postcode en woonplaats | data[Form][64] |
| Telefoonnummer | data[Form][3] |
| E-mailadres | data[Form][17] |
| Naam (huisdier) | data[Form][5] |
| Diersoort (select) | data[Form][62] |
| Medicatie (textarea) | data[Form][16] |

Use `read_page` with `filter: "interactive"` to get the current ref IDs, then fill each field:

**Owner details**
- Achternaam: `De Jong`
- Voorletter(s): `N`
- Straat en huisnummer: `Barestraat 38`
- Postcode en woonplaats: `9725CP Groningen`
- Telefoonnummer: `0655523013`
- E-mailadres: `nickthejong@hotmail.com`

**Pet details**
- Naam: `Dion`
- Diersoort: `hond` (select option value `1`)

**Medicine field** (data[Form][16], a textarea): enter the selected medicine(s), one per line, using exact names and dosage instructions from Step 1.

## Step 4: Confirm before submitting

Before clicking submit, show the user a summary:

```
Klaar om in te dienen:
- Naam: N. de Jong
- Huisdier: Dion (hond)
- Medicatie:
  [list selected medicines]

Wil je dit versturen?
```

Only submit if the user explicitly confirms. If they say no, stop — do not submit.

## Step 5: Verify submission

After clicking "Verstuur", check that the page changed to a confirmation. If the form is still shown unchanged, the order was NOT sent — tell the user and suggest calling the clinic (050 - 526 32 55).
