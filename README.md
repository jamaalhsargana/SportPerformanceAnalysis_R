# Does Home Advantage Help Arsenal? A Small Statistics Study in R

I've supported Arsenal since I was a kid, and there's a question every fan argues about: do they actually play better at the Emirates? For this project I collected Arsenal's results from part of the 2021 campaign and tested that question with proper statistics instead of gut feeling.

This was my individual portfolio assignment for the *Statistics and Networks Analysis in Data Science* module (MATPMDB) in my MSc AI at the University of Stirling.

## Results at a glance

| Question | Test | Result | p-value | Answer |
|---|---|---|---|---|
| Does Arsenal win more at home than away? | Welch two-sample t-test (win = 1, no win = 0) | t = −0.83 | 0.416 | No significant difference |
| Same question, on the win/no-win table | Fisher's exact test | – | 0.670 | No significant association |
| Do they get more points earlier in the season than later? | Welch t-test on points before vs after 6 March 2021 | – | 0.612 | No significant difference |
| How likely is a win? | Binomial likelihood (maximum likelihood estimate) | Overall ≈ 0.55, home ≈ 0.45, away ≈ 0.64 | – | Better away than at home in this sample |

So in this data, playing at home didn't help. If anything, Arsenal won a bigger share of their away games. But with only about 11 games of each kind, that difference is well within what you'd expect by chance (see [Things to keep in mind](#things-to-keep-in-mind)).

---

## The data

I put together a small dataset of **23 Arsenal matches**, one row per match:

| Column | Meaning |
|---|---|
| `Date` | Match date (dd/mm/yyyy) |
| `Opponent` | Team Arsenal played |
| `Score` | Final score |
| `Points` | Points Arsenal earned: 3 win, 1 draw, 0 loss |
| `Home_adv` | 1 = home game, 0 = away game |
| `Total_Points` | Arsenal's running points total |

In the analysis I added a `Result` column: **1 if Arsenal won, 0 otherwise**. So draws and losses both count as "no win".

## Walking through the analysis

**1. Setting up the hypotheses**

- H₀: P(win | home) = P(win | away). Home advantage makes no difference.
- H₁: P(win | home) ≠ P(win | away)

**2. t-test: home vs away.** I split the win/no-win results into home and away games and used a two-sample t-test, since the two groups are independent. With p = 0.416, I couldn't reject H₀.

**3. t-test: early vs late season.** People often say Arsenal start the season strongly and then fade. I split the matches at 6 March 2021 and compared points before and after. With p = 0.612, there's no evidence for it in this data.

**4. Visualising.** I used ggplot2 for a stacked bar chart of wins and no-wins at home and away, and a scatter plot of points over time with a LOESS trend line for each.

**5. Fisher's exact test.** Because the groups are so small, I also built a 2×2 table (home/away × win/no-win) and used Fisher's exact test, which doesn't rely on large-sample assumptions. p = 0.670, the same conclusion.

**6. Likelihood.** I treated the number of wins as binomial and plotted the likelihood over every possible win probability *p*:
- overall: 12 wins out of 22 → maximum at p ≈ 0.545
- home: 5 out of 11 → p ≈ 0.455
- away: 7 out of 11 → p ≈ 0.636

I also plotted the log-likelihood (support function) for home games, with a line at −2 to show the range of plausible values.

**7. Linear regression.** I fitted `Total_Points ~ Date` (p < 2.2e-16) and `Points ~ Total_Points` (p = 0.215).

## Running it

Everything is in one R Markdown file, `Performance _Analysis_R.Rmd`, which knits to PDF.

1. Install R (and RStudio if you like) and the `ggplot2` and `rmarkdown` packages:
   ```r
   install.packages(c("ggplot2", "rmarkdown"))
   ```
2. Put the data file `dataset.txt` (tab-separated, with a header row) in the same folder as the `.Rmd`.
3. The report also includes a photo, `arsenal_stadium.jpg`. Add it to the same folder, or delete that line, otherwise knitting will fail.
4. Knit to PDF in RStudio, or run:
   ```r
   rmarkdown::render("Performance _Analysis_R.Rmd")
   ```
   Knitting to PDF also needs a LaTeX installation (for example, `install.packages("tinytex"); tinytex::install_tinytex()`).

**Files in this repo:** `Performance _Analysis_R.Rmd` and this README. `dataset.txt` and the stadium image aren't uploaded yet.

## Things to keep in mind

Rereading this now, a few things stand out that I'd do differently:

- **The sample is tiny.** About 11 home and 11 away games isn't enough to detect anything except a huge effect. Not finding a significant difference is *not* evidence that there's no home advantage. The tests just didn't have enough data to tell.
- **The game counts don't add up.** The dataset has 23 matches, but the likelihood section uses 22 games (11 home + 11 away). I need to go back and check which match was left out and why.
- **The `Total_Points ~ Date` regression doesn't really tell us anything.** Total points is a running total, so it can only go up as the season goes on. A straight line will always fit it very well, which is where the tiny p-value comes from. It doesn't mean Arsenal were improving. Modelling points per game over time would answer that question properly.
- **`Points ~ Total_Points` mixes things up too**, because each match's points are already part of the running total.
- **Draws count as "no win".** This treats a draw the same as a loss. Using all three outcomes (win/draw/loss) or points per game would keep more information.
- **A t-test on 0/1 data** works roughly, but a test made for proportions (like `prop.test`) or a logistic regression would be the more natural choice. Fisher's test was the better fit here, which is why I included it.
- **One season, one team.** The results can't be generalised to Arsenal in other seasons, or to football as a whole.

## If I came back to this

- Collect several full seasons (and maybe several teams) so the tests actually have enough data
- Use logistic regression for win/no-win with home/away and opponent strength as predictors
- Model points per game over time instead of the running total
- Add confidence intervals for the home and away win rates, not just the point estimates
- Upload the dataset and image so the report can be knitted straight from the repo
