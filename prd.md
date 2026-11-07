---
title: Uber Rider App — Pricing & Fare Transparency
status: draft
created: 2026-11-07
updated: 2026-11-07
---

# Uber Rider App â€” Pricing & Fare Transparency
## Advanced Product Requirements Document (PRD)

**Document status:** Draft for product, design, engineering, data, finance, operations, and legal review  
**Product area:** Rider pricing and fare transparency  
**Target surface:** Rider app (platform scope to be confirmed)  
**Market scope:** Not selected; market-specific rules must be validated before implementation
**Document owner:** Product Management  
**Version:** 1.0  
**Date:** 7 November 2026

> **Important:** This is an independent product case study with proposed requirements and illustrative targets. It is not an Uber internal document and does not describe Uber's confidential pricing algorithms, APIs, commercial policies, or actual operating metrics. All monetary examples and thresholds must be validated for the launch market.

> **Evidence note:** This PRD is based only on the supplied file `Input/Uber_Pricing_Transparency_Advanced_PRD.md`. That file does not provide rider research, market data, Uber product documentation, policy, or engineering specifications. Its problem statements are hypotheses; requirements, sample APIs, sample payloads, example metrics, research samples, and rollout steps are proposals or illustrations, not verified facts about Uber or a launch market. Unknown market facts remain open for validation.

---

# 1. Executive summary

## 1.1 Product vision

Make ride pricing understandable, predictable, and trustworthy before a rider confirms a booking, while preserving the marketplace's ability to balance rider demand, driver supply, service costs, and operational constraints.

## 1.2 Problem statement

Riders may see a fare estimate before booking but still struggle to understand:
- Why one ride category costs more than another.
- Why prices change between searches or over short periods.
- Which components are included in the displayed price.
- Whether tolls, taxes, airport fees, waiting time, route changes, or other adjustments may affect the final amount.
- Why the final fare differs from the pre-booking estimate or upfront quote.
- Whether a promotion or discount has actually been applied.

When pricing feels opaque or unpredictable, riders may abandon booking, switch to another transport option, contact support, dispute charges, or avoid using the service again.

At the same time, pricing cannot be optimized only for the lowest rider price. It must also support driver-partner earnings, driver availability, marketplace liquidity, service reliability, and sustainable unit economics.

## 1.3 Proposed solution

Build an improved pricing experience with five connected capabilities:

1. **Clear pre-booking price presentation:** Show an understandable fare or upfront price, what it includes, and relevant conditions.
2. **Explainable price changes:** Explain meaningful changes in price using approved, non-sensitive reason categories such as high demand, selected ride category, route, or market-specific fees.
3. **Fare-change consent:** Where the product's pricing model permits a change after confirmation, explain the reason and obtain consent when required by policy and law.
4. **Post-trip fare breakdown:** Show the final fare, adjustments, discounts, taxes, tolls, and payment status in a clear receipt.
5. **Pricing issue resolution:** Let riders identify a specific fare concern and route it to the right support workflow with trip and pricing context.

## 1.4 Product outcome

Increase rider confidence and completed bookings by reducing pricing-related uncertainty, without worsening driver-partner outcomes, marketplace availability, safety, or contribution margin.

---

# 2. Background and product context

Ride-hailing pricing is a marketplace problem, not just a calculator. A pricing experience may need to account for:
- Pickup and destination
- Estimated route and distance
- Estimated duration
- Ride category and vehicle capacity
- Local base fare and market-specific rate cards
- Demand and supply conditions
- Tolls, airport fees, taxes, and other applicable charges
- Promotions, credits, and payment-specific offers
- Waiting time, route changes, stops, or other post-booking events
- Regulatory and contractual requirements

The exact pricing model differs by market and product. This PRD separates the **pricing engine** from the **rider-facing explanation and consent experience**. Engineering, Finance, Legal, and Marketplace Operations must validate the actual calculation rules for the launch market.

## 2.1 Product principles

1. **Price clarity before conversion:** Show material price information before the user commits.
2. **No misleading certainty:** Distinguish estimates from guaranteed or upfront prices.
3. **Explain material changes:** Give a clear reason when the expected amount changes.
4. **Fairness and consistency:** Apply approved pricing rules consistently and monitor for unintended disparate outcomes.
5. **Marketplace health:** Do not optimize rider price in isolation from driver supply, earnings, reliability, and unit economics.
6. **Auditability:** Every quote and final fare must be traceable to a versioned calculation and its inputs.
7. **Privacy by design:** Use only data necessary for pricing and comply with applicable privacy requirements.
8. **Market configurability:** Keep rate cards, fees, taxes, rounding, disclosures, and policy rules configurable by market.

---

# 3. Goals, non-goals, and scope

## 3.1 Goals

- Improve rider understanding of the price before booking.
- Reduce abandonment attributable to price uncertainty.
- Reduce pricing-related support contacts and fare disputes.
- Reduce unexplained differences between quoted and final amounts.
- Improve rider trust and repeat usage.
- Preserve driver-partner earnings health and marketplace availability.
- Provide Finance and Operations with auditable pricing records.

## 3.2 Non-goals

This release will not:
- Replace the entire marketplace dispatch or driver allocation system.
- Create a new global pricing strategy or universal rate card.
- Guarantee that every ride in every market has a fixed price.
- Expose confidential driver-level, competitor, or marketplace-sensitive data.
- Redesign driver-partner earnings statements.
- Launch a new loyalty, subscription, or membership program.
- Make automated legal or policy decisions without approved rules and human escalation paths.

## 3.3 MVP scope

**In scope**
- Fare estimate and/or upfront quote presentation, based on market capability.
- Fare breakdown before confirmation.
- Explanation of material price components and eligible price changes.
- Promotion and discount presentation.
- Quote expiry and refresh behaviour.
- Post-trip fare breakdown and receipt.
- Fare issue reporting.
- Instrumentation, experimentation, and operational dashboards.

**Later scope**
- Personalized price education.
- Advanced price comparison and trip timing suggestions.
- More sophisticated promotion recommendations.
- Proactive price-change notifications for saved or scheduled trips, subject to user value and policy review.

---

# 4. Problem discovery

## 4.1 Discovery questions

The team should validate the problem before committing to a solution. Key questions include:

### Rider understanding
- Do riders understand whether the displayed amount is an estimate or an upfront price?
- Which fare components are most confusing?
- Do riders notice taxes, tolls, airport fees, or other charges before booking?
- Can riders explain why two ride categories have different prices?
- Do riders understand when and why a displayed quote expires?

### Rider behaviour
- At which point do users abandon after viewing the fare?
- Does price-related abandonment vary by city, time of day, ride category, or trip distance?
- How often do users compare ride categories and then leave the flow?
- Does a price change between quote and confirmation cause abandonment?

### Post-trip experience
- How often does the final amount differ from the pre-booking amount?
- Which adjustment types generate the most complaints?
- Do users understand route, waiting-time, toll, or stop-related adjustments?
- How long does it take to resolve a fare dispute?

### Marketplace health
- Does reducing or smoothing rider prices affect driver acceptance and availability?
- Are there periods when lower prices create an imbalance between demand and supply?
- Does the pricing experience create unrealistic expectations about pickup time or service availability?

## 4.2 Discovery data to request

| Data set | Required fields | Why it matters |
|---|---|---|
| Quote funnel | Quote viewed, category selected, booking confirmed, abandonment | Find pricing-related drop-offs |
| Quote history | Quote ID, timestamp, amount, currency, expiry, quote type | Measure price changes and quote freshness |
| Final fare | Final amount and component-level adjustments | Identify quote-to-final differences |
| Pricing complaints | Issue category, reason, resolution, refund or adjustment | Find confusing or disputed components |
| Promotion usage | Offer viewed, applied, rejected, redeemed | Understand discount clarity |
| Marketplace supply | Eligible drivers, acceptance, pickup ETA, cancellations | Guardrail pricing changes |
| Driver outcomes | Relevant aggregate earnings and trip outcomes | Detect negative supply-side effects |
| Customer cohorts | New/returning, market, ride category, device/app version | Identify segment differences |

Use pseudonymized identifiers and aggregated analysis where possible. Access to precise location or sensitive customer data should be limited to approved purposes.

## 4.3 Funnel to analyze

1. User opens the ride request flow.
2. User enters pickup and destination.
3. Fare quote is requested.
4. Fare options are displayed.
5. User views price details.
6. User selects a ride category.
7. User confirms booking.
8. Driver is matched.
9. Trip is completed.
10. Final receipt is viewed.
11. User contacts support or submits a fare issue, if applicable.

Analyze conversion and drop-off at each step. Do not assume every user who leaves after viewing a fare did so because of price; validate with behavioural data, surveys, and experiments.

## 4.4 Research methods

### Quantitative research
- Funnel analysis by market, ride category, and customer cohort.
- Quote-to-confirmation conversion analysis.
- Distribution of quote-to-final differences.
- Fare-related support-contact analysis.
- Experimentation on price explanation and breakdown presentation.
- Correlation analysis between marketplace conditions and price-related abandonment.

### Qualitative research
- Moderated usability sessions with riders.
- Interviews with new, occasional, and frequent riders.
- Post-trip interviews with riders who raised fare concerns.
- Support-agent interviews to understand recurring complaint patterns.
- Driver-partner and Operations interviews to understand supply-side consequences.

### Proposed research sample
For an initial discovery round, recruit approximately:
- 8â€“12 frequent riders.
- 8â€“12 occasional riders.
- 5â€“8 riders who recently contacted support about fares.
- 5â€“8 driver-partners or relevant operations representatives.

This is a directional qualitative sample, not a statistically representative survey. Final sample size should be set by research goals and saturation.

## 4.5 Research hypotheses to validate

- H1: Some riders abandon because they cannot distinguish an estimate from an upfront price.
- H2: A concise, itemized pre-booking breakdown improves price comprehension.
- H3: Explaining material price changes reduces perceived unfairness and support contacts.
- H4: Showing promotion savings next to the eligible ride option improves offer comprehension.
- H5: Clear post-trip receipts reduce fare-related support contacts.
- H6: A more transparent experience can improve conversion without materially harming driver acceptance, availability, or contribution margin.

These are hypotheses, not established facts.

---

# 5. Product strategy and solution hypotheses

## 5.1 Opportunity statement

Riders need to understand what they are likely to pay, what is included, and what circumstances may change the amount. If Uber makes price information easier to understand at the moment of decision and after the trip, riders may make more confident decisions and raise fewer avoidable disputes.

## 5.2 Solution pillars

| Pillar | Product capability | Expected user value |
|---|---|---|
| Understand | Clear fare type and itemized breakdown | User knows what the amount represents |
| Compare | Comparable ride options and savings | User can make an informed choice |
| Trust | Explain meaningful price changes | Fewer surprises |
| Verify | Post-trip receipt and adjustment details | User can reconcile the final amount |
| Resolve | Contextual fare issue flow | Less effort to get help |

## 5.3 Hypothesis-to-measurement map

| Hypothesis | Intervention | Primary measure | Guardrail |
|---|---|---|---|
| Clearer price type reduces uncertainty | Label estimate versus upfront price | Quote-to-confirmation conversion | Fare complaints |
| Itemization improves comprehension | Expandable pre-booking breakdown | Comprehension task success | Booking latency |
| Price-change explanations build trust | Reason-specific explanation | Pricing-related support contacts | Cancellation rate |
| Promotion clarity improves offer use | Show discount and eligibility clearly | Eligible offer redemption | Net revenue and margin |
| Better receipts reduce disputes | Itemized final receipt | Fare contacts per 1,000 trips | Refund rate and dispute outcomes |

---

# 6. Target users and jobs to be done

## 6.1 Rider segments

### Segment A: Price-sensitive rider
- Compares available ride categories.
- Wants to know the payable amount and discount.
- May abandon if the amount is outside their budget.

### Segment B: Time-sensitive rider
- Values pickup reliability and speed.
- Wants to understand whether a higher-priced option offers a meaningful benefit.
- Needs price and ETA information together.

### Segment C: Frequent commuter
- Books repeatedly and notices price variation.
- Wants consistency, clear explanations, and fast repeat booking.
- May contact support if the final amount differs unexpectedly.

### Segment D: New or occasional rider
- May not understand pricing terminology or adjustments.
- Needs simple explanations and confidence before confirming.

## 6.2 Jobs to be done

- **Functional job:** Help me understand the likely cost of my ride before I book.
- **Decision job:** Help me compare ride options and choose the one that fits my budget and needs.
- **Trust job:** Help me understand why the price changed, if it changed.
- **Resolution job:** Help me verify and challenge a final fare if something appears incorrect.

---

# 7. User stories

| ID | User story | Priority |
|---|---|---|
| US-01 | As a rider, I want to know whether the displayed amount is an estimate or an upfront price so I can understand the level of price certainty. | P0 |
| US-02 | As a rider, I want to see the main components of the fare so I can understand what I am paying for. | P0 |
| US-03 | As a rider, I want to compare prices across eligible ride categories so I can choose an option that suits my budget. | P0 |
| US-04 | As a rider, I want to know when a quote expires so I do not rely on an outdated amount. | P0 |
| US-05 | As a rider, I want to understand a material price change before confirming so I can make an informed decision. | P0 |
| US-06 | As a rider, I want to see the value and conditions of an applied promotion so I know whether the discount is valid. | P1 |
| US-07 | As a rider, I want a clear final receipt so I can reconcile the completed trip. | P0 |
| US-08 | As a rider, I want to report a fare concern against a specific trip so I do not need to explain the entire journey again. | P1 |
| US-09 | As a rider, I want the app to retain the quote context for my booking so that support can investigate discrepancies. | P0 |
| US-10 | As a rider, I want prices and fees displayed in my local currency and applicable format so I can understand the amount. | P0 |

---

# 8. Detailed functional specifications

## Module 1: Quote generation and price type

**Objective:** Generate and present a current price representation for the selected route and ride category.

### Functional requirements

| ID | Requirement | Priority |
|---|---|---|
| PR-001 | The app must request a price quote after valid pickup, destination, and ride-category inputs are available. | P0 |
| PR-002 | The pricing service must return a unique quote ID. | P0 |
| PR-003 | The response must identify the quote type, such as estimate or upfront price. | P0 |
| PR-004 | The response must include currency, amount, creation time, and expiry time where applicable. | P0 |
| PR-005 | The app must clearly label the price type in the rider interface. | P0 |
| PR-006 | The service must return the market and pricing-rule version used to calculate the quote. | P0 |
| PR-007 | The app must request a refreshed quote when material quote inputs change. | P0 |
| PR-008 | The app must not present an expired quote as current. | P0 |

### Acceptance criteria

- A valid quote response includes quote ID, currency, amount or range, quote type, and applicable validity information.
- An estimate is labelled as an estimate and is not visually represented as a guaranteed final amount.
- An upfront price is labelled according to the applicable market policy and terms.
- If the quote expires, the app refreshes it or asks the user to refresh before confirmation.
- If quote generation fails, the app displays a recoverable error and does not invent or reuse an invalid price.
- Quote requests are idempotent where appropriate and traceable for debugging.

## Module 2: Pre-booking fare breakdown

**Objective:** Explain the components of the displayed fare without overwhelming the user.

### Proposed breakdown categories

The exact components depend on market and pricing model. Examples may include:
- Base or minimum fare
- Distance component
- Time component
- Demand-related adjustment, where applicable and permitted
- Tolls or airport fees, where applicable
- Taxes and statutory charges
- Promotion or credit
- Total estimated or upfront amount

Do not expose a component that is not actually used in the market's calculation. Do not imply that an estimate's component values are guaranteed if they can change.

### Functional requirements

| ID | Requirement | Priority |
|---|---|---|
| PR-009 | The app must provide an accessible fare-breakdown view before confirmation. | P0 |
| PR-010 | The breakdown must distinguish included components from conditional or variable charges. | P0 |
| PR-011 | The breakdown must display the applicable currency and rounding convention. | P0 |
| PR-012 | The app must show eligible discounts separately from the pre-discount amount. | P1 |
| PR-013 | The total shown must reconcile mathematically with the displayed components, subject to documented rounding rules. | P0 |
| PR-014 | The breakdown must use plain language and provide contextual help for unfamiliar terms. | P1 |

### Acceptance criteria

- The total matches the pricing service's returned payable or estimated amount.
- Conditional charges are not presented as certain charges.
- A discount is displayed only if it is eligible and applied or clearly identified as a potential offer.
- The user can expand and collapse the breakdown.
- The view is accessible with screen readers and does not rely on colour alone.
- All amounts use the correct market currency and formatting.

## Module 3: Price comparison across ride categories

**Objective:** Help riders compare eligible options on a like-for-like basis.

### Functional requirements

| ID | Requirement | Priority |
|---|---|---|
| PR-015 | The app must show available ride categories with their corresponding quote. | P0 |
| PR-016 | Each option must show the quote type and currency. | P0 |
| PR-017 | Each option must show relevant estimated pickup time. | P0 |
| PR-018 | The app may show a concise difference between selected and alternative options. | P1 |
| PR-019 | The app must not compare prices from different quote timestamps as if they were simultaneously valid. | P0 |
| PR-020 | The app must refresh affected options when route or material market inputs change. | P0 |

### Acceptance criteria

- Only eligible ride categories are displayed as selectable.
- Each displayed price is tied to a quote ID and timestamp.
- The user can distinguish price from pickup ETA and vehicle category.
- Savings or differences are calculated from valid comparable quotes.
- The app does not use misleading labels such as â€œcheapestâ€ if the option is unavailable or the quote is stale.

## Module 4: Price changes and re-confirmation

**Objective:** Ensure riders understand material price changes before committing.

### Change scenarios
- Quote expires before booking confirmation.
- Pickup or destination is changed.
- Selected ride category changes.
- A market pricing input changes before confirmation.
- A promotion becomes ineligible or expires.
- A product-specific rule requires a refreshed quote.

### Functional requirements

| ID | Requirement | Priority |
|---|---|---|
| PR-021 | The system must identify when a quote is stale or no longer valid. | P0 |
| PR-022 | The app must obtain a refreshed quote when required. | P0 |
| PR-023 | If the refreshed amount differs materially from the previously shown amount, the app must display the updated amount and explain the change using an approved reason category. | P0 |
| PR-024 | The user must be able to accept the updated quote or return to ride selection. | P0 |
| PR-025 | The system must record the quote accepted by the user. | P0 |
| PR-026 | The system must prevent booking confirmation against an expired or invalid quote. | P0 |
| PR-027 | Any post-confirmation fare adjustment must follow the market's approved pricing, disclosure, and consent rules. | P0 |

### Acceptance criteria

- The user is never silently switched to a materially different pre-booking price.
- The updated quote is visible before the user confirms.
- The user can decline the updated quote without being forced to proceed.
- The accepted quote ID and timestamp are stored against the booking.
- A change reason is selected from a controlled, approved taxonomy.
- The system logs old and new quote references without exposing confidential pricing logic.
- Post-booking adjustments are applied only under approved rules and are itemized in the receipt.

## Module 5: Promotions, discounts, and credits

**Objective:** Make promotion eligibility and savings understandable.

### Functional requirements

| ID | Requirement | Priority |
|---|---|---|
| PR-028 | The app must allow users to enter or select an eligible promotion where supported. | P1 |
| PR-029 | The promotion service must validate eligibility, validity period, market, ride category, and other configured conditions. | P1 |
| PR-030 | The app must show whether a promotion is applied, rejected, or unavailable. | P1 |
| PR-031 | The app must explain a rejection using an approved user-friendly reason. | P1 |
| PR-032 | The app must show the discount amount and resulting total where applicable. | P1 |
| PR-033 | The final receipt must show the applied discount or credit. | P1 |

### Acceptance criteria

- An expired or ineligible code is not applied.
- The user sees a clear reason when an offer cannot be used, where disclosure is permitted.
- The displayed discount matches the pricing service response.
- The same promotion cannot be redeemed more than allowed by its rules.
- Promotion calculations and redemption are auditable.

## Module 6: Final fare and receipt

**Objective:** Explain the completed trip's final amount and any differences from the pre-booking quote.

### Functional requirements

| ID | Requirement | Priority |
|---|---|---|
| PR-034 | The system must generate a final fare record after trip completion. | P0 |
| PR-035 | The receipt must identify the original accepted quote or quote type. | P0 |
| PR-036 | The receipt must show the final amount and currency. | P0 |
| PR-037 | The receipt must itemize applicable additions, adjustments, taxes, tolls, and discounts. | P0 |
| PR-038 | Where final fare differs from the earlier amount, the app must show an explanation using approved reason categories. | P0 |
| PR-039 | The receipt must show payment status and payment method where appropriate. | P0 |
| PR-040 | The receipt must be accessible from trip history. | P0 |

### Acceptance criteria

- The final receipt reconciles to the final fare record.
- Any applicable adjustment has a reason code and supporting trip context.
- The receipt does not describe an estimate as a guaranteed price.
- The payment state is accurate: paid, pending, failed, cash recorded, or another market-approved state.
- The user can access the receipt after leaving the trip-completion screen.

## Module 7: Fare concern and support entry point

**Objective:** Make it easy for users to report a specific pricing issue.

### Functional requirements

| ID | Requirement | Priority |
|---|---|---|
| PR-041 | The user can select a completed trip and choose a fare-related help topic. | P1 |
| PR-042 | The app must pre-populate relevant trip and receipt references. | P1 |
| PR-043 | The user can select a concern category, such as unexpected charge, discount missing, or fare differs from expectation. | P1 |
| PR-044 | The app must create a support case or route the user to the approved resolution workflow. | P1 |
| PR-045 | The system must retain the relevant quote, final fare, and adjustment audit trail for authorized investigation. | P0 |

### Acceptance criteria

- A support request is linked to the correct trip.
- The user can review the concern before submission.
- The app confirms submission and provides a case reference when supported.
- Support agents can see the permitted pricing context without requiring the user to re-enter known information.
- Access to pricing and trip data is role-based and audited.

---

# 9. Pricing business rules

The pricing service must be the authoritative source for price calculation. The mobile app must not independently calculate or alter the payable fare.

## 9.1 Quote rules
- Every quote must have a unique ID.
- Every quote must include a currency and quote type.
- Quote validity must be explicit where applicable.
- The quote must be tied to the relevant route, ride category, and pricing-rule version.
- A quote must be refreshed when material inputs change.
- The system must preserve the quote accepted at booking time.

## 9.2 Fare component rules
- Only market-approved components may be displayed.
- Each component must have a clear label and amount or an explicit conditional status.
- Rounding must be deterministic and documented.
- Taxes and statutory fees must follow local rules.
- Discounts must be applied according to their eligibility and stacking rules.

## 9.3 Final fare rules
- The final fare must be generated from authoritative trip and pricing records.
- Adjustments must have a reason code and traceable source event.
- A post-booking change must comply with applicable user disclosure and consent requirements.
- The receipt must reconcile to the payment ledger.
- The app must distinguish a fare adjustment from a payment failure or duplicate authorization.

## 9.4 Reason taxonomy for rider-facing explanations

Possible approved reason categories:
- Route or destination changed
- Trip duration or distance changed
- Waiting time or stop adjustment
- Toll, airport, or statutory charge
- Promotion eligibility changed
- Quote refreshed before confirmation
- Other market-approved adjustment

The rider-facing reason must be accurate, understandable, and supported by the underlying record. Avoid generic or misleading explanations.

---

# 10. Wireframe descriptions

These are textual wireframe specifications for Design. They are not final visual designs.

## Screen 1: Ride options and price overview

**Purpose:** Help the rider compare options before selection.

**Layout**
1. Map area showing pickup, destination, and route preview.
2. Pickup and destination summary.
3. Scrollable ride-category cards.
4. Each card displays:
   - Ride category and short description
   - Estimated pickup time
   - Price
   - Price-type label, such as â€œEstimateâ€ or market-approved â€œUpfront priceâ€
   - Relevant availability status
5. Payment method summary.
6. Primary CTA: â€œChooseâ€ or â€œConfirmâ€ depending on flow.

**Interaction**
- Selecting a ride category highlights the card.
- Tapping the price or â€œFare detailsâ€ opens Screen 2.
- Changing pickup or destination triggers refreshed quotes.
- Stale quotes are clearly refreshed before confirmation.

## Screen 2: Fare breakdown bottom sheet

**Purpose:** Explain the displayed price.

**Layout**
- Header: â€œPrice detailsâ€
- Price type and quote validity
- Itemized components
- Conditional charges with plain-language explanations
- Promotion or credit line, if applied
- Total estimated or upfront amount
- Contextual help link
- Close control

**Interaction**
- Expandable rows explain unfamiliar components.
- The total remains visible.
- The user can return to ride options without losing valid selections.

## Screen 3: Updated price confirmation

**Purpose:** Obtain informed acceptance when a pre-booking quote changes materially.

**Layout**
- Header: â€œPrice updatedâ€
- Previous displayed amount, where appropriate and accurate
- New amount
- Approved reason for change
- Updated fare details link
- Primary CTA: â€œAccept updated priceâ€
- Secondary CTA: â€œBack to ride optionsâ€

**Interaction**
- Accepting records the new quote ID and user confirmation.
- Returning does not create a booking.
- The screen must not use manipulative visual emphasis to push acceptance.

## Screen 4: Promotion entry and validation

**Purpose:** Show offer eligibility and savings.

**Layout**
- Promotion code field
- Apply CTA
- Available eligible offers, if supported
- Applied discount and revised total
- Clear validation or rejection message

**Interaction**
- Applying a code triggers server-side validation.
- The app displays the authoritative result.
- A rejected code does not change the total.

## Screen 5: Post-trip receipt

**Purpose:** Reconcile the final trip amount.

**Layout**
- Trip date and summary
- Final amount and currency
- Payment status and method
- Itemized fare components
- Discounts and credits
- Adjustments with explanations
- Link to fare help
- Download/share receipt, where supported

**Interaction**
- â€œReview fareâ€ opens the relevant help workflow.
- Receipt content remains available from trip history.

## Screen 6: Report a fare issue

**Purpose:** Capture a specific issue with minimal user effort.

**Layout**
- Selected trip and final amount
- Concern categories:
  - Fare higher than expected
  - Discount not applied
  - Unexpected fee or adjustment
  - Duplicate or incorrect charge
  - Other fare concern
- Optional free-text details
- Submit CTA

**Interaction**
- Trip context is pre-populated.
- The user reviews and submits.
- A case reference or next-step message is displayed.

---

# 11. API specifications

The following are illustrative API contracts for discussion with Engineering. They are not Uber's actual private APIs.

## 11.1 API: Generate fare quote

**Endpoint:** `POST /v1/pricing/quotes`

**Purpose:** Generate a quote for a route and ride category.

**Request**
```json
{
  "market_id": "IN-DEL",
  "pickup": {
    "latitude": 28.4595,
    "longitude": 77.0266
  },
  "destination": {
    "latitude": 28.5355,
    "longitude": 77.3910
  },
  "ride_category": "STANDARD",
  "currency": "INR",
  "promotion_code": null
}
```

**Illustrative response**
```json
{
  "quote_id": "Q-789012",
  "quote_type": "ESTIMATE",
  "currency": "INR",
  "amount": 350.00,
  "created_at": "2026-11-07T10:00:00Z",
  "expires_at": "2026-11-07T10:02:00Z",
  "pricing_version": "DEL-2026-11-03",
  "components": [
    {
      "code": "BASE",
      "label": "Base fare",
      "amount": 60.00,
      "certainty": "ESTIMATED"
    },
    {
      "code": "DISTANCE",
      "label": "Distance component",
      "amount": 180.00,
      "certainty": "ESTIMATED"
    },
    {
      "code": "TIME",
      "label": "Time component",
      "amount": 80.00,
      "certainty": "ESTIMATED"
    },
    {
      "code": "OTHER",
      "label": "Other applicable components",
      "amount": 30.00,
      "certainty": "ESTIMATED"
    }
  ],
  "disclosures": [
    "This is an estimate. The final fare may vary under applicable trip and market rules."
  ]
}
```

## 11.2 API: Retrieve quote details

**Endpoint:** `GET /v1/pricing/quotes/{quote_id}`

**Purpose:** Retrieve a quote and its validity status.

**Expected response fields**
- Quote ID
- Quote type
- Currency
- Amount or range
- Component breakdown
- Created and expiry timestamps
- Validity status
- Pricing-rule version
- Disclosures

## 11.3 API: Validate or refresh quote

**Endpoint:** `POST /v1/pricing/quotes/{quote_id}/refresh`

**Purpose:** Refresh a quote if it expires or material inputs change.

**Request**
```json
{
  "reason": "QUOTE_EXPIRED",
  "current_route_version": "ROUTE-456"
}
```

**Illustrative response**
```json
{
  "previous_quote_id": "Q-789012",
  "new_quote_id": "Q-789013",
  "quote_type": "ESTIMATE",
  "currency": "INR",
  "amount": 365.00,
  "change": {
    "amount_difference": 15.00,
    "reason_code": "MARKET_PRICE_REFRESH",
    "display_message": "The estimate has been updated."
  },
  "expires_at": "2026-11-07T10:04:00Z"
}
```

## 11.4 API: Accept quote and create booking

**Endpoint:** `POST /v1/bookings`

**Purpose:** Create a booking against a valid quote accepted by the rider.

**Request**
```json
{
  "quote_id": "Q-789013",
  "ride_category": "STANDARD",
  "payment_method_id": "PM-123",
  "user_confirmation": true
}
```

**Illustrative response**
```json
{
  "booking_id": "B-456789",
  "booking_status": "SEARCHING",
  "accepted_quote_id": "Q-789013",
  "currency": "INR",
  "quoted_amount": 365.00
}
```

The endpoint must validate quote ownership, validity, market, route, ride category, and applicable policy. Use an idempotency key to prevent duplicate booking creation.

## 11.5 API: Retrieve final fare

**Endpoint:** `GET /v1/trips/{trip_id}/fare`

**Purpose:** Retrieve final fare and component-level adjustments.

**Illustrative response**
```json
{
  "trip_id": "T-456789",
  "accepted_quote_id": "Q-789013",
  "currency": "INR",
  "final_amount": 390.00,
  "components": [
    {
      "code": "BASE",
      "label": "Base fare",
      "amount": 60.00
    },
    {
      "code": "DISTANCE",
      "label": "Distance component",
      "amount": 190.00
    },
    {
      "code": "TIME",
      "label": "Time component",
      "amount": 90.00
    },
    {
      "code": "TOLL",
      "label": "Toll",
      "amount": 50.00
    }
  ],
  "adjustments": [
    {
      "reason_code": "TRIP_COMPONENT_RECONCILIATION",
      "display_message": "Final fare reflects completed trip details."
    }
  ],
  "payment_status": "PAID"
}
```

## 11.6 API: Submit fare concern

**Endpoint:** `POST /v1/trips/{trip_id}/fare-issues`

**Request**
```json
{
  "issue_category": "FARE_HIGHER_THAN_EXPECTED",
  "description": "The final amount appears higher than expected.",
  "receipt_reference": "R-456789"
}
```

**Illustrative response**
```json
{
  "case_id": "CASE-12345",
  "status": "SUBMITTED",
  "message": "Your fare concern has been submitted."
}
```

## 11.7 API and platform requirements

- Authentication and authorization on every protected endpoint.
- Server-side validation of all price and promotion inputs.
- Idempotency for quote acceptance, booking creation, and payment operations where applicable.
- Consistent currency precision and rounding.
- Versioned pricing rules and auditable quote snapshots.
- Correlation IDs for tracing requests across services.
- Rate limiting and abuse protection.
- Defined timeout, retry, and error-handling behaviour.
- No reliance on client-side calculations for authoritative fare amounts.
- Data retention and access policies aligned with legal and privacy requirements.

---

# 12. Data model and auditability

## 12.1 Core entities

| Entity | Important fields |
|---|---|
| PriceQuote | quote_id, user_id, market_id, route_version, ride_category, quote_type, currency, amount, created_at, expires_at, pricing_version |
| QuoteComponent | component_id, quote_id, code, label, amount, certainty |
| Booking | booking_id, user_id, accepted_quote_id, booking_status, created_at |
| Trip | trip_id, booking_id, actual_trip_context, trip_status, completed_at |
| FinalFare | fare_id, trip_id, currency, final_amount, calculation_version |
| FareAdjustment | adjustment_id, fare_id, reason_code, amount, source_event |
| PromotionRedemption | redemption_id, promotion_id, booking_id, discount_amount, status |
| Payment | payment_id, trip_id, amount, currency, status, provider_reference |
| FareIssue | case_id, trip_id, issue_category, status, created_at |

## 12.2 Audit requirements

For every quote and final fare, retain the information required to reconstruct the decision:
- Quote and fare identifiers
- Relevant input and route versions
- Pricing-rule version
- Component amounts and currency
- Quote creation and expiry timestamps
- Accepted quote reference
- User confirmation event
- Final fare and adjustment reason codes
- Payment reconciliation reference
- Actor or service that initiated a material change

Do not store more precise location or personal information than required. Define retention periods with Legal and Privacy.

---

# 13. Metrics and measurement framework

## 13.1 North Star Metric

**Completed rides with a successfully reconciled fare per eligible period**

This combines marketplace value with pricing integrity. It should be read alongside total completed rides and guardrail metrics.

## 13.2 Primary metrics

| Metric | Definition | Desired direction |
|---|---|---|
| Quote-to-confirmation conversion | Confirmed bookings Ã· eligible quote sessions | Increase |
| Fare comprehension rate | Users correctly answering price-comprehension questions Ã· users tested | Increase |
| Pricing-related abandonment | Eligible quote sessions abandoned after price exposure Ã· eligible quote sessions | Decrease |
| Fare-related support contact rate | Fare-related contacts Ã· completed trips Ã— 1,000 | Decrease |
| Quote-to-final variance rate | Trips whose final amount differs from the applicable pre-booking amount beyond defined rules Ã· eligible trips | Decrease where controllable |
| Promotion comprehension | Users correctly identifying applied discount and conditions Ã· users tested | Increase |

**Important:** Quote-to-final variance must be segmented by quote type. An estimate is not a guaranteed price. The metric should identify unexplained or incorrectly disclosed differences, not penalize legitimate, policy-compliant adjustments.

## 13.3 Guardrail metrics

| Metric | Why it matters |
|---|---|
| Driver acceptance rate | Ensure the experience does not harm marketplace supply |
| Driver cancellation rate | Monitor marketplace reliability |
| Rider cancellation rate | Detect unintended booking friction |
| Pickup ETA and reliability | Ensure price changes do not create poor service outcomes |
| Contribution margin per trip | Protect sustainable economics |
| Driver-partner earnings indicators | Monitor supply-side impact |
| Refund and adjustment rate | Detect pricing or billing problems |
| Payment failure rate | Protect transaction completion |
| Safety and complaint indicators | Prevent harmful incentives or disclosures |

## 13.4 Metric segmentation

Analyze metrics by:
- Market and city
- Ride category
- New versus returning rider
- Time of day and day of week
- Trip distance band
- Quote type
- Promotion usage
- App version and platform
- Relevant supply-demand conditions

Avoid drawing conclusions from aggregate metrics when segment-level impacts differ.

---

# 14. Experimentation plan

## Experiment 1: Fare breakdown visibility

**Control:** Existing price presentation.  
**Variant:** Prominent â€œPrice detailsâ€ entry point with itemized breakdown.

**Hypothesis:** Easier access to fare components improves price comprehension and may reduce price-related abandonment.

**Primary metric:** Fare comprehension rate.  
**Secondary metrics:** Quote-to-confirmation conversion and fare-related support contacts.  
**Guardrails:** Screen latency, booking completion, and accessibility task success.

## Experiment 2: Price type labelling

**Control:** Existing price label.  
**Variant:** More explicit â€œEstimateâ€ or market-approved upfront-price label.

**Hypothesis:** Clearer price-type labelling reduces misunderstanding about final fare certainty.

**Primary metric:** Correct user understanding of price certainty.  
**Secondary metrics:** Fare complaints and quote-to-confirmation conversion.

## Experiment 3: Price-change explanation

**Control:** Generic updated-price message.  
**Variant:** Approved reason-specific explanation with updated amount and user choice.

**Hypothesis:** A clear explanation increases trust when a pre-booking quote changes.

**Primary metric:** Acceptance rate of refreshed quotes, interpreted alongside comprehension.  
**Secondary metrics:** Abandonment after refresh and fare-related support contacts.  
**Guardrails:** No increase in accidental acceptance, complaints, or booking errors.

## Experiment design principles

- Define the eligible population and exclusions before launch.
- Randomize at a level that avoids cross-contamination.
- Pre-register primary metrics and guardrails.
- Calculate sample size and experiment duration using baseline rates and minimum detectable effect.
- Run long enough to account for weekday and weekend behaviour.
- Monitor novelty effects and segment differences.
- Do not declare success on conversion alone.
- Stop or roll back if safety, payment integrity, or material marketplace guardrails are breached.

---

# 15. Edge cases and failure handling

| Scenario | Expected behaviour |
|---|---|
| Quote service unavailable | Show a clear error; do not invent a price |
| Quote expires before confirmation | Refresh quote and show updated amount |
| Pickup or destination changes | Invalidate affected quote and generate a new one |
| Ride category changes | Generate a quote for the selected category |
| Currency mismatch | Block confirmation and log a high-severity error |
| Component totals do not reconcile | Do not display a misleading breakdown; fail safely and alert monitoring |
| Promotion expires during booking | Revalidate and explain the result before confirmation |
| Promotion service times out | Do not assume the discount was applied |
| User taps confirm repeatedly | Use idempotency to prevent duplicate bookings |
| Network fails after confirmation | Retrieve booking status before retrying creation |
| Pricing rules are updated during an active quote | Apply documented quote-validity and versioning rules |
| Route changes during the trip | Apply approved trip and fare rules; preserve an audit trail |
| Toll or fee data is delayed | Apply market-approved handling and disclose any resulting adjustment |
| Payment succeeds but response times out | Reconcile provider status before retrying payment |
| Duplicate payment callback | Deduplicate using provider transaction reference |
| Receipt generation fails | Preserve the fare record and allow receipt retrieval later |
| User disputes a fare | Create a case linked to quote, trip, fare, and payment records |
| Accessibility service cannot read a price label | Treat as a release-blocking accessibility defect for affected flow |

---

# 16. Security, privacy, legal, and compliance

- Encrypt data in transit and at rest.
- Use role-based access to pricing, trip, and payment records.
- Avoid exposing internal pricing or marketplace-sensitive signals.
- Ensure rider-facing explanations are accurate and approved.
- Comply with local consumer-protection, tax, transport, privacy, and payment requirements.
- Ensure promotion terms are visible and enforceable.
- Maintain auditable records of price acceptance and fare adjustments.
- Apply data minimization and retention limits.
- Conduct security, privacy, and accessibility reviews before launch.
- Provide an escalation path for potentially unlawful, discriminatory, or incorrect pricing outcomes.

---

# 17. Dependencies and stakeholder ownership

| Stakeholder/team | Responsibility |
|---|---|
| Product Management | Problem definition, scope, requirements, prioritization, and success criteria |
| UX Research | Rider research, comprehension testing, and usability validation |
| Product Design | Wireframes, interaction design, content hierarchy, and accessibility |
| Pricing Engineering | Quote calculation, pricing rules, versioning, and auditability |
| Rider App Engineering | Price presentation, quote refresh, and confirmation flow |
| Marketplace Engineering | Supply-demand context and relevant integrations |
| Payments Engineering | Payment status, reconciliation, and receipts |
| Data Science / Analytics | Instrumentation, experimentation, and measurement |
| Finance | Fare components, taxes, reconciliation, and unit economics |
| City Operations | Market rules, operational readiness, and driver-partner impact |
| Customer Support | Fare issue categories and resolution workflow |
| Legal / Privacy / Compliance | Disclosures, consumer rules, privacy, and regulatory review |
| QA | Functional, integration, regression, and edge-case testing |

---

# 18. Prioritization

Use MoSCoW for scope and RICE for comparing candidate enhancements.

## 18.1 MoSCoW

| Priority | Scope |
|---|---|
| Must have | Quote type, quote validity, price display, core fare breakdown, quote refresh, accepted quote record, final receipt, auditability |
| Should have | Promotion clarity, reason-specific price-change explanation, fare issue entry point |
| Could have | Personalized price education, advanced comparisons, optional timing suggestions |
| Won't have in MVP | New loyalty program, subscription pricing, automated personalized price negotiation, broad pricing-strategy redesign |

## 18.2 RICE scoring example

RICE = (Reach Ã— Impact Ã— Confidence) Ã· Effort.

The following scores are illustrative planning assumptions, not measured product data.

| Initiative | Reach (1â€“5) | Impact (1â€“5) | Confidence (1â€“5) | Effort (1â€“5) | Illustrative score |
|---|---:|---:|---:|---:|---:|
| Clear estimate/upfront label | 5 | 4 | 4 | 2 | 40.0 |
| Itemized fare breakdown | 5 | 4 | 4 | 3 | 26.7 |
| Quote expiry and refresh | 5 | 5 | 4 | 4 | 25.0 |
| Price-change explanation | 4 | 4 | 3 | 3 | 16.0 |
| Promotion clarity | 3 | 3 | 4 | 2 | 18.0 |
| Contextual fare issue flow | 3 | 3 | 4 | 3 | 12.0 |

Use the score as an input to prioritization, not as an automatic decision. Dependencies, compliance, technical risk, and marketplace impact may override the numerical ranking.

---

# 19. Rollout and launch plan

## Phase 0: Discovery and validation
**Activities**
- Baseline funnel and fare complaint analysis.
- Rider and driver-partner research.
- Pricing and policy review with Finance, Legal, and Operations.
- Confirm market-specific quote types and fare components.

**Exit criteria**
- Validated problem statement.
- Agreed MVP scope.
- Approved pricing terminology and disclosures.
- Baseline metrics and guardrails documented.

## Phase 1: Design and technical definition
**Activities**
- Prototype price overview, breakdown, refreshed quote, and receipt.
- Test comprehension and accessibility.
- Finalize API contracts and data model.
- Define event taxonomy and dashboards.

**Exit criteria**
- Design approval.
- API and event contracts reviewed.
- Security, privacy, and legal requirements documented.

## Phase 2: Build and internal testing
**Activities**
- Implement quote display and breakdown.
- Implement quote expiry and refresh.
- Persist accepted quote references.
- Implement final receipt and audit records.
- Run functional, integration, payment, and regression tests.

**Exit criteria**
- All P0 acceptance criteria pass.
- No unresolved critical or high-severity pricing defects.
- Quote-to-receipt reconciliation passes test scenarios.
- Monitoring and rollback mechanisms are ready.

## Phase 3: Limited pilot
**Activities**
- Release to a small eligible cohort in one selected market.
- Monitor conversion, comprehension, pricing complaints, technical errors, and marketplace guardrails.
- Collect rider and support-agent feedback.

**Exit criteria**
- No critical pricing, payment, privacy, or safety incidents.
- Guardrails remain within approved thresholds.
- Product and operations teams approve expansion.

## Phase 4: Gradual expansion
**Activities**
- Increase eligible traffic in controlled steps.
- Monitor market and segment-level outcomes.
- Validate localization, taxes, fee rules, and market-specific copy.
- Expand only after each checkpoint passes.

## Phase 5: Post-launch optimization
**Activities**
- Run controlled experiments.
- Review support contact reasons.
- Improve price education and receipt clarity.
- Reassess business and marketplace impact.

---

# 20. Launch readiness checklist

### Product and design
- [ ] Price type is clearly labelled.
- [ ] Fare breakdown is understandable and accessible.
- [ ] Quote expiry and refresh states are designed.
- [ ] Updated-price confirmation provides a genuine choice.
- [ ] Receipt explains applicable adjustments.

### Engineering
- [ ] Quote IDs and pricing versions are persisted.
- [ ] Client cannot override authoritative prices.
- [ ] Idempotency and retry behaviour are implemented.
- [ ] Quote and final fare records are auditable.
- [ ] Monitoring and alerts are configured.

### Data and analytics
- [ ] Events are instrumented and validated.
- [ ] Baseline dashboard is available.
- [ ] Experiment design and guardrails are approved.
- [ ] Segment definitions are documented.

### Finance, Legal, and Operations
- [ ] Fare components and rounding are validated.
- [ ] Taxes, fees, and disclosures are market-approved.
- [ ] Driver-partner and marketplace impacts are reviewed.
- [ ] Support workflows and escalation paths are ready.
- [ ] Privacy and security reviews are complete.

---

# 21. Open questions and decisions required

| ID | Question | Decision owner |
|---|---|---|
| OQ-01 | Which city and market will be used for the pilot? | Product / Operations |
| OQ-02 | Does the market use an estimate, upfront price, or both? | Product / Pricing |
| OQ-03 | Which fare components may be disclosed to riders? | Pricing / Finance / Legal |
| OQ-04 | What quote validity period is appropriate? | Pricing Engineering |
| OQ-05 | What constitutes a material price change requiring renewed confirmation? | Product / Legal |
| OQ-06 | Which post-booking adjustments are permitted? | Pricing / Legal / Operations |
| OQ-07 | What are the baseline conversion and fare-contact rates? | Analytics |
| OQ-08 | Which driver and marketplace guardrails must be monitored? | Marketplace / Operations |
| OQ-09 | What retention period applies to quote and fare audit data? | Legal / Privacy |
| OQ-10 | What are the approved launch thresholds and rollback triggers? | Product / Engineering / Operations |

---

# 22. Proposed acceptance test scenarios

| Test ID | Given | When | Then |
|---|---|---|---|
| AT-01 | A valid route and category are selected | User requests a quote | A quote with ID, currency, type, and validity is returned |
| AT-02 | Quote is an estimate | User views ride options | UI clearly labels it as an estimate |
| AT-03 | Quote is expired | User attempts confirmation | System refreshes or blocks confirmation |
| AT-04 | Refreshed quote differs materially | User returns to confirmation | New amount and approved reason are displayed |
| AT-05 | User rejects refreshed price | User selects back | No booking is created |
| AT-06 | Promotion is invalid | User applies the code | Discount is not applied and a clear message appears |
| AT-07 | Quote is valid | User confirms booking | Booking stores the accepted quote ID |
| AT-08 | Trip is completed | Final fare is generated | Receipt reconciles to final fare record |
| AT-09 | Final fare includes an adjustment | User views receipt | Adjustment and approved explanation are visible |
| AT-10 | User reports a fare issue | User submits the form | Case is linked to the correct trip |
| AT-11 | Payment response times out | Client retries status check | System reconciles status and avoids duplicate charge |
| AT-12 | Quote components do not sum to total | App receives response | Invalid breakdown is not presented as trustworthy; error is logged |

---

# 23. Definition of Done

The feature is ready for launch only when:

1. All P0 functional requirements are implemented.
2. All P0 acceptance and test scenarios pass.
3. Price type, quote validity, and disclosures are approved for the selected launch market.
4. Accepted quote and final fare records are traceable.
5. Payment and receipt reconciliation tests pass.
6. Analytics events and dashboards are validated.
7. Accessibility, privacy, security, and legal reviews are complete.
8. Support and operational teams are trained.
9. Rollback and incident-management procedures are documented.
10. Product, Engineering, Finance, Legal, and Operations sign off on launch readiness.

---

# 24. Product Manager's recommendation

Do not position this initiative as â€œshowing a fare breakdownâ€ alone. That is a UI improvement, not the full product problem.

The stronger product opportunity is **pricing confidence across the entire ride lifecycle**:

- Before booking: help the rider understand the amount and its certainty.
- At confirmation: ensure the rider knowingly accepts the applicable quote.
- During and after the trip: explain legitimate adjustments.
- After payment: provide a reconcilable receipt and an effective issue-resolution path.

The key trade-off is to improve rider trust without undermining driver-partner outcomes, marketplace reliability, or sustainable unit economics.

A successful launch should demonstrate improvement in price comprehension and pricing-related support contacts, while maintaining or improving booking conversion and staying within approved marketplace and financial guardrails.

---

# 25. Success metrics and launch thresholds

The source brief does not include baseline values or approved numerical targets. Set numeric targets, minimum detectable effects, sample sizes, and rollback limits with Analytics, Product, Finance, Marketplace Operations, and Legal before the pilot. Do not represent illustrative targets as observed Uber performance.

| Metric | Definition | Desired direction | Baseline / target | Type |
|---|---|---|---|---|
| Fare comprehension | Share of tested riders who correctly identify price type, what is included, and when the amount can change | Increase | Establish baseline; set target before experiment | Primary |
| Quote-to-confirmation conversion | Confirmed bookings divided by eligible quote sessions | Increase | Establish by market and quote type; set target before experiment | Primary |
| Pricing-related support contacts | Fare-related contacts per 1,000 completed trips | Decrease | Establish baseline and consistent issue taxonomy | Primary |
| Pricing-related abandonment | Eligible quote sessions abandoned after price exposure divided by eligible quote sessions | Decrease | Establish baseline; define attribution carefully | Secondary |
| Quote-to-final explanation coverage | Eligible final-fare differences with a valid, rider-visible explanation divided by eligible differences | Increase | Instrument after confirming market rules | Secondary |
| Fare dispute / adjustment rate | Fare-related disputes or adjustments per 1,000 completed trips | Decrease where attributable to confusion or errors | Segment legitimate adjustments from errors | Secondary |
| Promotion comprehension | Tested riders who correctly identify applied discount and conditions | Increase | Measure only where promotions are in scope | Secondary |
| Booking latency and completion | Time from quote display to confirmation, and successful booking completion | No material degradation | Pre-register acceptable bounds | Counter-metric |
| Rider and driver cancellation; driver acceptance; pickup ETA | Market health measures for eligible trips | No material deterioration | Market-specific bounds approved before launch | Counter-metrics |
| Contribution margin and driver-partner earnings indicators | Approved aggregate marketplace and financial measures | Remain within approved bounds | Finance and Marketplace to define; access-controlled | Counter-metrics |
| Payment failures, refunds, safety and complaint indicators | Rates for relevant incidents in the pilot population | No material deterioration; critical incidents trigger pause | Existing operational thresholds apply, to be confirmed | Counter-metrics |

Report results by market, city, ride category, new/returning rider, quote type, promotion use, and relevant supply-demand conditions where data and privacy policy permit. Do not declare success from conversion alone. Compare experiment results with a pre-registered control and include confidence intervals and exposure counts.

## 25.1 Metric instrumentation requirements

- Record quote shown, price type, quote timestamp/expiry, breakdown opened, quote refreshed, changed-price explanation shown, confirmation accepted/declined, trip completed, final receipt viewed, support issue submitted, and relevant outcome events.
- Use stable event definitions across control and variant; avoid collecting unnecessary precise location or personal information.
- Link events using approved pseudonymous quote, booking, and trip identifiers.
- Validate event completeness and reconciliation before interpreting experiment results.
- Treat missing or inconsistent instrumentation as a launch-readiness blocker for claims about impact.

# 26. Go-to-market strategy

This is a proposed rollout for the feature described in this PRD, not a claim about Uber's current launch process. The initial release should be a controlled product pilot, not a broad marketing launch, because the market, local fare rules, and baseline performance are not specified.

| Stage | Audience and activity | Gate to proceed |
|---|---|---|
| Market readiness | Select one market only after Product, Pricing, Finance, Legal, Privacy, Accessibility, Support, and Operations confirm applicable quote types, fare components, language, taxes/fees, and post-booking rules | Written market decision; approved disclosures; baseline and guardrails available |
| Internal / limited validation | Exercise quote, refresh, confirmation, receipt, and fare-help flows with test accounts and approved internal scenarios; verify analytics and support routing | No unresolved critical pricing, payment, privacy, accessibility, or reconciliation defects |
| Controlled rider pilot | Randomize eligible riders or sessions in the selected market using an approved experiment design; size the cohort based on power analysis, not an invented fixed percentage | Pre-registered success criteria met; counter-metrics within approved bounds; no critical incidents |
| Gradual expansion | Expand in controlled steps within the approved market; review segment-level results and operational capacity at each step | Each expansion checkpoint passes the same metric and incident gates |
| Ongoing operations | Maintain dashboards, fare-issue taxonomy, support guidance, incident owner, rollback path, and periodic review of market disclosures | Named owners and operational response process remain active |

## 26.1 Launch communications and enablement

- Use concise in-product copy that explains the feature and price certainty without promising that every final amount is fixed.
- Localize labels and explanations with market-specific Legal and Operations approval.
- Prepare support-agent guidance for quote types, permitted adjustments, receipt explanations, and escalation; do not train agents on unverified internal pricing explanations.
- Publish customer-facing help content only after the actual shipped behavior and applicable market rules are confirmed.
- Do not claim improved trust, lower disputes, or better conversion in launch communications until measured results support those claims.

## 26.2 Rollback and pause conditions

Pause or roll back for any critical pricing or payment integrity issue, materially misleading disclosure, privacy/security incident, accessibility blocker in the affected flow, or breach of a pre-approved marketplace or financial guardrail. Owners must define numeric thresholds and response times before launch; this source does not provide them.

# 27. Current-app evidence and candidate opportunity gaps

This section distinguishes published help guidance from hypotheses. Official help pages are not a full audit of every Uber app version, market, or ride type. They cannot establish that a feature is absent from the app. The launch market and app version must be selected before any gap is called confirmed.

| Published evidence | Candidate opportunity to investigate | What must be verified in the app / research |
|---|---|---|
| Uber says upfront pricing is available in most cities, while some cities use minimum or time-and-distance pricing instead. [Fare calculation guidance](https://help.uber.com/riders/article/how-are-fares-calculated-/?nodeId=d2d43bbc-f4bb-4882-b8bb-4bd8acf03a9d&valueOf=1374fa) | Make the applicable price type and certainty easy to understand for each ride option. This is an opportunity hypothesis, not evidence of a missing label. | In the target market, inspect each ride type and booking path. Test whether riders can tell an upfront price from an estimate or range and understand any limits. |
| Uber's published guidance lists circumstances that may change an upfront price, including changed destinations/stops, significant route or duration changes, tolls not included, and wait-time fees. [Fare calculation guidance](https://help.uber.com/riders/article/how-are-fares-calculated-/?nodeId=d2d43bbc-f4bb-4882-b8bb-4bd8acf03a9d&valueOf=1374fa) | Test whether pre-booking disclosures make conditional charges and post-booking change conditions understandable before riders request. | Observe price details, help text, and confirmation screens; run comprehension tasks for the market's specific conditions. |
| Uber says receipts explain why a final charge differs from the upfront price; separate help pages describe updated receipts and a fare-help route. [Fare calculation guidance](https://help.uber.com/riders/article/how-are-fares-calculated-/?nodeId=d2d43bbc-f4bb-4882-b8bb-4bd8acf03a9d&valueOf=1374fa), [updated receipt help](https://help.uber.com/riders/article/why-was-my-trip-receipt-updated?nodeId=0553f7db-0a17-40a2-a715-fa865d4f8ff8), [fare calculation in the app](https://help.uber.com/riders/article/how-was-my-fare-calculated?nodeId=39637ecf-bb58-4d02-95c8-cbbf52ff45c1) | Investigate whether explanations are sufficiently specific, easy to find, and connected to a low-effort resolution path. | With consent and privacy safeguards, review representative receipts and fare-help journeys; measure comprehension, task completion, and support contacts. Do not assume the flow is missing. |
| Uber's published help describes differing price models and availability by city/product, and notes that some fees or conditions vary. [Fare calculation guidance](https://help.uber.com/riders/article/how-are-fares-calculated-/?nodeId=d2d43bbc-f4bb-4882-b8bb-4bd8acf03a9d&valueOf=1374fa), [included rider price](https://help.uber.com/en/riders/article/whats-included-in-your-rider-price?nodeId=d6587e64-42d0-40af-8cea-78840cf55b3e) | Market-to-market consistency of terminology and fare detail may be a product opportunity, subject to local rules. | Compare approved flows across the chosen market and ride categories; identify only differences that cause measurable comprehension or task problems. |

## 27.1 Candidate feature to validate

**Candidate:** A market-configurable “price confidence” experience that puts price type, included fare components, conditional charges, quote freshness, and approved change conditions together before the rider requests a trip, then links any final-fare adjustment to a specific explanation and help route.

This is a proposed concept derived from the supplied PRD and the published help materials above. It is not a statement that Uber lacks these capabilities. Validate whether riders have the problem, whether current app flows already solve it, and whether any intervention improves comprehension without harming marketplace guardrails before committing to build.

## 27.2 In-app gap-finding checklist

For one selected market, ride type, app version, and test route:

1. Capture the quote screen and note whether the shown amount is a price, estimate, or range; record any timestamp or expiry information.
2. Find the pre-booking price details and list which components are itemized, conditional, or omitted. Compare the display with the market-approved terms, not with assumptions from another market.
3. Change a route input in a test scenario and observe whether the quote refreshes and whether the changed amount and reason are clear before confirmation.
4. Review a completed-trip receipt and trace the final amount to listed components, discounts, adjustments, and payment state.
5. Follow the fare-help flow and record steps, repeated data entry, available categories, expected resolution path, and accessibility issues.
6. Ask riders to explain the price and what could change it, without coaching. Record failed tasks and confusion as observed evidence.
7. Compare app observations with support-contact themes and fare-variance data before prioritizing a feature.

**Evidence rule:** Label an item “confirmed gap” only when app observation or user/operational data demonstrates a problem for the selected market and version. Until then, label it “opportunity hypothesis.”

# 28. Additional open decisions

| ID | Decision | Owner |
|---|---|---|
| OQ-11 | Which market, app version, ride types, and user journey will the in-app gap assessment cover? | Product / Operations |
| OQ-12 | What baseline period and eligible population will be used for the metric readout? | Analytics |
| OQ-13 | What numeric minimum success thresholds and counter-metric bounds are approved before experiment launch? | Product / Analytics / Finance / Marketplace |
| OQ-14 | Which existing app flows already meet the candidate requirements in the selected market? | Product / Design / Research |
| OQ-15 | Which rider-facing explanations and local disclosures are approved for the selected market? | Legal / Pricing / Operations |

**Document note:** This is an advanced educational product specification. All proposed APIs, sample payloads, thresholds, research sample sizes, metric definitions, and rollout steps are illustrative. They require validation with actual market data, Uber product policies, engineering architecture, and applicable laws before implementation.


