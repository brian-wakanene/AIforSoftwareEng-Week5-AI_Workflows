
### **Ethics & Bias**

**How might biased training data affect patient outcomes?**  
Using biased data can lead to wrong prediction and overfitting. For example, having some job groups under represented can lead the model to underestimate readmission risk for these groups. This results in fewer interventions e.g., follow-up calls, increasing actual readmissions, worsening health disparities, and eroding trust in the system.

**1 Strategy to Mitigate Bias:**  
Apply a metric toenforce equality in representation of the data e.g., by race and income during training to ensure equal influence per subgroup.

---

### **Trade-offs**

**Interpretability vs. Accuracy in Healthcare:**  
High-accuracy complex models like deep neural networks often act as “black boxes,” making it hard for clinicians to trust or act on predictions. Interpretable models like logistic regression, decision trees allow explanation of risk factors supporting clinical decision-making and regulatory compliance, but may may sacrifice predictive performance.

**Impact of Limited Computational Resources:**  
With constrained resources, lightweight, interpretable models like logistic regression are preferred over deep learning. These require less memory, enable real-time inference on edge devices, and avoid latency in critical care settings, even if slightly less accurate. Prioritizing deployability and speed over marginal accuracy gains is essential when infrastructure is limited.