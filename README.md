# SemProject_MLEngine

Group Members  
189813 Tumaini Wekesa  
190013 Kasongo Wasingya Mediatrice  
179581 Mutemi Hellen Nzisa  

## TechPath ML Engine

TechPath is a mobile app that builds a career-skills roadmap for technology students. A student types the career they want, and the app shows the skills to learn, an explanation of each, and learning resources. This repository holds the ML engine behind it.

The engine cleans the student's text, converts it to TF-IDF features, and uses Logistic Regression to predict the most likely career paths. It then ranks the skills on the best path by cosine similarity and returns the top 5.

## Dataset and cleaning

Roadmap data from roadmap.sh (`nilbuild/developer-roadmap` on GitHub), converted from JSON to CSV. Cleaning took 10,634 raw rows down to 10,204 rows across 86 career paths:

- Dropped 30 rows with no title or description
- Dropped 308 duplicates (same career path and title)
- Removed links, URLs and tags from 95 descriptions
- Dropped 3 descriptions under 10 words
- Dropped 89 rows from 7 career paths with fewer than 30 skills

## EDA observations

1. Classes are imbalanced: the largest path has almost 10 times more skills than the smallest.
2. About 1,000 skill titles (like "Git") appear on more than one roadmap.
3. Most descriptions are 50 to 80 words.
4. Each career has its own vocabulary (for example android: kotlin, jetpack, app).

## Modelling and validation

- Base model: TF-IDF + Logistic Regression
- 80/20 stratified train/test split
- 5-fold stratified cross-validation, with TF-IDF refitted inside each fold
- Regularisation: grid search over C and n-gram range. Best was C = 30 with unigrams and bigrams (CV macro-F1 0.696)

## Results (test set)

| Metric | Score |
|---|---|
| Accuracy | 0.726 |
| Macro-F1 | 0.725 |
| Top-3 accuracy | 0.882 |
