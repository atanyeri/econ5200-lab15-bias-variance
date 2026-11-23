# Bias, Variance and an Unbiased Cross-Validation Score

## Objective

I measured how optimistic a cross-validation score becomes when the same score is used to choose the model, and I tested whether nested cross-validation gives an honest estimate instead.

## Methodology

- I drew 80 points from a simulated curve with known noise, so I could compare every estimate with the truth.
- I cross-validated polynomials of degree 1 to 12 with 5-fold CV and kept the degree with the smallest CV error, as an analyst would.
- I scored that chosen fit on 20,000 new points from the same simulation to get its true error.
- I ran nested cross-validation: an outer loop of folds, with the whole degree search repeated inside each one, so each outer fold scored a choice it had no part in making.
- I split the expected error into bias squared, variance and noise by refitting each degree on many fresh simulated samples.
- I wrote my own k-fold cross-validation function that works with any scikit-learn model on a table with named columns, and checked that it reproduced the lab's numbers exactly.

## Key Findings

- The search chose degree 5, with a CV error of 0.1853. On 20,000 new points its true error was 0.2330, so the reported score made the model look better than it is. Picking the smallest of many noisy scores favours the one that got lucky, the same effect as the winner's curse.
- Nested cross-validation estimated 0.1910. That is closer to the truth because the score was never used to make the choice. It still falls short, because no resampling of the same 80 points can correct for that sample's own luck.
- The noise alone contributed 0.2025, a floor that no model can get below. A reported error of 0.1853, below that floor, can only be an overly optimistic estimate.
- My k-fold function matched the lab: with 10 folds, degree 5 gave 0.1921.
