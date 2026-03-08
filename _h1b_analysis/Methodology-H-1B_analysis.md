---
title: Methodology - H-1B estimated probability of selection in lottery by wage level and degree
description: Methodology for analysis
date: 2026-03-06
---

Initial research and analysis through [ChatGPT 4 Turbo](https://platform.openai.com/docs/models/gpt-4-turbo) but the 128K [context window](https://www.ibm.com/think/topics/context-window) filled pretty quickly. I moved to a second session, and then switched to [Claude Pro Opus 4.2](https://www.anthropic.com/claude/opus). Graphs/charts and tables are by Claude using matplotlib.

For the distributions among wage levels and degree levels, I used USCIS’ [data from 2020 through 2024](https://www.federalregister.gov/d/2025-23853/page-60947). The number of unique registrants has varied greatly over the past seven years, from 118,000 in FY2020 (274,000 total registrations, but only 118,000 unique ones) to 450,000 in FY2023. FY2024 and 2025 were similar, around 445,000. But last year’s (FY 2026) registrations dipped sharply, to 339,000. This could be due to uncertainty over the new administration, higher registration fees discouraging speculative registrations, and the ongoing shift toward offshoring or nearshoring some jobs.

For this reason, I ran multiple scenarios. The initial scenario includes 370,000 registrants. This is also included in the four-scenario analysis, along with scenarios including 340,000 registrants (duplicating last year’s total), 290,000 registrants (reflecting a moderate continuation of the downward trend) and 220,000 registrants. The latter represents an amplification of these trends, plus the possible effect of the $100,000 fee, discouraging applicants from abroad. 

Unlike USCIS’ analysis and the cap total (85,000), I used 120,000 as the number of selections. This is closer to the actual number of selections recently – about [135,000](https://www.uscis.gov/sites/default/files/document/reports/ola_signed_h1b_characteristics_congressional_report_FY24.pdf) (see page 21) for FY 2025 and [120,000](https://www.uscis.gov/working-in-the-united-states/temporary-workers/h-1b-specialty-occupations/h-1b-electronic-registration-process) in FY2026. In FY 2024, it was [188,400](https://www.uscis.gov/sites/default/files/document/reports/ola_signed_h1b_characteristics_congressional_report_FY24.pdf). This reflects withdrawn and abandoned registrations and petitions, and denials by USCIS. Based on this, USCIS appears to underestimate the probability of selection in their analysis from December 2025. 

Using a higher initial estimate of registrations (370,000) in the initial scenario shifts the probabilities closer toward USCIS’ estimate, and employing several scenarios provides a range of estimates, while reflecting the likely reality of more than 85,000 selections needed to achieve that number of approvals.

To view the analyses in spreadsheet form, see this Google [drive](https://docs.google.com/spreadsheets/d/19RvlvrqS0_MqH0AUib_RHIwuh67Eecjs/edit?usp=sharing&ouid=114406606957833871318&rtpof=true&sd=true). It includes the analyses across four scenarios, a walkthrough, and the distributions among wage level and degree pool.  
