# Explaining Financial Models by Deleting Transactions

We explain how we use the LOTO method to understand the real impact of each transaction on financial models.

Author: Denis Reis

---

How can you trust a model to support financial decisions if you can't understand its decisions? For us, this wasn't just an academic question. It was a core requirement for security, debugging, and responsible deployment. Here, at Nubank, we are leveraging transformer-based architectures to develop foundation models that learn representations directly from raw transaction data. By processing sequences of transactions as tokens, these systems can efficiently summarize complex financial behavior.

In this post, we explore the explainability of such models — the ability to examine how a single transaction influences the final prediction. Standard tools like SHAP and gradient-based methods come with limitations in our rapidly evolving environment. We adopt a simpler approach: Leave One Transaction Out (LOTO), which provides a good assessment of transaction impact while being compatible with virtually any model architecture.

## Why Explainability Matters

Understanding how inputs influence outputs is paramount for responsible deployment. We identified three key use cases:

- **Monitoring Model Drift**: Tracking changes in the relative importance of different transaction types over time signals shifts in customer behavior.
- **Debugging and Insights**: Understanding how different parts of the input contribute to predictions is invaluable for debugging anomalies and guiding feature selection.
- **Exploitability Prevention**: We define exploitability as a model vulnerability that malicious users could leverage to manipulate behavioral assessments. By examining transaction relevance before deployment, we can detect potential vulnerabilities.

## The Challenge with Standard Methods

SHAP [1] satisfies our explainability requirements — it's model-agnostic, provides directional values, and can group embedding dimensions into whole-transaction importance scores. However, it is computationally prohibitive for deep neural networks like ours.

We explored gradient-based alternatives such as Integrated Gradients [2] and Layer-Wise Relevance Propagation [3]. While more efficient, these methods presented three challenges: they cannot natively group components into per-transaction scores, they suffer from positional bias (assigning high importance to the first and last transactions regardless of content), and they are not architecture-agnostic.

[FIGURE: Bar chart showing importance scores from gradient-based methods concentrated at both ends of the transaction sequence, illustrating positional bias. Caption: "Figure 1: Gradient-based methods show strong positional bias in importance attribution"]

## Leave One Transaction Out (LOTO)

A consistent characteristic across all our model architectures is their ability to handle variable-length sequences. This provides a simple way to measure a transaction's marginal impact: remove it and observe the change.

The method is straightforward: remove each transaction one at a time and measure the difference between the new prediction and the original. This directly reveals the impact of that specific transaction in its fixed context. We apply LOTO in two modes: a **global analysis** (randomly removing one transaction per customer across a large dataset) and a **local analysis** (removing every transaction from a single customer's sequence for deep-dive debugging).

[FIGURE: Side-by-side comparison showing LOTO importance distribution (even) vs gradient-based distribution (biased toward sequence endpoints). Caption: "Figure 2: LOTO reveals a more even distribution of high-impact transactions compared to gradient-based methods"]

The main trade-off is that LOTO tests transactions individually, missing interaction effects. We accepted this for the gains in simplicity and computational efficiency.

## Conclusion

In our journey to build a reliable and interpretable financial AI system, we found that the simplest explainability method was also the most effective. LOTO provides clear, actionable, and unbiased insights that are robust to our evolving model architecture. By directly measuring the impact of removing each transaction, we can monitor model behavior, debug anomalies, and detect potential vulnerabilities — all critical capabilities for responsible deployment at Nubank's scale.

## References

[1] Lundberg, S. M., & Lee, S. I. (2017). A Unified Approach to Interpreting Model Predictions. Advances in Neural Information Processing Systems, 30.

[2] Sundararajan, M., Taly, A., & Yan, Q. (2017). Axiomatic Attribution for Deep Networks. Proceedings of the 34th International Conference on Machine Learning, 70, 3319-3328.

[3] Bach, S., Binder, A., Montavon, G., Klauschen, F., Muller, K. R., & Samek, W. (2015). On pixel-wise explanations for non-linear classifier decisions by layer-wise relevance propagation. PLoS ONE, 10(7), e0130140.
