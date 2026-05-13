# System Flow Documentation

This document outlines the user journeys and system logic for the Repayment Stress Simulator. It maps our features to specific use cases to ensure a smooth demo and implementation.

---

## Flow 1: New User Onboarding & First Impression
**Goal:** Take a first-time user (e.g., a sari-sari store owner) from landing to their first "Aha!" moment with minimal friction.

### 1. Landing Page (The Hook)
- **User Action:** Arrives via mobile web or app.
- **System Display:** 
  - Visual headline: "Is your next loan safe for your business?"
  - Quick summary of the "Repayment Stress Simulator."
  - **CTA:** [Start Free Stress Test] with an optional [Login for More Accurate Results] path.
- **Feature Integrated:** Value proposition highlighting the **True Cost Revealer**.

### 2. Guest Mode or Optional Login
- **User Action:** Starts the stress test without logging in, or logs in only if they want more accurate results.
- **Guest Requirement:** Ask only the six basic inputs needed for a fast estimate:
  - Amount to borrow
  - Total repayment
  - Due date
  - Normal daily cash left after basic expenses
  - Bad-day cash left after basic expenses
  - Minimum cash they must keep
- **Input Helper:** For the two cash-left fields, show an optional "Help me estimate" helper that calculates cash left from daily sales minus basic daily costs. The helper should not be required for the main guest flow.
- **Optional Login Requirement:** If the user wants better accuracy, collect a richer business profile later: store type, sales pattern, recurring expenses, restocking needs, household survival floor, existing debts, and slow days.

### 3. Optional Accuracy Upgrade: Adaptive Cash Flow Profile
**Goal:** Gather more accurate data only when the user chooses the login/profile path. Guest users can skip this and use the six basic inputs above.

- **A. Category Selection:**
  - System asks: "How do you earn your money?"
    - **Option 1: I sell products** (Sari-sari, Online shop, Market vendor)
    - **Option 2: I provide a service/gig** (Grab/Lalamove rider, Barber, Freelancer)
    - **Option 3: I have a fixed salary** (Office worker, Kasambahay, Factory worker)

- **B. Branching Questions (Based on Category):**
  - **If Seller (Option 1):**
    - "Average daily sales?"
    - "How much do you spend on inventory/restocking?"
    - "Which days are your slowest?"
  - **If Service/Gig Worker (Option 2):**
    - "Average daily earnings/tips?"
    - "Daily costs to work (Gas, Load, Tool maintenance)?"
    - "Which days do you typically take off or have low demand?"
  - **If Fixed Salary (Option 3):**
    - "Monthly take-home pay?"
    - "Monthly fixed bills (Rent, Utilities, Internet)?"

- **C. Common Foundations (For All Users):**
  - **Personal Survival Floor:** "How much do you *need* to take home for your family's basic daily needs (Food, Medicine, School)?"
  - **Existing Debt:** "Total daily or weekly payments for other loans you are currently paying off?"
  - **Income Baseline:** "On a normal day, how much cash is left after basic expenses?"
  - **Bad-Day Baseline:** "On a bad day, how much cash is left after basic expenses?"
  - **Minimum Cash Buffer:** "What is the minimum cash you must keep after repayment?"

- **Feature Integrated:** **Contextual Micro-Lessons** (A tip appears: "Your 'Survival Floor' is your most important number. If a loan cuts into this, it puts your family at risk.")

### 4. Personal Dashboard (The Hub)
- **User Action:** Arrives at the main screen.
- **System Display:** 
  - Current "Cash Health" status based on the guest estimate or saved profile.
  - Visual Feed: "Recent Loans Checked" (empty for new user).
  - **Primary CTA:** [Test a New Loan Offer].
- **Feature Integrated:** **Danger Zone Calendar** (Shows a preview of the current month with no risks marked yet).

---

## Flow 2: The Active Stress Test (Evaluating a Loan)
**Goal:** Transform raw loan numbers into a clear "Safe" or "Dangerous" decision for the user.

### 1. Loan Entry
- **User Action:** Clicks [Test a New Loan Offer] from the Dashboard.
- **Input Fields:** 
  - Loan Amount (e.g., ₱5,000)
  - Total Repayment (e.g., ₱7,500)
  - Due Date or Term (e.g., 30 days)
- **System Action:** Validates that the total repayment is greater than the amount borrowed.

### 2. The "True Cost" Reveal
- **System Display:** An immediate "True Cost" card appears before the full simulation.
  - "This loan costs you **₱50 for every ₱100** you borrow."
  - "Your daily interest cost is **₱83/day**."
- **Feature Integrated:** **True Cost Revealer**.

### 3. The Stress Simulation (The "Aha!" Moment)
- **System Action:** Runs the loan numbers against the user's guest inputs or saved profile.
- **Guest Formula:**
  - Days Until Due = number of days from today to the due date.
  - Projected Cash After Repayment = Normal Daily Cash Left After Expenses x Days Until Due - Total Repayment.
  - The borrowed amount is not counted as spare cash because it is assumed to be used for inventory, bills, or another borrowing purpose.
- **Gauge Logic:**
  - **GREEN:** Projected cash after repayment is greater than or equal to the user's minimum cash buffer.
  - **YELLOW:** Projected cash after repayment is non-negative but below the user's minimum cash buffer.
  - **RED:** Projected cash after repayment is negative.
- **System Display:** A large, dynamic **Cash Health Gauge**.
  - **GREEN:** "Manageable. You maintain your survival floor and inventory needs."
  - **YELLOW:** "Thin Buffer. A few bad sales days could force you to use your food budget to pay."
  - **RED:** "High Risk. You will not have enough cash to pay."
- **Feature Integrated:** **Dynamic Repayment Stress Simulator**.

### 4. Deep-Dive: Breakpoints & Calendar
- **System Display:** Below the gauge, show the "Limits of Survival."
  - **Breakpoint:** "Your income can drop by **30%** before this loan becomes unsafe."
  - **Calendar View:** A monthly calendar with "Danger Zones" highlighted where payments fall on slow days or "Restock Days."
- **Feature Integrated:** **Breakpoint Analysis** and **Danger Zone Calendar**.

### 5. Resolution & Suggestions
- **User Action:** If the result is Yellow or Red, the user sees a "Safety Path" card.
- **System Display:**
  - "To stay in the GREEN, try borrowing **₱3,500** instead."
  - "Or ask for a **45-day term** to lower the daily burden."
- **Feature Integrated:** **Safer Borrowing Suggestions**.

---

## Flow 3: The "What-If" Crisis (Simulating Disruptions)
**Goal:** Prove the loan's resilience by modeling real-world unpredictable events that affect income.

### 1. Percentage Drop Selection
- **User Action:** While viewing a Stress Test result (Flow 2), the user clicks on the "Test Your Resilience" section.
- **System Display:** A set of interactive **Impact-Based Buttons**:
  - **[Moderate Drop - 30%]:** Models a 30% income drop for 7 days.
  - **[Major Disruption - 60%]:** Models a 60% income drop for 7 days.
  - **[Zero Income - 100%]:** Models a 100% income drop for 3 consecutive days.
  - **[My Bad Day]:** Uses the user's bad-day cash-left input as a personalized stress case.
  - **[Custom Slider]:** Allows user to manually drop income by any percentage (0% to 100%) to see the exact moment the gauge turns Red.
- **Feature Integrated:** **Breakpoint & Percentage Drop Testing**.

### 2. Dynamic Impact Animation
- **System Action:** Instantly recalculates the **Cash Health Gauge** and **Breakpoint Analysis** based on the selected percentage drop.
  - For percentage buttons: Stress Daily Cash Left = Normal Daily Cash Left x (1 - Drop Percentage).
  - For My Bad Day: Stress Daily Cash Left = Bad-Day Cash Left After Expenses.
- **System Display:** The Gauge needle moves in real-time.
  - *Example:* A "Green" loan (Safe) may swing to "Red" (Danger) when a 60% income drop is activated.

### 3. Crisis Explanation & Impact
- **System Display:** A blunt, factual summary of the financial gap:
  - "You are short by **₱1,200** on your due date."
  - "Warning: Your cash is fully depleted."
- **Feature Integrated:** **Impact Status Reporting**.

### 4. Resilience Recommendation
- **System Display:** Updates the **Safer Borrowing Suggestions** to account for the crisis.
  - "To survive this income drop with this loan, the math shows you need a **₱2,500 cash buffer** on hand *before* you borrow."
  - "Alternatively, reducing the loan to **₱4,000** keeps your projected cash above the minimum buffer."

---

## Flow 4: The After-Action Report (Closing the Loan)
**Goal:** Review performance after full repayment to build the user's long-term financial record.

### 1. Loan Completion
- **User Action:** Marks the loan as [Fully Paid] on the Dashboard.
- **System Action:** Moves the loan from "Active" to "History."

### 2. Performance Summary
- **System Display:** A factual debrief of the loan period:
  - "Loan Term: 30 Days."
  - "Stress Days Survived: 5 (days where income was below average)."
  - "Lowest Cash Buffer: ₱400 (Your survival floor stayed safe)."
- **Feature Integrated:** **Impact Status Reporting**.

### 3. Profile Growth
- **System Display:** Updates the user's **Borrowing Profile**.
  - "You have successfully finished **1 loan**."
  - "Your resilience score has increased."
  - "Next Check: You are ready to test another loan offer."
- **Feature Integrated:** **Borrowing Profile Over Time**.

---

## All flows for the MVP/Prototype are now documented.
