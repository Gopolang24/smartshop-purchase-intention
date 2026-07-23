# Understanding Classification Algorithms & Model Selection

1.  **Classification Fundamentals**

Classification is a supervised learning technique in which a model
trained on labelled data predicts a categorical class for new
observations, in contrast to regression, which predicts a continuous
value (Bishop, 2006). Binary and multi-class classification differ by
the number of possible outcomes, two for binary, three or more for
multi-class (Hastie, Tibshirani and Friedman, 2009). Both appear in the
SmartShop Online dataset: VisitorType is multi-class, recording each
session as New_Visitor, Returning_Visitor, or Other, while Revenue is
binary, recorded as TRUE or FALSE. Revenue is treated as the target
variable for two reasons. Technically, it satisfies the definition of a
classification target set out above, a discrete, two-valued outcome
rather than a continuous measure. Practically, it is also the outcome
the business cares about: PageValues, BounceRates, VisitorType and the
remaining fields are useful only to the extent that they help explain
whether a session converts. Predicting Revenue therefore meets both the
formal requirement for a classification problem and the organisation\'s
actual objective of identifying visitors likely to purchase.

2.  **Understanding Support Vector Machines**

A Support Vector Machine separates classes using a hyperplane, a
decision boundary sitting in a space with as many dimensions as there
are predictor variables (Cortes and Vapnik, 1995). It doesn\'t just fit
any hyperplane that happens to separate the classes, though. It
specifically looks for the one that maximises the margin, the distance
between the boundary and the nearest point from each class. What\'s
interesting is that this distance is decided entirely by those nearest
points, the support vectors, since they\'re the only observations
actually constraining where the boundary sits. Take away a point far
from the boundary and nothing changes. Take away a support vector and
the whole hyperplane shifts. The reason a wider margin is preferred
comes down to how well it holds up on new data: a boundary that sits too
close to one class leaves little room for natural variation, and
anything that lands near that edge risks being misclassified.

Because SVM classifies by calculating distances between points, any
feature on a larger scale ends up dominating that calculation, whether
or not it\'s actually the more important variable. This shows up clearly
in the SmartShop dataset. ProductRelated_Duration reaches as high as
63,973, while BounceRates never climbs past 0.2, so left unscaled,
duration would swamp the margin calculation entirely. Standardisation
fixes this by subtracting each feature\'s mean and dividing by its
standard deviation, which puts every variable on a comparable footing
with a mean of zero and a standard deviation of one (Kuhn and Johnson,
2013).

3.  **Model Comparison and Selection**

Table 1 compares Logistic Regression, Support Vector Machines, and
Decision Trees across the five dimensions relevant to the SmartShop
Online business problem.

  ----------------------------------------------------------------------------
  **Dimension**      **Logistic         **Support Vector     **Decision Tree**
                     Regression**       Machine**            
  ------------------ ------------------ -------------------- -----------------
  Interpretability   High; coefficients Low; the decision    High; splits
                     indicate the       boundary,            follow a readable
                     direction and      particularly with    if-then rule
                     magnitude of each  non-linear kernels,  structure that
                     predictor\'s       is difficult to      stakeholders can
                     effect             explain to a         inspect directly
                                        non-technical        
                                        audience             

  Computational      Efficient to train Computationally      Fast to train and
  efficiency         and predict,       expensive to train   predict, scales
                     scales well to     on large datasets,   reasonably well
                     large datasets     especially with      
                                        non-linear kernels   
                                        (Géron, 2022)        

  Need for feature   Recommended, since Required, as         Not required,
  scaling            gradient-based     classification       since splits are
                     optimisation       relies on distance   threshold-based
                     converges faster   calculations         rather than
                     on scaled features                      distance-based

  Ability to handle  Assumes a linear   Captures non-linear  Captures
  complex            decision boundary, boundaries via the   non-linear
  relationships      limited without    kernel trick         relationships and
                     manual feature                          feature
                     engineering                             interactions
                                                             naturally through
                                                             recursive
                                                             splitting

  Typical business   Regulated contexts High-dimensional     Contexts
  applications       requiring          classification tasks requiring
                     transparent        where predictive     transparent,
                     justification,     accuracy outweighs   rule-based
                     such as credit     the need for         decisions, such
                     scoring            explanation          as marketing
                                                             segmentation
  ----------------------------------------------------------------------------

Comparing several models before deployment is good practice because no
single algorithm performs best in every context; each carries
assumptions that suit some data structures and business problems better
than others (James et al., 2021). If SmartShop Online deployed only a
Decision Tree, it might conclude that conversion comes down to a handful
of clear thresholds, missing more complex interactions between variables
like BounceRates and ProductRelated_Duration that a non-linear SVM would
actually pick up, leaving real predictive accuracy unused. If SmartShop
Online deployed only an SVM, by contrast, it would risk a model that
marketing managers cannot interpret or justify internally, which erodes
trust in its recommendations regardless of accuracy. This points to a
genuine tension between predictive performance and practical usability.
A business will sometimes deliberately forgo the best-performing model
in favour of one that is interpretable, faster to retrain, or cheaper to
run in production (Provost and Fawcett, 2013). Comparison is what allows
that trade-off to be made deliberately, rather than by default, ensuring
the model selected reflects both statistical performance and
SmartShop\'s actual operational constraints.

4.  **Ethical Considerations**

Two ethical considerations are particularly relevant here. First,
although session-level behavioural data such as browsing duration or
page visits does not include a name or ID number, it can still
constitute personal information under the Protection of Personal
Information Act (POPIA) when combined with identifiers such as IP
address or device data, meaning SmartShop Online remains obligated to
collect and process it lawfully, with appropriate consent and purpose
limitation (Republic of South Africa, 2013).

Second, accountability becomes difficult to establish once model output
is used to target or exclude visitors from marketing campaigns. This
risk is more pronounced for less interpretable models such as SVM, where
neither the organisation nor the customer can easily establish why a
particular prediction was made, weakening the organisation\'s ability to
justify or challenge a contested decision.

5.  **Conclusion**

This report has established that predicting Revenue is fundamentally a
classification problem, and that Logistic Regression, SVM, and Decision
Trees each address it through different trade-offs between
interpretability, computational efficiency, and the ability to capture
complex relationships. No single algorithm satisfies all of SmartShop
Online\'s requirements simultaneously, which is precisely why comparing
models, rather than adopting the first one that performs adequately,
allows the organisation to select a predictive model deliberately,
balancing accuracy against practical constraints such as
interpretability and accountability. Part B will apply this comparative
approach directly to the dataset.

**References**

Bishop, C.M. (2006) Pattern Recognition and Machine Learning. New York:
Springer.

Cortes, C. and Vapnik, V. (1995) \'Support-vector networks\', Machine
Learning, 20(3), pp. 273--297.

Géron, A. (2022) Hands-On Machine Learning with Scikit-Learn, Keras, and
TensorFlow. 3rd edn. Sebastopol, CA: O\'Reilly Media.

Hastie, T., Tibshirani, R. and Friedman, J. (2009) The Elements of
Statistical Learning: Data Mining, Inference, and Prediction. 2nd edn.
New York: Springer.

James, G., Witten, D., Hastie, T. and Tibshirani, R. (2021) An
Introduction to Statistical Learning: with Applications in R. 2nd edn.
New York: Springer.

Kuhn, M. and Johnson, K. (2013) Applied Predictive Modeling. New York:
Springer.

Provost, F. and Fawcett, T. (2013) Data Science for Business: What You
Need to Know about Data Mining and Data-Analytic Thinking. Sebastopol,
CA: O\'Reilly Media.

Republic of South Africa (2013) Protection of Personal Information Act,
No. 4 of 2013. Pretoria: Government Printer.

