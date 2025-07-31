# Neuromatch
Research project done under Neuromatch's Computational Neuroscience course

# Introduction & Hypotheses

Under 0% contrast—when no visual cue is present—mice must rely entirely on their internal expectations to decide which side to choose. Recent work (Teodorescu et al. 2023, preprint: https://www.biorxiv.org/content/10.1101/2023.07.04.547684v1) shows that 20–30% of brain regions encode these priors in their pre‑stimulus activity, with secondary motor cortex (MOs) among the strongest.

In this notebook, we examine whether MOs firing in the 0–0.5 s before stimulus onset predicts the trial’s block bias (probabilityLeft) and whether it carries information about the mouse’s eventual choice. Although choice decoding can often be achieved in downstream motor areas, our focus here is on the prior—what the mouse expects.

All sessions tagged with MOs were selected and, in this initial analysis, included all neurons on that probe rather than anatomically restricting to neurons from MOs. Future work should refine this selection and also extend it to other frontal regions (e.g., PL, ACCd).


## Hypotheses & Analyses

We ran four tests:

1. **Predict block bias (all blocks)**  
   - Use MOs firing 0–0.5 s before stimulus to predict `probabilityLeft` (0.2, 0.5, 0.8) on every trial.  
   - **Result**: 57.7 % accuracy (±10.6 %), p < 0.001.  

2. **Predict block bias (strong bias only)**  
   - Restrict to 0‑contrast trials with `probabilityLeft` = 0.2 or 0.8.  
   - **Result**: 58.5 % accuracy (±4.2 %), p < 0.001.  

3. **Predict choice (all contrasts)**  
   - Predict left vs right choice from pre‑stimulus activity over all contrasts.  
   - **Result**: 51.5 % accuracy (±3.4 %), p = 0.22 (not significant).  

4. **Predict choice (zero contrast)**  
   - Predict choice with no sensory evidence (contrast = 0).  
   - **Result**: 59.4 % accuracy (±7.4 %), p = 0.18 (not significant).  


## Interpretation

- MOs firing *before* the stimulus reliably reflects the block bias the mouse has learned—even with no visual cue.  
- Pre‑stimulus MOs activity does **not** reliably predict the actual choice, except weakly under zero contrast.  
- This supports the idea that **priors are encoded in frontal cortex**, while choice signals may rely on other regions or later processing stages.  


## Limitations

-  **Selected a single session** with MOs units and used **all sorted clusters** in that probe’s range, rather than sub-selecting only the most anterior or strictly “MOs‐only” channels.

- Results should be replicated across additional sessions and, ideally, across multiple frontal subregions (PL, ACCd) to confirm generality.

- **Pre-processing and dimensionality reduction** (e.g., PCA, regularization) were minimal—tuning these steps may enhance decoding performance.

- **Model hyperparameters** were left at defaults; systematic optimization (e.g., grid search, nested CV) could further boost accuracy.

- Increasing number of trials could also improve the models generalization.
