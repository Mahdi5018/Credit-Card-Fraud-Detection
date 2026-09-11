Credit Card Fraud Detection

An academic machine-learning project using a Decision Tree classifier in Python to identify fraudulent credit card transactions in a highly imbalanced dataset.

Main finding: the adjusted model detected 86 of 98 fraud cases, compared with 77 of 98 initially. Fraud recall increased from 78.6% to 87.8%, but false positives rose from 14 to 2,738. The project illustrates why fraud detection must be evaluated using both missed fraud and false alarms, rather than accuracy alone.

Research questions

Can a Decision Tree identify fraudulent transactions when fraud represents only a small fraction of the data?

How do class weighting and tree constraints affect fraud detection?

What trade-off emerges between catching more fraud and incorrectly flagging legitimate transactions?

Dataset

The presentation cites the Credit Card Fraud Detection dataset on Kaggle.

Item

Value

Total transactions

284,807

Fraudulent transactions

492

Fraud share, calculated from the reported counts

Approximately 0.173%

Test-set transactions

56,962

Fraudulent transactions in the test set

98

The target is Class, and the input matrix contains the remaining columns. The presentation discusses numbered features such as V17, V12 and V14 when interpreting the tree.

Methods

1. Explore the class distribution

Inspect and visualize the distribution of fraudulent and non-fraudulent transactions. The extreme imbalance motivates examining the confusion matrix and fraud recall alongside accuracy.

2. Split the data

The code shown in the presentation uses an 80% training / 20% test split, with random_state=42 and stratify=y to preserve class proportions.

3. Train a Decision Tree

Use scikit-learn's DecisionTreeClassifier, generate predictions and visualize the tree with plot_tree. A Decision Tree makes the sequence of feature-based splits visible, although numbered features limit interpretation in everyday business terms.

4. Adjust the model

The adjusted configuration shown in the presentation is:

DecisionTreeClassifier(
    max_depth=4,
    min_samples_split=10,
    class_weight="balanced",
    random_state=42
)

Depth and minimum-split constraints control tree complexity, while balanced class weights give greater weight to the minority class during training. The presentation compares the resulting predictions with the initial reported results.

5. Evaluate the trade-off

Fraud recall: the share of actual fraud cases detected, TP / (TP + FN).

Fraud precision: the share of fraud alerts that are correct, TP / (TP + FP).

Accuracy: the share of all transactions classified correctly.

Confusion matrix: the counts of correct predictions, missed fraud and false alarms.

The presentation reports accuracy and recall. Precision below is calculated from its confusion matrices.

Findings

Model comparison

Both reported confusion matrices contain 56,962 transactions, including 98 fraud cases.

Metric

Initial model

Adjusted model

Correctly cleared legitimate transactions (TN)

56,850

54,126

Legitimate transactions flagged as fraud (FP)

14

2,738

Missed fraud cases (FN)

21

12

Detected fraud cases (TP)

77

86

Fraud recall

78.6%

87.8%

Accuracy

99.94%

95.17%

Fraud precision, calculated

84.6%

3.0%

Counts are transcribed from the presentation's confusion matrices. Metrics are rounded; calculated precision is additional analysis of those counts, not a separately reported experiment.

Why accuracy is insufficient

The initial model's 99.94% accuracy still leaves 21 of 98 fraud cases undetected. For context, predicting every test transaction as non-fraud would achieve approximately 99.83% accuracy while detecting no fraud at all. This baseline is calculated from the reported test-set counts.

What changed after adjustment

The adjusted model caught 9 additional fraud cases, but generated 2,724 additional false alarms. Recall improved by approximately 9.2 percentage points, while fraud precision fell sharply.

This is an improvement in recall, not evidence that the adjusted model is better overall. Its alerts would require substantially more review, and most would concern legitimate transactions.

Tree interpretation

The presentation highlights V17 as influential, alongside V12, V26, V27, V14 and V10. This is an interpretation of the displayed tree, rather than evidence that those features cause fraud or remain equally important across different samples.

Business interpretation

Choosing between these configurations would require estimates of fraud losses, the cost of reviewing alerts, customer friction from false alarms and the team's review capacity.

The adjusted model could only be justified if the value of the additional detected fraud outweighed the costs of the extra alerts. The project does not estimate those costs or demonstrate deployment, prevented losses or operational impact.

Limitations

Extreme class imbalance: high accuracy can conceal missed fraud, so precision and recall must be assessed together.

Small fraud test sample: results depend on just 98 fraudulent transactions. No uncertainty intervals or repeated-split results are reported.

No documented independent tuning evaluation: the presentation shows a train/test split but no separate validation set or cross-validation procedure. If adjustments were guided by test results, another untouched holdout would be needed for a final assessment.

Initial configuration ambiguity: the initial training slide shows both a default Decision Tree assignment and an assignment with max_depth=4. The exact configuration responsible for the initial confusion matrix should be confirmed in the executable code.

Several settings change together: the comparison does not isolate the separate effects of depth, minimum split size and class weighting.

False-positive burden: the adjusted model's approximately 3.0% fraud precision makes recall alone an inadequate basis for selecting it.

Limited generalization evidence: the random split does not establish performance on future transactions or another payment environment.

Reproducibility: the presentation contains code screenshots, but this README does not establish a runnable environment or independently reproduce the results.

Possible next steps

Publish the complete notebook or scripts and dependency versions, and clarify the initial model configuration.

Reserve an untouched test set and use stratified cross-validation on training data for model selection.

Compare configurations using precision-recall curves, average precision and alert volumes, alongside recall.

Evaluate expected costs using explicit assumptions about missed fraud and false alarms.

Compare the Decision Tree with other classifiers and test performance on a time-based holdout where appropriate.

Source

This README summarizes the submitted Credit Card Fraud Detection project presentation. Additional calculations are identified above and use only the confusion-matrix counts reported in tha
