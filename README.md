# **Clinical Trial Monte Carlo Model**

#### This project is a computational framework designed to model the biological variance, regulatory hurdles, and financial risks of bringing a new pharmaceutical drug to market.

  The core of the model tracks the phase progression of a clinical trial, simulating monthly patient enrollment and their response to a new, candidate drug for which the user will provide the preclinical efficacy results. 
  To account for the fact that the preclinical efficacy rarely translates to human patients, the model applies a preclinical-to-clinical translation factor to the user’s initial input before calculating cohort responses for phase 3. These factors are highly optimistic compared to historical broad-spectrum averages, assuming that the simulated candidates utilize modern, highly targeted precision medicine.

  Early phases (1 and 2) test for basic safety and efficacy. These phases have more predictable costs and duration, because they require smaller patient cohorts, which makes them less susceptible to massive variations. 
  Phase 3, however, is usually the bottleneck of clinical research, because it requires bigger cohorts (1000 patients in our model), causing large variations in both cost and time-to-market. During this phase, the candidate drug’s performance is compared against the current Standard of Care, while also introducing unpredictable, real world FDA regulatory events such as safety signals that halt trials or fast-track designations that accelerate enrollment.

  The Monte Carlo simulation uses dynamic FDA approval thresholds that adapt to 3 different drug categories, requiring the new drug to demonstrate a statistically meaningful superior effect compared to the current SoC (for example, therapies for rare diseases require a much larger statistical margin of superiority because they typically rely on smaller patient cohorts, which results in lower statistical power).
  The model also performs efficacy sweeping, running thousands of simulations across various arbitrary efficacy levels and calculating their corresponding mean NPVs, generating a baseline viability curve.
  The simulation results are stored in a SQLite3 database and then analyzed.

  To evaluate whether the candidate drug is worth the financial investment, the simulator calculates the risk-adjusted Net Present Value, using a lognormal distribution to model the variations in peak market sales (including the upside potential of ‘blockbuster’ drugs to generate huge revenues), adjusted to the drug’s simulated clinical performance and also taking into account the R&D costs.

  The candidate’s clinical viability is analyzed by benchmarking its simulated performance against the minimum acceptable Probability of Technical and Regulatory Success (PTRS) from Phase 1 to Market, specific to its drug category. 

  By separating clinical risk from financial risk, the model delivers a verdict on whether the candidate should proceed to clinical phases or not. 

  
  The drug profiles from the simulator use parameters calibrated to reflect real-world clinical success rates, trial costs, and regulatory hurdles. The baseline probabilities and trial costs were derived from the BIO Clinical Development Success Rates 2011-2020 Report and papers such as “Estimation of clinical trial success rates and related parameters" (Wong, Siah, & Lo, 2019. Published in Biostatistics).

## **1. Oncology**
  - Clinical Characteristics: High biological complexity, severe patient morbidity, and tumor heterogeneity (mutation/resistance).
  - Trial Economics: Highest monthly burn rate ($3.0M) due to expensive clinical sites, complex dosing regimens, and high-frequency imaging/biopsies required for FDA endpoints.
  - Phase 2 & PTRS: Tumors frequently build resistance, resulting in a brutally low Phase 2 success rate (25%). Consequently, the end-to-end Minimum Acceptable PTRS is only 5%. An oncology drug that survives Phase 2 is highly valuable, balancing the extreme early-stage failure rates. 
  - The lowest clinical translation factor (70%)
  - Regulatory Margin: Standard of Care (SoC) is generally low (30%), requiring a standard 5% superiority margin to prove statistical significance.

## **2. Immunology**
  - Clinical Characteristics: Moderate biological complexity, targeting specific inflammatory or autoimmune pathways.
  - Trial Economics: Moderate monthly burn rate ($1.5M) reflecting outpatient monitoring and standard biomarker assays.
  - Phase 2 & PTRS: Better understood mechanistic pathways lead to a higher Phase 2 survival rate (45%) compared to oncology. The minimum acceptable end-to-end PTRS is 10%. 
  - High clinical translation factor (80%)
  - Regulatory Margin: The market is generally crowded with highly effective existing treatments, setting a steep Standard of Care baseline (50%). A new candidate must still clear a standard 5% superiority margin over this already-high bar.

## **3. Rare Diseases**
  - Clinical Characteristics: Highly targeted, usually monogenic conditions with extremely small patient populations and severe unmet medical needs.
  - Trial Economics: Lower monthly clinical burn rate ($1.0M) because there are fewer patients to actively monitor, though recruitment is notoriously slow
  - Phase 2 & PTRS: Because the exact molecular cause of the disease is usually known, Phase 2 efficacy success is high (55%). A venture capital or pharmaceutical firm will demand a higher end-to-end PTRS (15%) to justify funding. 
  - Extremely high translation factor (90%)
  - Regulatory Margin: Many rare diseases have no existing treatments, establishing a low Standard of Care (15%). However, because patient cohorts are so small, a larger statistical margin (+10%) is required to compensate for the trial's inherently low statistical power.

[Check out my LinkedIn profile](https://www.linkedin.com/in/maria-săvoaia-846132336)
