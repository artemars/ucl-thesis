# Predicting Return Direction from Volume-Sampled Limit Order Books  

Using LOBSTER level-5 snapshots, we convert raw event streams into volume-sampled bars and engineer domain-specific features to evaluate their effectiveness for high-frequency direction prediction. The task is formulated as a three-class classification problem with labels “up,” “flat,” and “down.”  


## Key Highlights  
- Converted raw LOBSTER order book and event data into volume-sampled bars, engineering features such as aggressor entropy rate and high–low volatility estimators. Feature informativeness was validated via multiple permutation tests.  
- Constructed a Transformer extension using rough path signatures, including a smoothing spline-based variant, and benchmarked against baselines. While the Rough Transformer [1] was competitive, the LSTM achieved the strongest performance, reducing loss by 5% relative to the naïve baseline.  
- Reported per-class binary F1 scores. The LSTM consistently outperformed on the minority “up” class, achieving a 25% higher F1 over the “down” class primarily through improved recall.  
- Applied SHAP values to identify reliance on negative return autocorrelation and confirmed additional feature contributions through a further ablation study under the multi-hypothesis framework.  
- Predictive structure is weak overall. Models lean heavily on stylised negative return autocorrelation to improve cross entropy loss (correct directional bets), while secondary engineered features provide noisy—though statistically significant—incremental signal.

## Abstract  
High-frequency trading (HFT) lacks a single, concrete definition. Most accounts emphasise its quick execution speeds and, some, a shift from clock time to a “volume clock.” Although HFT operations are highly secretive, the very fact that such firms profit at these horizons stands in tension with the strong form of market efficiency. Such firms find reason to depart from the market portfolio, consequently generating small, consistent alpha. Motivated by this insight, we assess the scope for alpha generation in a high-capitalisation stock. We engineer domain-informed features from LOB snapshots and event streams in volume time, casting the task as a three-class classification problem (down/flat/up). Our results indicate weak but genuine class structure, with models consistently more reliable on one minority class. The existence of label/feature structure is corroborated by permutation tests, and model attributions consistently assign the greatest influence to bar (executed) return—consistent with well-documented short-horizon negative autocorrelation in returns. Importantly, removing this feature still yields statistically significant structure, implying that our models leverage other bespoke features in concert with bar return: the microstructure regularity is not the sole driver. Overall, the signal tends to lower cross entropy while hurting accuracy, suggesting that bar return often nudges predictions in the right direction, whereas secondary features add only modest—often noisy—incremental information, consistent with the well-known difficulty of alpha generation in the absence of under-exploited data.  

## Results Snapshot – Test Performance  

On MSFT LOBSTER volume-bar data, we tested five models over 10 seeded runs.  

| Model              | Cross-Entropy Loss (↓) | 95% CI (Loss) | Accuracy (%) | Macro F1 (%)  | 
|--------------------|-------------------------|---------------|--------------|--------------|  
| **LSTM**           | **0.874 ± 0.001**      | [0.873, 0.874] | 62.93 ± 0.00 | **30.15 ± 0.00** |
| Rough Transformer  | 0.882 ± 0.001          | [0.882, 0.883] | 62.80 ± 0.00 | 29.02 ± 0.00 |
| Vanilla Transformer| 0.880 ± 0.003          | [0.878, 0.882] | 63.06 ± 0.00 | 27.23 ± 0.00 | 
| Spline Transformer | 0.888 ± 0.001          | [0.888, 0.889] | **63.06 ± 0.00** | 25.85 ± 0.00 | 
| Random Forest      | 0.888 ± 0.001          | [0.887, 0.888] | 63.06 ± 0.00 | 25.82 ± 0.00 | 

## References
[1] Moreno-Pino, F., Arroyo, Á., Waldon, H., Dong, X., & Cartea, Á. (2024).  
  *Rough Transformers: Lightweight and Continuous Time Series Modelling through Signature Patching.*  
  arXiv preprint arXiv:2405.20799.  
  Available at: [https://arxiv.org/abs/2405.20799](https://arxiv.org/abs/2405.20799)  
