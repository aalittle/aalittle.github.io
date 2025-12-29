---
title: "Reliability as a Growth Strategy"
date: 2025-01-15
description: "How a 36% crash rate reduction unlocked 31% user growth"
read_time: 5
---

Most teams treat reliability as a cost center. Fix bugs when customers complain. Patch crashes when the numbers get embarrassing. Ship features first, stabilize later.

We took the opposite approach. And it worked.

## The situation

Field Service Mobile serves 332,000 monthly active users. Frontline technicians in manufacturing, energy, and telecom depend on it to do their jobs. When the app crashes, a technician is standing in front of a customer with no way to complete the work order.

At the start of FY25, our user-perceived crash rate was 10.09%. Not catastrophic, but not great. More importantly, it was creating friction that limited our ability to grow into new accounts.

## The decision

We made a deliberate choice: allocate 8% of engineering capacity to stability work in Release 256.3. That meant saying no to feature requests. It meant pushing back on product timelines.

The bet was simple. If we could demonstrate reliability improvements, enterprise customers would trust us with larger deployments.

## What we did

Three things made the difference:

**1. Introduced code freeze discipline.** No changes in the final week before release. This sounds obvious, but we'd been shipping hot fixes right up to the deadline.

**2. Built Firebase Crashlytics velocity alerting.** We could now see crash rate trends in real-time instead of waiting for weekly reports.

**3. Customer beta testing.** We recruited design partners to run pre-release builds in production environments. They found issues we never would have caught in QA.

## The results

User-perceived crash rate dropped from 10.09% to 6.49%. A 36% reduction.

But here's what mattered more: MAU grew 31% year-over-year. Google Play ratings jumped from 2.6 to 3.8 weekly. Enterprise accounts like TKE expanded to 6,000+ technicians.

The reliability work didn't just prevent churn. It unlocked growth.

## The lesson

Reliability isn't a tax on feature development. It's a prerequisite for scale.

When your product is mission-critical, every crash is a broken promise. Customers don't expand deployments with vendors they don't trust.

We proved that stability investment compounds. The same customers who saw fewer crashes became our strongest advocates for new deployments.

---

*This is part of a series on engineering leadership lessons from scaling Field Service Mobile. Follow me on [LinkedIn](https://www.linkedin.com/in/aalittle/) for more.*
