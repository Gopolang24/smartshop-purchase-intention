# Model Evaluation, Business Interpretation & Recommendations

1.  **Interpretation of Classification Results**

Three predictive models were built and tested to see which one could
best identify online shoppers likely to buy: Logistic Regression, a
Support Vector Machine, and a Decision Tree. The Decision Tree came out
on top. It correctly identified 54% of actual purchasers in the test
data. The other two models only managed 36 to 37%, meaning the Decision
Tree caught roughly half again as many genuine sales in progress as
either of the others.

Logistic Regression and the Support Vector Machine performed almost
identically to each other, both correct overall in roughly 88% of cases,
but both too cautious: each missed close to two in three real purchases.
This closeness is telling. Both models work by drawing a single straight
dividing line through the customer data, and the fact that they land on
nearly the same result suggests a simple straight-line boundary captures
a meaningful share of what separates browsers from buyers, but not all
of it.

The Decision Tree\'s advantage points to what that straight line misses.
Purchasing behaviour on this site does not move up or down smoothly with
any one browsing signal on its own. It more likely depends on
combinations of signals, page value and engagement together, for
example, rather than any single behaviour in isolation. In business
terms, predicting a sale is less about tracking one metric closely and
more about recognising a pattern of behaviour building at once.

2.  **Business Recommendation**

SmartShop Online should deploy the Decision Tree model to identify
shoppers likely to complete a purchase. Across every measure that
matters for this business, catching real buyers, cost, and speed, it is
the stronger choice of the three models tested.

The Decision Tree identifies roughly half again as many real purchasers
as the other two models. That matters because it means far more
customers get flagged while there is still time to act, before they
leave the site for good. A discount, a product suggestion, a chat
prompt, any of these only work if the system catches the customer in
time. The other two models are too cautious. They let most at-risk
purchases slip through unnoticed, and those are exactly the customers
SmartShop Online needs to reach.

The Decision Tree also shows its reasoning, and the marketing team can
actually use that. It points to which browsing behaviours are driving a
predicted purchase, viewing high-value pages, staying engaged instead of
bouncing off quickly, rather than just spitting out a verdict nobody can
explain. That means campaigns can target real, explainable behaviour
instead of chasing a prediction no one understands.

It is also the cheapest of the three to run. Training takes a fraction
of a second, so the model can be refreshed regularly as new browsing
data comes in, without the delay or computing cost the alternative
approach would require to keep pace with the same schedule.

Missing a genuine buyer costs SmartShop Online more than sending an
unnecessary promotional message to someone who was not going to buy
anyway, and the relative size of those two costs is what should drive
the choice of model (Provost and Fawcett, 2013). On that basis, the
Decision Tree is the strongest of the three: it is the only one built
around getting that exact trade-off right. On predictive strength,
transparency, and cost together, it is the model best suited to
deployment, and the clear recommendation of this analysis.

3.  **Reflection on the Predictive Modelling Process**

The trickiest moment for me was not a coding error, it was a numbers
problem. The SVM-versus-Logistic-Regression timing ratio would not sit
still. Across different runs of my own notebook, the same code gave me
31.5 times at one point and 35.9 times at another, no changes made in
between, just the ordinary noise of rerunning something on a real
machine. It taught me that wall-clock timing is genuinely noisier than
it looks, and that the fix is not finding the \"right\" number, it is
picking one clean, final run and being upfront that a repeat run would
likely land somewhere close but not identical.

The clearest gap I closed was on the dummy variable trap. My first
explanation was just that it avoids perfect predictability, true but
shallow. I only actually understood it once I worked through why a full
set of dummies makes the design matrix singular, and why regularisation
just masks that problem instead of fixing it. Before that I could
describe the symptom. I could not explain the mechanism. That gap,
between stating what something does and understanding why, is what I
would carry forward.

This showed up directly in Section 2. My first draft claimed no
predictor pair showed extreme correlation, while Figure 3, which I had
built myself, showed 0.91 and 0.86 sitting right there. Catching it
meant checking my own interpretation against my own output, the same
discipline behind the encoding and leakage-prevention choices elsewhere
in the pipeline.

The 5-fold cross-validation step mattered for the same reason. A single
split had already made the Decision Tree look strong, but the
cross-validation results, a higher mean F1 and lower standard deviation,
confirmed that was not a lucky split. It changed how I judge a good
model: accuracy alone would never have caught that two linear models
were missing nearly two-thirds of actual purchasers.

4.  **Future Improvements**

The Decision Tree was left at its default settings for this analysis.
Limiting how deep it is allowed to grow, or how few sessions can sit in
one leaf, is a standard way to guard against overfitting beyond what
cross-validation already checked (James et al., 2021).

A Random Forest, an ensemble of many trees rather than one, is a natural
next step (Breiman, 2001). It could capture the same behavioural
interactions the single tree relies on, with more stability and less
sensitivity to any one training split.

The current features describe how much a visitor did on the site, not in
what order. Capturing the actual sequence of pages visited, for instance
whether a product page came before or after browsing informational
content, could reveal intent the aggregated counts miss.

Once deployed, the model needs ongoing monitoring: for the usual drift
as browsing habits or site design change over time, and specifically for
whether PageValues stays available early enough in a live session to
support real-time intervention, not only describe a session after the
fact (Sakar et al., 2019).

**References**

Breiman, L. (2001) \'Random forests\', *Machine Learning*, 45(1), pp.
5--32.

James, G., Witten, D., Hastie, T. and Tibshirani, R. (2021) *An
Introduction to Statistical Learning with Applications in R*. 2nd edn.
New York: Springer.

Provost, F. and Fawcett, T. (2013) *Data Science for Business: What You
Need to Know about Data Mining and Data-Analytic Thinking*. Sebastopol,
CA: O\'Reilly Media.

Sakar, C.O., Polat, S.O., Katircioglu, M. and Kastro, Y. (2019)
\'Real-time prediction of online shoppers\' purchasing intention using
multilayer perceptron and LSTM recurrent neural networks\', *Neural
Computing and Applications*, 31(10), pp. 6893--6908.

