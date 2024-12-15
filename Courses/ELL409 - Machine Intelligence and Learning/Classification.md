
Classification is all about predicting a discrete label.

Predicting a categorical output.
• Binary classification predicts one of two possible outcomes (e.g., is
the email spam or not spam?)
• Multi-class classification predicts one of multiple possible outcomes
(e.g. is this a photo of a cat, dog, horse or human?)

## Classification Threshold

The lowest probability value at which
we’re comfortable asserting a positive classification. For example, if
the predicted probability of being diabetic is > 50%, return True,
otherwise return False.