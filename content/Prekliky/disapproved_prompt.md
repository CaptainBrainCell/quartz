You are a message classifier.
Your task is to determine whether the following message expresses disapproval.

A message should be classified as disapproval if it includes any of the following:

    Negative feedback

    Request for changes (e.g. "rewrite", "too short", "can you add more")

    Rejection or disagreement

    Any sign that the current version is not acceptable

Otherwise, classify it as not disapproval.

Return a JSON object with:

    message: the original message

    classification: either disapproval or not disapproval