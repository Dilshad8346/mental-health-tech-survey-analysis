# Mental Health in Tech Survey — Data Analytics Project Report

**Analyst:** MD Dilshad
**Dataset:** OSMI Mental Health in Tech Survey (2014) — 1,259 responses, 27 columns
**Tools:** Python (pandas, NumPy), Chart visualizations
**Date:** July 2026

---

## 1. Business Problem

Tech industry mein mental health ek hidden cost hai — productivity loss, attrition, aur absenteeism ke through. Par companies ko pata nahi:

- Employees treatment kyun nahi lete, jabki problems hain?
- Company policies (benefits, leave, anonymity) ka actual impact kya hai?
- Kaunse levers pull karne se sabse zyada improvement milegi?

**Core question:** _Employee ke treatment-seeking behavior ko kaunse workplace factors drive karte hain, aur company kya concrete action le sakti hai?_

---

## 2. KPIs (Key Performance Indicators)

| KPI                                           | Value               | Kyun important            |
| --------------------------------------------- | ------------------- | ------------------------- |
| Treatment-seeking rate                        | 50.6%               | Primary outcome variable  |
| Family history → treatment lift               | +38.7 pts           | Strongest predictor       |
| Interview disclosure gap (mental vs physical) | 3.5% vs 16%         | Stigma ka direct measure  |
| Fear rate jab leave "very difficult"          | 62.2%               | Policy-culture link       |
| Benefits awareness gap (mid-size cos)         | 33–39% "Don't know" | Communication failure     |
| Male vs Female treatment gap                  | 45.4% vs 68.9%      | Help-seeking behavior gap |

---

## 3. Data Cleaning Summary

Raw data dikhne mein saaf tha, andar se ganda:

- **Age column:** 8 absurd values (−1726, 5, 329, 99999999999). Fix: values <15 ya >80 ko NaN kiya, median (31) se impute. Final range: 18–72.
- **Gender column:** 49 unique messy values (`M`, `Mail`, `msle`, `Cis Male`, `Male `...). Fix: 3 clean categories — Male (992), Female (251), Other (16).
- **Missing values:** state (515) → "Not Applicable" (non-US respondents); self_employed (18) → mode; work_interfere (264) → "Not Applicable"; comments (1095) → "No comment".
- **Whitespace:** trailing spaces stripped (`Female ` ≠ `Female` bug).
- **Column names:** sab snake_case, lowercase.
- **Result:** 0 nulls, 0 duplicates, correct dtypes.

**Documented judgment calls:** Age median-imputation ek trade-off tha (8 rows drop karna bhi valid option tha). Gender ko 3 categories mein collapse karna analytical simplification hai — inclusivity-focused analysis mein ye approach galat hoti.

---

## 4. EDA Approach

- Data mostly **categorical** hai, isliye Pearson correlation apply nahi hota. Relationships crosstabs (normalized) se nikale gaye.
- Har analysis mein outcome variable = `treatment`, aur ek workplace/demographic factor cross kiya gaya.
- Null results ko bhi report kiya gaya (remote work) — honesty > drama.

---

## 5. Charts & Chart Selection Reasons

| Chart                         | Type                        | Reason                                                                 |
| ----------------------------- | --------------------------- | ---------------------------------------------------------------------- |
| Treatment split               | Donut                       | Single binary proportion — simple part-to-whole                        |
| Family history × treatment    | Vertical bar                | 2-category comparison, gap highlight karna tha                         |
| Mental vs Physical disclosure | Grouped bar                 | Do series ka direct baseline comparison                                |
| Leave difficulty × fear       | Horizontal bar              | Ordinal categories + long labels; staircase pattern clearly dikhta hai |
| Company size × benefits       | 100% stacked horizontal bar | 3 categories (Yes/No/Don't know) ka part-to-whole across 6 groups      |
| Gender × treatment            | Bar                         | Category comparison with caveat labeling                               |
| Remote × treatment            | Bar                         | Null result dikhane ke liye — bars almost equal                        |

**Rule used:** Chart type data ka job decide karta hai, decoration nahi. Ordinal data → horizontal bars; part-to-whole → stacked/donut; comparison → grouped bars.

---

## 6. Insights (Observation → Why → Business Impact → Recommendation → Analyst Learning)

### Insight 1: Treatment rate 50.6% — par ye number biased hai

- **Observation:** 50.6% respondents ne treatment liya, 49.4% ne nahi.
- **Why:** Survey OSMI (mental health advocacy group) ne conduct kiya. Self-selection — jo log topic mein interested hain, wahi survey bharte hain. Real population rate isse kam hoga.
- **Business Impact:** Agar company is number ko benchmark maan le, toh wo apni problem underestimate karegi (unke employees mein untreated cases zyada honge).
- **Recommendation:** Is data ko directional insight ki tarah use karo, absolute benchmark ki tarah nahi. Internal anonymous survey karo apna baseline banane ke liye.
- **Analyst Learning — Selection Bias:** Data kahan se aaya, ye analysis se pehle ka sawal hai. "Compared to whom?" har sample se poochho.

### Insight 2: Family history sabse bada predictor hai (+38.7 points)

- **Observation:** Family history walon mein 74.2% treatment rate vs 35.5% bina family history ke.
- **Why (2 hypotheses, dono valid):** (a) Genetic/environmental risk genuinely zyada. (b) Jin gharon mein topic khula hai, wahan help lena normalized hai — stigma pehle se toota hua.
- **Business Impact:** Companies jo sirf "awareness campaigns" chalati hain, wo asli lever miss kar rahi hain — lever hai **normalization**, not information.
- **Recommendation:** Leadership storytelling programs — senior log apne experiences share karein. Ye "family openness" effect ko workplace mein replicate karta hai.
- **Analyst Learning — Confounding:** Ek hi number ke multiple explanations ho sakte hain. Data alone decide nahi kar sakta kaunsa sahi hai. Dono hypotheses report karna strength hai, weakness nahi.

### Insight 3: Stigma Gap — dataset ka sabse powerful finding

- **Observation:** Interview mein physical health problem 16% log discuss karenge; mental health sirf 3.5%. "Clear No" — physical: 39.7%, mental: 80.1%.
- **Why:** Mental illness = "weak/risky hire" perception. Physical illness ko sympathy milti hai, mental ko suspicion.
- **Business Impact:** Ye stigma hiring pipeline aur internal disclosure dono ko poison karta hai. Untreated employees = silent productivity loss.
- **Recommendation:** Disclosure-safe policies — anonymous EAP access, no-questions-asked mental health days. Jab tak disclosure unsafe hai, har wellness program underutilized rahega.
- **Analyst Learning — Baseline Comparison:** 3.5% akela meaningless hai. Physical health (16%) ke against compare kiya toh story bani. Har number se poochho: "compared to what?"

### Insight 4: Leave difficulty × fear — perfect staircase

- **Observation:** Fear of negative consequences: leave "very easy" → 8.7%; "somewhat easy" → 12.8%; "somewhat difficult" → 42.9%; "very difficult" → 62.2%. Monotonic pattern, ek bhi step ulta nahi.
- **Why:** Leave policy sirf rule nahi — company culture ka signal hai. Chhutti lena mushkil = "yahan mental health seriously nahi liya jaata" ka message.
- **Business Impact:** High-fear environment mein employees problems chhupate hain jab tak wo crisis na ban jayein — tab cost 10x hoti hai (sick leave, attrition, replacement hiring).
- **Recommendation:** Cheapest high-impact fix — leave approval se direct manager ko hatao, HR-direct ya self-serve route do. Process friction girao, fear apne aap girega.
- **Analyst Learning — Proxy Metrics:** "Culture" directly measure nahi hota; "leave kitni easy hai" uska measurable proxy hai. Unmeasurable ka proxy dhoondhna analyst ka core skill hai.

### Insight 5: Company size × benefits — do alag problems

- **Observation:** Benefits "Yes": 1–5 employees → 11.7%; 1000+ → 64.9%. Par mid-size companies (26–500) mein 36–39% employees ko _pata hi nahi_ ("Don't know") ki benefits hain ya nahi.
- **Why:** Chhoti companies — genuine budget constraint. Mid-size — benefits exist karte hain par communication zero. Paisa spend ho raha hai, value deliver nahi ho rahi.
- **Business Impact:** Mid-size companies benefits pe paisa kharch kar rahi hain jo employees use hi nahi karte kyunki unhe pata nahi. Pure waste.
- **Recommendation:** Mid-size ke liye: benefits _badhao mat_, _batao_ — onboarding module + quarterly reminder. Near-zero cost. Small companies ke liye: pooled/group EAP plans jo affordable hote hain.
- **Analyst Learning — "Don't know" is data:** Beginner ambiguous category ko drop karta hai. Senior analyst usme insight dhoondhta hai — yahan "Don't know" communication failure ka direct evidence hai.

### Insight 6: Gender gap — help-seeking ka, illness ka nahi

- **Observation:** Treatment rate — Female: 68.9%, Male: 45.4%, Other: 87.5% (n=16, small sample).
- **Why:** Ye illness gap nahi, **help-seeking behavior gap** hai. "Mard ko dard nahi hota" conditioning — men problems dabate hain, treatment avoid karte hain.
- **Business Impact:** Male-dominated tech workforce mein majority employees untreated hain — sabse bada silent risk pool.
- **Recommendation:** Male-targeted interventions — anonymous channels, peer-led groups. Generic wellness posters men tak nahi pahunchte.
- **Analyst Learning — Measurement vs Reality:** Column measure karta hai "treatment liya?", not "bimaar hai?". In dono ko confuse karna beginners ki sabse common galti hai. **Bonus — Small Sample Warning:** 16 logon ka 87.5% = 14 log. Chhote group ke percentages unstable hote hain — report karo par weight mat do.

### Insight 7: Remote work — no meaningful difference (null finding)

- **Observation:** Treatment rate — remote: 52.7%, office: 49.7%. Sirf 3 points ka difference — practically zero.
- **Why:** 2014 mein remote work rare aur self-selected tha — jo remote the, wo already flexible companies mein the.
- **Business Impact:** "Remote work mental health bigaadta hai" wali policy decisions is data se justify nahi hoti.
- **Recommendation:** Remote policy decisions ko is factor pe base mat karo; leave ease aur benefits communication pe focus karo — wahan signal strong hai.
- **Analyst Learning — Null Findings:** Difference na milna bhi valid result hai. Fake pattern dikhane wala analyst credibility khota hai; "no effect found" honestly report karna trust banata hai.

---

## 7. Trends, Correlations & Anomalies

### Trends

- Company size badhne ke saath benefits availability consistently badhti hai (11.7% → 64.9%).
- Leave difficulty badhne ke saath fear monotonically badhta hai (8.7% → 62.2%).
- Work interference jitna zyada, treatment rate utna zyada (Never: 14.1% → Often: 85.4%) — severity treatment drive karti hai.

### "Correlations" (categorical data note)

- Ye dataset mostly categorical hai — Pearson correlation apply nahi hota. Relationships normalized crosstabs se measure kiye gaye. Formal testing ke liye chi-square / Cramér's V appropriate hote (future scope).
- Strongest relationships: family_history × treatment, leave × mental_health_consequence, work_interfere × treatment.

### Anomalies

- **Age:** −1726 aur 99,999,999,999 jaise values — data entry / troll responses. Cleaned via median imputation.
- **Gender:** 49 free-text values — survey design flaw (free-text field instead of dropdown). Root cause: data collection stage pe validation missing.
- **Fear-supervisor paradox:** Jo log consequences se darte hain, unme se sirf 9.2% supervisor se baat karte hain vs 72.7% jo nahi darte. Fear communication ko kill kar deta hai — exactly jab communication sabse zyada zaroori hai.

### Root Causes (5-Whys style, stigma example)

1. Employees treatment kyun nahi lete? → Disclosure se darte hain.
2. Kyun darte hain? → Negative career consequences ka fear (62% jahan leave difficult).
3. Fear kyun hai? → Company signals (difficult leave, no anonymity) unsafe environment dikhate hain.
4. Signals kharab kyun hain? → Policies compliance ke liye bani hain, employee trust ke liye nahi.
5. **Root cause:** Mental health ko HR-checkbox treat kiya jaata hai, culture-design problem nahi.

---

## 8. Business Impact Summary

- **Untreated employees = hidden cost:** Presenteeism (kaam pe hain par productive nahi) absenteeism se zyada costly hoti hai.
- **Mid-size companies:** Benefits spend ka 35%+ waste ho raha hai kyunki employees ko awareness nahi.
- **High-fear cultures:** Problems crisis-stage tak chhupti hain — early intervention ka window miss hota hai.
- **Male-majority workforce:** Sabse bada untreated segment = sabse bada preventable attrition risk.

---

## 9. Recommendations (Priority Order)

1. **Leave process friction hatao** (HR-direct route) — cheapest, fastest, staircase pattern directly address karta hai.
2. **Benefits communication fix karo** (mid-size) — onboarding + quarterly reminders, near-zero cost.
3. **Anonymity guarantee karo** aur usko loudly communicate karo — anonymity "Yes" walon mein treatment rate 60.8% vs "Don't know" walon mein 45.3%.
4. **Male-targeted channels** banao — anonymous, peer-led.
5. **Leadership normalization** — senior storytelling, family-openness effect ko replicate karo.

---

## 10. Key Learnings (Analyst ke liye)

1. **Selection bias** — sample ka source pehle check karo.
2. **Confounding** — multiple explanations ko acknowledge karo.
3. **Baseline comparison** — har number se "compared to what?" poochho.
4. **Proxy metrics** — unmeasurable cheezon ke measurable proxies dhoondho.
5. **"Don't know" is data** — ambiguous categories mein insights hote hain.
6. **Measurement vs reality** — column ka meaning samjho, naam nahi.
7. **Null findings** — "no effect" bhi honest, valuable result hai.
8. **Data cleaning is judgment** — har decision defend karne layak hona chahiye.

---

## 11. Conclusion

Ye analysis dikhata hai ki tech workplace mein mental health treatment ka sabse bada barrier resources nahi, **safety aur communication** hai. Companies ke paas benefits hain (specially large ones), par stigma, difficult leave processes, aur poor communication unhe useless bana dete hain. Sabse actionable levers sasti hain: leave friction hatao, benefits batao, anonymity guarantee karo. Data ka clear message: **fear kam karo, treatment khud badhega.**

---

## 12. Future Scope

- **Statistical testing:** Chi-square tests aur Cramér's V se relationships formally validate karna.
- **Predictive model:** Logistic regression se treatment-seeking predict karna (family_history, work_interfere, leave as features).
- **Time comparison:** OSMI ke newer surveys (2016+) se compare karke post-COVID trends dekhna.
- **Text mining:** Comments column (164 non-empty) ka sentiment analysis.
- **Dashboard:** Power BI interactive dashboard with company-size aur country filters.

---

---

# 5-Minute Presentation Script

**[0:00–0:30] Opening**

"Good morning. Ek sawal se shuru karta hoon — agar aapko migraine ho, toh kya aap interview mein bata denge? Shayad haan. Aur agar anxiety ho? Is survey ke 1,259 tech professionals mein se sirf 3.5% ne haan bola. Aaj main aapko dikhaunga ki ye gap kyun exist karta hai, aur companies isko kaise fix kar sakti hain — data ke saath."

**[0:30–1:15] Dataset & Cleaning**

"Dataset hai OSMI Mental Health in Tech Survey — 1,259 responses, 27 columns. Raw data dikhne mein saaf tha par andar se ganda: Age column mein −1726 aur 99 billion jaise values, Gender mein 49 alag-alag spellings. Maine pandas se systematic cleaning ki — age outliers median se impute kiye, gender ko 3 categories mein standardize kiya, aur har missing value ko meaning ke saath fill kiya, blindly nahi. Important point: maine har cleaning decision document kiya, kyunki cleaning judgment hai, formula nahi."

**[1:15–3:30] Top 3 Insights**

"Teen sabse strong findings:

**Pehla — Stigma Gap.** Physical health problem 16% log interview mein discuss karenge, mental health sirf 3.5%. Same log, same interview — 4.5 guna ka difference. Ye batata hai ki problem awareness ki nahi, safety ki hai.

**Doosra — Leave Staircase.** Jahan mental health leave lena 'very easy' hai, wahan sirf 8.7% employees ko negative consequences ka dar hai. Jahan 'very difficult' hai — 62.2%. Perfect staircase, ek bhi step ulta nahi. Leave policy rule nahi, culture ka thermometer hai.

**Teesra — Communication Failure.** Mid-size companies mein 36–39% employees ko pata hi nahi ki unke paas mental health benefits hain ya nahi. Company paisa kharch kar rahi hai, employee ko khabar nahi. Fix almost free hai: onboarding mein batao, quarterly remind karo."

**[3:30–4:30] Recommendations**

"Meri top 3 recommendations, priority order mein: Ek — leave approval se manager ko hatao, HR-direct route do; sabse sasta, sabse fast fix. Do — benefits communication fix karo; ye spending problem nahi, messaging problem hai. Teen — anonymity guarantee karo aur usse loudly communicate karo — data mein anonymity walon ka treatment rate clearly zyada hai."

**[4:30–5:00] Closing**

"Ek line mein conclusion: is data ka message hai — fear kam karo, treatment khud badh jayega. Companies ko naye programs ki zaroorat nahi, existing programs ko _safe aur visible_ banane ki zaroorat hai. Thank you — questions ke liye ready hoon."

---

---

# 10 Viva Questions with Answers

**Q1. Aapne Age ke outliers median se kyun impute kiye, drop kyun nahi kiye?**
A: Sirf 8 rows affected thi (0.6% of data). Drop karna bhi valid tha, par main remaining 26 columns ki information waste nahi karna chahta tha. Median isliye (mean nahi) kyunki outliers ke hote hue mean khud distorted hota hai. Maine ye trade-off report mein document kiya hai — agar analysis age-critical hota, toh main drop karta.

**Q2. Gender ko 3 categories mein collapse karna kya information loss nahi hai?**
A: Haan, hai — aur maine ye explicitly acknowledge kiya hai. Ye analytical simplification hai jo comparison ke liye zaroori thi (49 categories pe crosstab meaningless hota). Agar project ka focus diversity/inclusivity hota, toh ye approach galat hoti. Cleaning decisions context-dependent hote hain.

**Q3. Aapne correlation matrix kyun nahi banaya?**
A: Kyunki data mostly categorical hai. Pearson correlation continuous variables ke liye hota hai. Categorical relationships ke liye normalized crosstabs use kiye; formal testing ke liye chi-square ya Cramér's V appropriate hote — wo future scope mein hai.

**Q4. Family history wala 39-point gap — kya ye causation hai?**
A: Nahi, correlation hai. Do competing explanations hain: genetic risk aur family openness (stigma normalization). Observational data se causation prove nahi hota — uske liye controlled study chahiye. Maine dono hypotheses report ki hain.

**Q5. "Don't know" responses ko aapne drop kyun nahi kiya?**
A: Kyunki "Don't know" khud ek insight hai. Mid-size companies mein 36–39% "Don't know" ka matlab hai benefits exist karte hain par communication fail hai. Ambiguous category ko drop karna information destroy karna hota.

**Q6. Sample biased hai toh analysis useful kaise hai?**
A: Absolute numbers (jaise 50.6% treatment rate) population pe generalize nahi honge — ye limitation hai. Par _relative_ patterns (leave difficulty → fear staircase, stigma gap) internally valid hain kyunki bias sab groups pe roughly equally apply hota hai. Directional insights ke liye data useful hai, benchmarks ke liye nahi.

**Q7. Sabse actionable insight kaunsa hai aur kyun?**
A: Leave staircase (Insight 4). Kyunki: (a) pattern strongest hai — perfect monotonic, (b) lever concrete hai — approval process change karna, (c) cost lowest hai — koi naya budget nahi chahiye. Actionability = impact × feasibility ÷ cost.

**Q8. Remote work ka koi effect nahi mila — kya ye analysis failure hai?**
A: Nahi, ye valid null finding hai. "No meaningful difference" report karna analyst ki honesty dikhata hai. Isse ek business decision bhi nikla: remote policy ko mental health argument se justify/attack mat karo, evidence support nahi karta.

**Q9. Aap is analysis ko aage kaise extend karenge?**
A: Teen steps: (1) chi-square tests se relationships formally validate karna, (2) logistic regression model — treatment predict karne ke liye with family_history, work_interfere, leave as features, (3) OSMI ke newer surveys se time-trend comparison, specially post-COVID.

**Q10. Data cleaning mein sabse bada risk kya tha?**
A: Silent assumptions. Example: work_interfere ke 264 missing values ko maine "Not Applicable" fill kiya — assumption ye ki in logon ko mental health issue nahi tha (survey skip logic). Agar ye assumption galat hai, toh unka treatment rate 1.5% wala pattern misleading ho sakta hai. Isliye har fill decision documented hai — reproducibility ke liye.

---

# Strong Opening & Closing

**Opening (viva/presentation ke liye):**
"Ek sawal se shuru karta hoon — migraine interview mein bata denge? Shayad. Anxiety? Is dataset ke 1,259 professionals mein se 96.5% ne kaha nahi. Mera project isi silence ko data se decode karta hai."

**Closing:**
"Is project ne mujhe ek cheez sikhaayi jo har dataset pe apply hoti hai: numbers khud kuch nahi bolte — unse sahi sawal poochna padta hai. 'Compared to what?' — ye teen words mere har future analysis ka starting point rahenge. Aur is data ka final message simple hai: fear kam karo, treatment khud badhega. Thank you."
