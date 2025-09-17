# CASE STUDY: Optimizing Sign-Up and Login Processes for 365DataScience.com

Sign-up processes can make or break customer acquisition. 365DataScience is an online learning platform for data science, offering lessons, quizzes, exercises, exams, gamified features, and a newsfeed. The platform requires subscription-based accounts, so optimizing sign-up and login flows is critical for user experience and revenue growth.

---

## Sign-Up and Login Flow Overview

- **Registration Options:**  
  1. Manual registration (email/password)  
  2. Social media login (Google, Facebook, LinkedIn)  

- **Potential Issues:** Visitors may encounter errors during sign-up or login.  

**Key Metrics to Track:**  
- Visitor-to-Registered Conversion Rate  
- Devices and Operating Systems used  
- Preferred Sign-Up & Login Types  
- Error Messages

<img width="226" height="312" alt="image" src="https://github.com/user-attachments/assets/7cf9b1bd-41fa-4f8b-8141-f1b7db15ee7e" />

---
## Basic Terminology

- **Visitors:** Individuals who visit the website but have not registered.  
- **Users:** Registered individuals on the platform.  
- **Customers:** Users with access to premium features (paying subscribers).  

**Performance Metrics:**  
- **Sign-Up Success Rate:** Successful sign-ups ÷ Total sign-up attempts  
- **Visitor-to-Registered Conversion Rate:** Successful sign-ups ÷ Total website visitors  

**Benchmark Conversion Rates:**  
- Average online course platform: 2–5%  
- Software services: ~3%  
- **Note:** High visitor numbers alone do not indicate success; conversion matters.

---

## Why Optimize the Sign-Up Process?

1. **First Impressions Matter:** Last step before engaging with content; can make or break the user experience.  
2. **Simplicity is Key:** Reduce friction by collecting only essential info upfront (email/password).  
3. **Options for Convenience:**  
   - One-click social media sign-up  
   - Password-less registration  
   - Phone number registration  
   - Auto-fill features (country code, address)  
4. **Behavior Analysis:** Track visitor behavior, identify challenges, and adjust the platform based on real usage.  

---

## Key Metrics Definitions

| Metric | Description |
|--------|-------------|
| Sign-Up Success Rate | % of users who start and successfully complete the sign-up process |
| Visitor-to-Registered Conversion | % of website visitors who become registered users |
| Sign-Up Conversion Rate | Overall effectiveness of website in converting visitors to users |

---

## Dashboard Design Concept

The Tableau dashboard should be structured into **three pages**:

### Page 1: Sign-Up Performance
- 3 graphs:  
  - Sign-Up Conversion Rate (bar/line)  
  - Visitor-to-Registered Rate  
  - Sign-Up Behavior Variations  
- 2 stacked bar charts:  
  - Most frequently used OS  
  - Device types (Mobile vs Desktop) with:  
    - Successful sign-ups  
    - Failed sign-ups  
    - Unsuccessful sign-ups that later succeed  
- Filters:  
  - Time Range  
  - Sign-Up Types  

### Page 2: Sign-Up Types & Errors
- Bar charts showing:  
  - Sign-Up Methods (Email, Google, Facebook, LinkedIn)  
  - Error Messages (colored by type)  
- Filters:  
  - Time Range, Sign-Up Type, Attempt, Device, OS  

### Page 3: Login Types & Errors
- Graphs for:  
  - Login Types (Email vs Social Media)  
  - Login Errors  
- Filters:  
  - Time Range, Device, OS  

---

## Data Integration & Database Structure

---

## Database Tables

| Table             | Key Columns                                | Description                     |
|------------------|--------------------------------------------|---------------------------------|
| Visitors          | visitor_id (PK), user_id (FK), first_visit_date | Track visitors and conversions |
| Students          | user_id (PK), date_registered             | All registered users            |
| Actions           | session_id (PK), visitor_id (FK), error_id, action_date, action_name | Track user activity            |
| Error Messages    | error_id (PK), error_text                  | Details of errors               |
| Student Purchases | purchase_id (PK), user_id (FK), purchase_date | Paid subscriptions             |
| Sessions          | session_id (PK), visitor_id (FK), session_os, session_date | Track device and OS usage

---

## Data Preparation & SQL Tables

Using SQL, we created the necessary tables for in-depth analysis and visualization in Tableau.  

The three primary tables for this analysis are:

1. **Conversion Rate**  
   - Tracks the sign-up conversion rate from **July 1, 2022, to January 2023** for all successful registrations.  
   - Key metrics:  
     - Total number of visitors  
     - Number of visitors who registered  
     - Visitors who did not make any direct purchases  
   - Purpose: Indicates overall effectiveness of the sign-up process and highlights opportunities for improving onboarding.

<img width="515" height="235" alt="image" src="https://github.com/user-attachments/assets/2dc306d9-39da-4b35-8c2c-e8ec1589a6d2" />

2. **Sign-Up Flow**  
   - Tracks visitor registration attempts, including:  
     - Sign-up method (Email, Google, Facebook, LinkedIn)  
     - Device type (Mobile vs Desktop)  
     - Operating system (Windows, iOS, Android, MacOS)  
     - Success and failure counts for each registration attempt  
   - Purpose: Identifies friction points in the registration process and user preferences.

<img width="912" height="235" alt="image" src="https://github.com/user-attachments/assets/cb213d16-4dd8-407c-8ea1-f67b7ba9a119" />

3. **Login Flow**  
   - Tracks login attempts for registered users, including:  
     - Login method  
     - Device and OS used  
     - Error types encountered  
   - Purpose: Measures post-registration experience and highlights login-related issues.

<img width="881" height="231" alt="image" src="https://github.com/user-attachments/assets/7daa54d3-b854-4e3b-ac2e-7841b3d49e56" />

> **Note:** These tables serve as the foundation for creating dashboards in Tableau to visualize trends, conversion performance, and error patterns.

## Creating the Dashboard

The Tableau dashboard is designed to provide a complete view of the sign-up and login performance for 365DataScience.com. It is divided into **three main pages**, each focusing on a specific part of the user journey.

---

### **Page 1: Sign-Up Performance Overview**

**Purpose:** Track overall sign-up behavior, conversion rates, and device/OS performance.  

**Features:**
- **Graphs:**  
  - Bar/line chart of **Sign-Up Conversion Rate**  
  - **Visitor-to-Registered Conversion Rate** over time  
  - Sign-Up behavior variations  
- **Stacked Bar Charts:**  
  - Most frequently used **Operating Systems**  
  - Device split (**Mobile vs Desktop**) showing:  
    - Successful sign-ups  
    - Failed sign-ups  
    - Unsuccessful sign-ups that later succeed  
- **Filters:**  
  - Time Range  
  - Sign-Up Types  

**Snapshot:**  
<img width="1362" height="769" alt="image" src="https://github.com/user-attachments/assets/96f2bbde-1b64-4f0c-8691-dccf05b3f912" />

---

### **Page 2: Sign-Up Types & Errors**

**Purpose:** Identify which sign-up methods are most used and which generate errors.  

**Features:**
- Bar charts of **Sign-Up Methods** (Email, Google, Facebook, LinkedIn)  
- Color-coded bar charts for **Error Messages** associated with each method  
- Filters for:  
  - Time Range  
  - Sign-Up Type  
  - Attempt Type  
  - Device  
  - Operating System  

**Snapshot:**  
<img width="1361" height="762" alt="image" src="https://github.com/user-attachments/assets/772bbd83-d679-4e41-be5d-34529f8b8a65" />

---

### **Page 3: Login Types & Errors**

**Purpose:** Analyze post-registration login experience and detect login-related friction.  

**Features:**
- Graphs showing **Login Methods** (Email vs Social Media)  
- Graphs of **Login Errors** and their frequency  
- Filters for:  
  - Time Range  
  - Device  
  - Operating System  

**Snapshot:**  
<img width="1360" height="761" alt="image" src="https://github.com/user-attachments/assets/bce61498-2967-42c4-b41d-8a3e28b1035b" />

## Analysis Report 

This section summarizes the findings from the Tableau dashboard, highlighting key metrics and actionable insights from the sign-up and login flow optimization.

---

## 🔹 Conversion Rate

### Sign-Up Options
- **Email:** requires email, password, name  
- **Social Media:** Google, Facebook, LinkedIn  

### Conversion Rates & Trends
- **November:** Highest visitors & conversion rate (**26%**) → boosted by 21-day free unlimited access campaign *(outlier)*  
- **Industry Benchmark:** 2–5% typical for online education platforms → other months align with benchmark  
- **December:** Drop in sign-ups, consistent with holiday seasonality  
- **Variation Drivers:** Marketing campaigns, seasonality, website activity, new course launches  

**Snapshot:**  
<img width="917" height="186" alt="image" src="https://github.com/user-attachments/assets/f822229f-2095-4293-8a2f-7a541f6059be" />

---

## 🔹 Device & Platform Insights

### Mobile vs Desktop
- Mobile registrations **2% higher than desktop**  
- Failure rate **3x higher on mobile** → 3.24% vs 1.16%  

### Operating Systems
- **Most used:** Windows → Android → iOS → MacOS  
- **Failures:** Android (2,309) far higher than Windows/iOS  
- **Retries:** Android (4,077 successful) → indicates repeated friction in process  

**Snapshot of All Signup Types:**  
<img width="925" height="240" alt="image" src="https://github.com/user-attachments/assets/fa1e498a-9612-4644-9b11-6520b0d50087" />

**Snapshot of email Signup Types:**  
<img width="534" height="242" alt="image" src="https://github.com/user-attachments/assets/efbffb73-32d2-4755-b9dc-04750cedea0c" /> 

**Key Insight:**  
➡️ Android email registration is the single largest source of sign-up failures. Prioritizing this issue could significantly reduce friction and improve overall conversion.

---

## 🔹 Sign-Up Types & Preferences

### Sign-Up Preferences
- **Google:** 54,614 visitors → most popular  
- **Email:** 23,319 visitors  
- **LinkedIn + Facebook:** ~11,000 combined  

**Why Google Dominates**
- **External:** Users already have Google accounts → easy/fast, fewer passwords  
- **Internal:** Google button larger and more visible; Facebook/LinkedIn smaller icons often overlooked   
- **Insight:** Email still widely used despite higher fail rates → possibly due to privacy concerns  

**Snapshot:**  
<img width="944" height="238" alt="image" src="https://github.com/user-attachments/assets/5bbd5a06-3b4d-4c80-b772-3d1f4680279f" />

---

## 🔹 Current State of Affairs: Success & Fail Rates

### Failure Percentages
We get these by the dynamic value which is dividing the total number of failures by the total number of signups: 
- Email → **6.5%** (2x higher than Google)  
- Google → **3.2%**  
- Facebook → **7.6%** *(highest)*  
- LinkedIn → **4.0%**  

**All Attempts:**
<img width="939" height="233" alt="image" src="https://github.com/user-attachments/assets/7ccdd6a9-7596-4cd5-9185-56616308a566" />

**Failed Attempts:**
<img width="938" height="232" alt="image" src="https://github.com/user-attachments/assets/4c02dce1-c432-493a-a218-1ea6066d9a13" />

### By Device
- **Desktop failures lower:** Google (708), Email (234)
<img width="1174" height="299" alt="image" src="https://github.com/user-attachments/assets/894c35cd-6793-4431-ac6b-7b3cc5d5674c" />

- **Mobile failures higher:** Email (1273), Google (1063), Facebook (351), LinkedIn (187)
- → Mobile is the **largest contributor to sign-up friction**
<img width="1177" height="297" alt="image" src="https://github.com/user-attachments/assets/96ac226b-d21d-4610-ba5b-eaf697a14dbd" />

### Error Details
- **Email (Mobile):** 600 invalid email, 459 missing name field → **85% of failed email registrations**
  <img width="1183" height="599" alt="image" src="https://github.com/user-attachments/assets/9fc7d14b-ad8e-41cd-ad7d-8a691984de95" />

- **Google:** 778 popup closures (browser blocking, unsuitable account, accidental close)
  <img width="1179" height="596" alt="image" src="https://github.com/user-attachments/assets/032f71f5-878f-4850-a9ee-f304666f440d" />

- **Facebook:** 349 unknown errors → requires technical investigation  
  <img width="1177" height="596" alt="image" src="https://github.com/user-attachments/assets/a57e87c2-1b65-4cd5-a14f-0a85c4561371" />

### Success Rates
- Google → **91%**  
- LinkedIn → **87%**  
- Email → **65%**  
- Facebook → **69%**  

**Insight:**  
➡️ Email has the lowest success rate → redesign email sign-up flow to mimic Google’s streamlined process.

---

## 🔹 Login Types

### Login Preferences
- **Email:** 217,948 sessions (most used)  
- **Google:** 211,594 sessions  
- *Note: multiple logins per visitor per session → counts exceed sign-up totals*  

### Failure Rates
- **Email:** ~25% failure (54,250 errors) → mostly invalid credentials (typos, case sensitivity, forgotten passwords)  
- **Google:** 13,135 errors → mostly popup closures  
- **Facebook/LinkedIn:** very low usage overall  

### OS/Device Patterns
- Android + Windows → most popup closure issues  
- iOS + MacOS → relatively stable  

### Fixes for Email Login
- Simplify password rules  
- Improve forgotten password flow  
- Add “Remember Me” option  
- Offer auto-generated strong passwords  

**Insight:**  
➡️ Email remains the most preferred login method, but with **high failure rates**. Fixing email login flow would reduce friction and improve retention.

## 🔹 Business Objective

### Focus Metrics
- **Visitor-to-Free Conversion Rate:** Percentage of visitors who register for a free account (did not subscribe within 30 minutes).  
- **Free-to-Paid Conversion Rate:** Percentage of free users who later become paying customers.  

### Why It Matters
- Higher visitor-to-free conversion → platform attracts the target audience.  
- Increasing free users → expands potential paid customer pool.  
- Optimizing website design and sign-up experience → boosts both free and paid conversions.  

### Business Goals
- Maintain steady user growth.  
- Increase % of registered users.  
- Boost visitor-to-free conversion rate.  
- Convert more free users into paid subscriptions.  

---

## 🔹 Hypothesis, Opportunity, and Actionable Insights

### 1. Hypothesis
- Website visitors experience problems with **email registration on mobile devices**.  
- Google sign-up is more successful than email; social media options (Facebook/LinkedIn) may be underutilized due to small icon size.  
- **Hypothesis:** Highlighting social media sign-up options and simplifying the registration flow will:
  - Increase visitor-to-free conversion rate.  
  - Lead to more paid subscriptions.  
  - Drive higher revenue and more registered users.
 
### 2. Opportunity Sizing
- **Objective:** Estimate potential growth from implementing the hypothesis.  
- Visitor-to-free conversion rate: currently **2–5%**, average **3.2%** (excluding November outlier).  
- **Target lift:** 10% → raises conversion to **3.52%**.  

**Impact Example (January):**
- Free users: 6,454 → 7,099 (+645)  
- Paid subscribers: 213 → 234 (+21)  
- Revenue: $6,490 → $7,020  

**Revenue Scaling:**  
- Every 10,000 visitors → ~14 additional free-to-paid conversions → +$420 revenue per 10k visitors  

**Considerations:**  
- Seasonality, marketing campaigns, competition, major events  
- Goal: Ensure improvements align with company objectives and investment is justified

### 3. Actionable Insights
Immediate recommendations to improve sign-up experience:
1. **Emphasize social media sign-up options.**  
2. **Increase prominence/size of social media buttons.**  
3. **Simplify email registration:**
   - Show only “Sign up with email” button initially.  
   - Reduce password complexity requirements.  
   - Simplify password restoration (“Forgot password”) process.  
4. **Investigate unknown Facebook signup errors** (collaborate with dev team).  
5. Consider mimicking **Google’s streamlined, user-friendly design** for email sign-up.

### 4. Expected Results
- Improved user experience across devices  
- Increased visitor-to-registered conversion rate  
- Higher free-to-paid subscription conversions → incremental revenue  
- Measurable outcomes validated through **A/B testing** of revised sign-up flow
