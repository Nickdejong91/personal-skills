---
name: order-dion-medicine
description: Orders medicine for Dion, Nick's Dutch Shepherd dog, via huisdierenkliniek.nl. Use this skill whenever the user mentions ordering medicine for Dion, the dog's medication, Dion's pills, or anything about placing a vet prescription order. Even if the user just says "order Dion's medicine" or "bestel medicijnen voor Dion", trigger this skill immediately.
---

## What this skill does

Fills out and submits the medication order form at huisdierenkliniek.nl for Dion (Nick's dog), using pre-set owner and pet details.

## Step 1: Ask which medicine(s) to order

Before opening the browser, ask the user which medicine(s) they want to order. Present the three options clearly:

1. Carporal — 1x per day, 1 tablet
2. Enurance 50mg — 2x per day, ½ tablet
3. Gabapentine 300mg — 3x per day, 1 tablet

They can select one, two, or all three. Wait for their answer before proceeding.

## Step 2: Fill out the form

Navigate to: https://www.huisdierenkliniek.nl/form/Bestelformulier%20medicatie/1

Fill in all fields with these fixed values:

**Owner details**
- Achternaam: `De Jong`
- Voorletter(s): `N`
- Straat en Huisnummer: `Barestraat 38`
- Postcode en Woonplaats: `9725CP`
- Telefoonnummer: `0655523013`
- E-Mailadres: `nickthejong@hotmail.com`

**Pet details**
- Naam: `Dion`
- Diersoort: `hond`

**Medicine field** (`data[Form][16]`): enter the selected medicine(s), one per line, using the exact names and dosage instructions as listed in Step 1.

## Step 3: Confirm before submitting

Before clicking submit, show the user a summary:

```
Klaar om in te dienen:
- Naam: N. de Jong
- Huisdier: Dion (hond)
- Medicatie:
  [list the selected medicines]

Wil je dit versturen?
```

Only submit the form if the user explicitly confirms. If they say no or want to change something, go back to Step 1.
