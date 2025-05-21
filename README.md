# Covid--19-Jupyter-notebook
# COVID-19 Data Analysis

# Import Libraries
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# Setup style
sns.set(style="whitegrid")

#  Load the Data
df = pd.read_csv("owid-covid-data.csv", parse_dates=["date"])
df.head

#  Clean the Data
df = df[["location", "date", "total_cases", "new_cases", "total_deaths", "new_deaths", "people_vaccinated", "continent"]]
df = df.dropna(subset=["continent"])  # remove aggregate rows like "World"
df = df.fillna(0)  # fill missing values with 0
df.head()
#  Global Trends Over Time
global_trends = df.groupby("date")[["new_cases", "new_deaths", "people_vaccinated"]].sum()

plt.figure(figsize=(14, 6))
sns.lineplot(data=global_trends)
plt.title("Global Daily COVID-19 Metrics")
plt.ylabel("Count")
plt.xlabel("Date")
plt.show()
#  Top Countries by Total Cases
latest = df[df["date"] == df["date"].max()]
top_countries = latest.groupby("location")[["total_cases", "total_deaths", "people_vaccinated"]].sum()
top_countries = top_countries.sort_values("total_cases", ascending=False).head(10)

top_countries.plot(kind="bar", figsize=(12, 6))
plt.title("Top 10 Countries by Total Cases")
plt.ylabel("Count")
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()
