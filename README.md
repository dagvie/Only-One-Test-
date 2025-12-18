# Week 4 Required Assignment: Hypotheses from Raw Listings Data

This week you’ll begin working with **real-world car listings data** pulled through our self-serve system.

You can access the self-serve form [here](https://docs.google.com/forms/d/e/1FAIpQLSdQQMShcp2OaFkdOclqkJN5hmWjWsowEte62WrglmW1S61Vkw/viewform?usp=header).

Note, this assignment is required for everyone, regardless of grade. The first draft is due before next week's class.

---

## Instructions & Policies
- **AI use:** You may use AI to ask questions if you are stuck or need clarification. However, do not use AI code-completion or copy/paste code from AI. We're still working on gaining fluency in pandas.
- **Output:** Ensure your notebook runs top-to-bottom without errors. Remove stray debug code before submitting. When ready to submit, do a "restart and clear outputs," then run everything through sequentially.

---

## The Setup

- Data is available via a [**Google Form**](https://docs.google.com/forms/d/e/1FAIpQLSdQQMShcp2OaFkdOclqkJN5hmWjWsowEte62WrglmW1S61Vkw/viewform?usp=header).
- After submitting the form, you’ll receive your dataset by email. Check your spam folder if it doesn’t show up!
- Each pull is limited to **100,000 rows**. If you’re unsure your pull worked, do a quick test run first.

You can filter the raw listings data by:
- **Time**  
- **Make/Model**  
- **State**  

---

## Important Note on Hypotheses

We are **not yet in position** to directly compare *entire populations* (e.g., *“all F-150s in Wisconsin vs. all F-150s in Wyoming”*).  
Instead, you’ll frame hypotheses about **samples** from your data.

Example:  
*A sample of 30 F-150s from Wisconsin is cheaper than the distribution of samples of 30 from Wyoming.*

---

## Your Tasks

1. **Pull one or more datasets** using the Google Form.  
   - Verify you receive the file by email.  
   - Import it into your notebook.

2. **Explore the dataset**:  
   - Basic summaries (`.describe()`, histograms, averages, etc.).  
   - Identify any quirks in the data (missing values, odd entries, etc.).

3. **Clean the dataset**:  
   - Turn this data set into a clean data frame that supports the analysis you'd like to do. 
   - This may involve all the tricks we've done before:
      - Dropping certain observations
      - Doing missing value imputation
      - Truncating data to a particular range


3. **Formulate a hypothesis** related to **samples** from your group’s dataset.  
   - Phrase these clearly in writing.  
   - The hypothesis should state both a statistic and a data generating process that will give you a reference distribution. 
   - Examples:  
     - *A sample of 50 Camrys from 2015–2020 in California will have higher mileage than a sample of 50 Camrys from Texas.*  
     - *In samples of 20 trucks from the Midwest, the average asking price exceeds $20,000.*

4. **Test your hypotheses informally** using the tools you know so far (sampling, grouping, simple comparisons).  
   - Formal inference will come later.  
   - For now, focus on setting up the problem and seeing what the data suggests.

5. **Reflect**:  
   - What did you conclude?
   - Go back and run all of your code without any of the cleaning (or with minimal cleaning). Do the conclusions change at all?  
   - How did the sample framing change the way you thought about your comparisons?

---

## Deliverables

- A Jupyter notebook that:  
  - Runs top-to-bottom without errors.  
  - Contains your data pull and exploratory summaries.  
  - Clearly labels your hypotheses and results.  
  - Includes a short written reflection.

## Feedback 

Nice job on the exploration and cleaning. 

There are pieces of code that aren't relevant to testing your hypothesis. For the sake of clarity, I'd drop them. Here are some examples: 
```
def iqr(series):
    return series.quantile(0.75) - series.quantile(0.25)

# Apply your IQR function to prices across locations
price_iqr_location= (
    listings.groupby('location')['price']
    .apply(iqr)
    .reset_index(name='price_iqr_location')
    .sort_values('price_iqr_location', ascending=False)
)

print(price_iqr_location.head(10))

f150_avg_price = (
    F150_sample[(F150_sample["make"] == "ford") & (F150_sample["model"] == "f150")]
    .groupby("location")["price"]
    .mean()
    .reset_index()
    .sort_values("price", ascending=False)
)
print(F150_sample)

def avg_price_diff(series):
    # Calculate the average absolute difference from the mean price
    mean_price = series.mean()
    avg_diff = (series - mean_price).abs().mean()
    return avg_diff

price_diff_location = (
    listings.groupby('location')['price']
    .apply(avg_price_diff)
    .reset_index(name='avg_price_diff')
    .sort_values('avg_price_diff', ascending=False)
)

print(price_diff_location.head(30))

# Step 3: Plot chart for ford f150
plt.figure(figsize=(10,6))
plt.bar(f150_avg_price['location'], f150_avg_price['price'], color='blue')
plt.xticks(rotation=45, ha='right')
plt.title("Average ford f150 Price by Location")
plt.ylabel("Average Price ($)")
plt.xlabel("Location")
plt.tight_layout()
plt.show()

listings.describe()

f150_avg_price = (
    listings[(listings["make"] == "ford") & (listings["model"] == "f150")]
    .groupby("location")["price"]
    .mean()
    .reset_index()
    .sort_values("price", ascending=False)
)
print(listings)

plt.figure(figsize=(10,6))
plt.bar(f150_avg_price['location'], f150_avg_price['price'], color='blue')
plt.xticks(rotation=45, ha='right')
plt.title("Average ford f150 Price by Location")
plt.ylabel("Average Price ($)")
plt.xlabel("Location")
plt.tight_layout()
plt.show()
```

This all feels like you're struggling to understand what to do.  You can follow the structure we've done in class. 

1. Create an empty list to hold the differences in means between Mpls and Dallas.
2. Take samples from each of your groups.
3. Calculate the average price per group.
4. Subtract those and store the differences.
5. Then plot the values, paying attention to the zero value, since that's the value with no difference in the means.
6. Calculate the fraction of replicates that are more extreme than the value you found. 
