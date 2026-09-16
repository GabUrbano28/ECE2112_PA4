# ECE2112_PA4
### Made by Antonio Gabriel L. Urbano | 2ECE-A
### EXPERIMENT 4: DATA WRANGLING AND DATA VISUALIZATION
The content of this repository contains the Programming Assignment 4 for our course "Advance Computer Programming and Algorithms" this S.Y. 2026-2027. This project covers three problems pertaining to Module 4 -  DATA WRANGLING AND DATA VISUALIZATION. 

### Objectives
The objectives of this experiment are to filter tabular data using several categorical and numerical operations, construct focused DataFrames by selecting relevant features, summarize the relationship between categorical features and a numerical variable, and communicate a data comparison using clear and correctly labeled plots.

### Methods Used

### Setup Data
```pyhton
import pandas as pd

df = pd.read_excel("board2.xlsx")

df['Average'] = df[
    ['Math', 'Electronics', 'GEAS', 'Communication']
].mean(axis=1)

display(df.head())
```



### 1. Visayas Communication Dataframe

#### Requirement:
Filter Visayas students in the Communication track, calculate missing Average values, and keep the Name, Gender, Math, Electronics, and Average columns.

```pyhton
vis_comm_filter = (df['Hometown'] == 'Visayas') & (
    df['Track'] == 'Communication'
)
VisComm = df[vis_comm_filter][
    ['Name', 'Gender', 'Math', 'Electronics', 'Average']
]

display(VisComm)
print(f"Number of rows in VisComm: {len(VisComm)}")
```

### 2. Visayas Female Dataframe
### Requirement:
Extract female students from Visayas with columns Name, Track, GEAS, Electronics, and Average into VisFemale, then output a temporary filtered view for Average >= 60 without altering VisFemale.

```pyhton
vis_female_filter = (df['Hometown'] == 'Visayas') & (df['Gender'] == 'Female')
VisFemale = df[vis_female_filter][
    ['Name', 'Track', 'GEAS', 'Electronics', 'Average']
]

print("VisFemale DataFrame:")
display(VisFemale)

print("\nVisFemale with Average >= 60:")
display(VisFemale[VisFemale['Average'] >= 60])
```

### 3. Category-Average Visualization
### Requirement:
Compute categorical means across Track, Gender, and Hometown, display them on side-by-side axes normalized from 0 to 100, and algorithmically identify the highest-performing groups.

```pyhton
#PART C. LETTER - A
mean_track = df.groupby('Track')['Average'].mean().reset_index()
mean_gender = df.groupby('Gender')['Average'].mean().reset_index()
mean_hometown = df.groupby('Hometown')['Average'].mean().reset_index()

#PART C. LETTER - B
print("Mean Average by Track:")
display(mean_track)

print("\nMean Average by Gender:")
display(mean_gender)

print("\nMean Average by Hometown:")
display(mean_hometown)

#PART C. LETTER - C

fig, axes = plt.subplots(1, 3, figsize=(16, 5), sharey=True)

axes[0].bar(
    mean_track['Track'],
    mean_track['Average'],
    color='steelblue',
    edgecolor='black',
)
axes[0].set_title('Mean Average by Track', fontsize=12, fontweight='bold')
axes[0].set_xlabel('Track', fontsize=10)
axes[0].set_ylabel('Mean Board Exam Score', fontsize=10)
axes[0].tick_params(axis='x', rotation=15)

axes[1].bar(
    mean_gender['Gender'],
    mean_gender['Average'],
    color='mediumseagreen',
    edgecolor='black',
)
axes[1].set_title('Mean Average by Gender', fontsize=12, fontweight='bold')
axes[1].set_xlabel('Gender', fontsize=10)


axes[2].bar(
    mean_hometown['Hometown'],
    mean_hometown['Average'],
    color='coral',
    edgecolor='black',
)
axes[2].set_title('Mean Average by Hometown', fontsize=12, fontweight='bold')
axes[2].set_xlabel('Hometown', fontsize=10)

axes[0].set_ylim(0, 100)

plt.suptitle('Average Board Exam Scores Across Student Categories', fontsize=16, y=1.02)

plt.tight_layout()
plt.show()

#PART C. LETTER - D

top_track = track_mean.loc[track_mean['Average'].idxmax()]
top_gender = gender_mean.loc[gender_mean['Average'].idxmax()]
top_hometown = hometown_mean.loc[hometown_mean['Average'].idxmax()]

print(
    f"1. Track: The category with the highest sample mean average is "
    f"'{top_track['Track']}' with a mean score of {top_track['Average']:.2f}."
)
print(
    f"2. Gender: The category with the highest sample mean average is "
    f"'{top_gender['Gender']}' with a mean score of {top_gender['Average']:.2f}."
)
print(
    f"3. Hometown: The category with the highest sample mean average is "
    f"'{top_hometown['Hometown']}' with a mean score of {top_hometown['Average']:.2f}."
```
