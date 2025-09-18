## A/B Test: Highlight Social Media Sign-Up Options

### Hypothesis
**H₀ (Null):** Modifying the sign-up screen (highlighting social media sign-up options) will **not** significantly affect the visitor-to-free conversion rate.  
**H₁ (Alternative):** The modified sign-up screen (Variant B) **will** increase the visitor-to-free conversion rate.

**Expected outcomes if H₁ is true**
- Higher social-media sign-ups.
- Higher visitor → free conversion rate.
- Larger pool of free users → more paid conversions → higher revenue.
---

### Test Design

**Variants**
- **Variant A (Control):** Original sign-up page.
<img width="290" height="433" alt="image" src="https://github.com/user-attachments/assets/992e1be1-67fb-497f-b042-2df2a2cb4917" />

- **Variant B (Treatment):** Modified sign-up page with:
  - Larger / more prominent social login buttons (Google, Facebook, LinkedIn).
  - Simplified email flow (less password friction, improved error messaging).
  - Any UI/UX microcopy improvements to reduce friction.
<img width="336" height="341" alt="image" src="https://github.com/user-attachments/assets/1eebbebb-062f-4740-a625-dc502be27c60" />

**Implementation notes**
- Software development team implements the two variants and A/B assignment.
- Use an A/B testing tool or framework with integrated analytics (e.g., Optimizely, VWO, internal solution).
- Ensure users are randomly & evenly assigned to A or B.

**Test parameters**
- **Duration:** 1 month  
- **Total visitors (example):** ~300,015 split roughly evenly  
  - Group A: `160,196` assigned  
  - Group B: `159,819` assigned  
- **Confidence level (α):** 95% (`α = 0.05`)  
- **Power:** target `0.8` (observed power: `>86%` in the example)  
- **Test type:** One-sided hypothesis test (we expect an increase)

---

### Key Performance Indicators (KPIs)
- **Primary KPI:** Visitor → Free conversion rate (free registered users / assigned visitors or / visitors who opened the signup window)  
- **Secondary KPIs:**  
  - Free → Paid conversion (downstream)  
  - Signup-window open conversion rate  
  - Average signup time  
  - Bounce rate on sign-up page  
  - Error counts (by device/OS/type)  
  - Session duration, CTR on sign-up elements

**Monitoring safeguards**
- Watch for crashes, big drops in signups, or spikes in errors.
- Avoid “peeking” (do not stop early and check repeatedly).
- Check for novelty effect (temporary lift because UI looks new).

---

### Execution Steps
1. Implement Variant A and Variant B in staging and test.  
2. Deploy experiment with random assignment and tracking keys (store `variant_id` with visitor/session).  
3. Track KPIs in real-time; log errors separately for debugging.  
4. Run test for full duration (1 month) or until sample size / event thresholds are met.  
5. After completion, run statistical analysis (one-sided test, calculate p-value and power).  
6. If statistically significant and positive, roll out Variant B; otherwise keep control or iterate.

---

### Statistical Setup & Interpretation

- **Null Hypothesis (H₀):** No effect on conversion rate.  
- **Alternative Hypothesis (H₁):** Conversion rate for Variant B > Variant A.  
- **Alpha (α):** 0.05 (95% confidence) — reject H₀ if `p < 0.05`.  
- **Power:** Target 0.8 (80%) — higher is better (example had >86%).  
- **P-value:** probability of observing results as extreme as the sample if H₀ is true.  
  - `p < 0.05` → strong evidence to reject H₀.  
  - `p >= 0.05` → insufficient evidence.

Use an appropriate test (z-test / chi-square / proportion test) depending on data and assumptions. For conversion rates between two independent proportions, a two-sample proportion z-test or online AB calculator works.

---

### Results (observed) - mock data

**Test group sizes**
| Variant | Assigned visitors |
|---------|-------------------:|
| A       | 160,196            |
| B       | 159,819            |

**Signup window open (subset of visitors) & free registrations**
| Metric | Variant A | Variant B |
|--------|----------:|----------:|
| Signup-window opens | 5,620 | 5,181 |
| Free registrations (from those opens) | 3,147 | 3,284 |
| **Window conversion rate** | **56.0%** | **63.4%** |
| Average signup time | 02:30 | < 02:00 |

**Statistical summary**
- **Test power:** > 86% (good)  
- **P-value:** `< 0.05` (statistically significant at 95% confidence)  
- **Interpretation:** Variant B outperforms Variant A on the primary KPI (signup-window conversion). Confidently implement Variant B.
<img width="920" height="314" alt="image" src="https://github.com/user-attachments/assets/e088c421-f9f5-4f6d-9a89-c922f0d89af3" />

---

## Further Analysis: Sign-Up Flow Optimization

The A/B test provided strong evidence that highlighting social media sign-up options improves the visitor-to-free conversion rate. However, additional analysis is required to strengthen long-term decision-making and uncover hidden opportunities.

### 1. Funnel Deep-Dive
- **Visitor → Sign-up window open**  
  Identify friction points where visitors abandon before opening the sign-up window.  
- **Sign-up window open → Free registration**  
  Already improved in Variant B, but further breakdown by device, OS, and browser can show if certain users still struggle.  
- **Free registration → Paid subscription**  
  Track if social logins result in *equal or higher* paid conversion rates compared to email sign-ups.

### 2. Error Message Analysis
- Categorize failed sign-up attempts by error type (e.g., password strength, OAuth timeout, email validation).  
- Investigate “Unknown” errors and prioritize fixes for recurring technical failures.  
- Determine whether Variant B reduced error frequency compared to Variant A.

### 3. Device and OS Segmentation
- Compare conversion rates across **desktop, mobile, and tablet**.  
- Explore whether mobile users show a stronger lift with social logins.  
- Prioritize UX/UI improvements on devices with the highest drop-off.

### 4. Cohort & Retention Tracking
- Measure retention of users who signed up via **email vs social login**.  
- Determine if one method correlates with **longer engagement** or **higher upgrade rates**.  
- Build cohorts (e.g., July 2022 vs January 2023) to compare long-term patterns.

### 5. Behavioral Insights
- Track **average sign-up time** by sign-up type.  
- Study click-through patterns on the sign-up page (e.g., if users hover on email before switching to Google).  
- Test alternative **button placements** or **default highlighting** of the most successful login method.

### 6. Additional Experiments
- **Micro-copy testing:** Adjust button text (“Sign up with Google” vs “1-click Google Sign-Up”).  
- **Default prioritization:** Place the most successful social login first and measure impact.  
- **Passwordless option:** Test phone number or magic link sign-ups to reduce friction further.  
- **Multi-step flow:** Compare single-screen vs two-step registration (basic info → additional profile setup).

### 7. Long-Term Business Impact
- Correlate **sign-up type → paid conversion → revenue contribution**.  
- Validate whether social login users churn faster or slower compared to email users.  

---

