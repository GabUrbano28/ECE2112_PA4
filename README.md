# ECE2112_PA4
### Made by Antonio Gabriel L. Urbano | 2ECE-A
### EXPERIMENT 4: DATA WRANGLING AND DATA VISUALIZATION
The content of this repository contains the Programming Assignment 4 for our course "Advance Computer Programming and Algorithms" this S.Y. 2026-2027. This project covers three problems pertaining to Module 4 -  DATA WRANGLING AND DATA VISUALIZATION. 

### Objectives
The objectives of this experiment are to filter tabular data from (`board2.xslx`) using several categorical and numerical operations, construct focused DataFrames by selecting relevant features, summarize the relationship between categorical features and a numerical variable, and communicate a data comparison using clear and correctly labeled plots.

### Dataset Format
This experiment uses the supplied `ECE Board Exam 2` dataset. 

| Column Name | Type | Description |
| :--- | :--- | :--- |
| **Name** | Categorical | Student Number |
| **Gender** | Categorical | Student gender (`Male`, `Female`) |
| **Track** | Categorical | ECE track specialization (`Communication`, `Microelectronics`, `Instrumentation`) |
| **Hometown** | Categorical |  (`Luzon`, `Visayas`, `Mindanao`) |
| **Math** | Numerical | Board exam score in Mathematics |
| **GEAS** | Numerical | Board exam score in GEAS |
| **Electronics** | Numerical | Board exam score in Electronics  |
| **Communication**| Numerical | Board exam score in Communication  |

### Analysis Method
#### Part A
Filter: Isolate records where `Hometown` is Visayas and `Track` is Communication.

Features: Keep `Name`, `Gender`, `Math`, `Electronics`, and `Average`.

Outputs: Generate the filtered DataFrame alongside the total matching row count.

#### Part B
Filter: Select entries where `Hometown` is Visayas and `Gender` is Female.

Features: Keep `Name`, `Track`, `GEAS`, `Electronics`, and `Average`.

Outputs: Output the complete `VisFemale` DataFrame plus a non-destructive subset where `Average >= 60`.

#### Part C

Filter: Perform grouped aggregations across `Track`, `Gender`, and `Hometown`.

Features: Include categorical groups paired with the `Average` score.

Outputs: Display 3 summary tables in a bar chart using a shared 0–100 scale.


### Setup Data
```pyhton
import pandas as pd

df = pd.read_excel("board2.xlsx")

df['Average'] = df[
    ['Math', 'Electronics', 'GEAS', 'Communication']
].mean(axis=1)

display(df.head())
```

**Output:**

| Index | Name | Gender | Track | Hometown | Math | Electronics | GEAS | Communication | Average |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **0** | S1 | Male | Instrumentation | Luzon | 58 | 89 | 75 | 78 | 75.00 |
| **1** | S2 | Female | Communication | Mindanao | 52 | 75 | 90 | 52 | 67.25 |
| **2** | S3 | Female | Instrumentation | Mindanao | 83 | 74 | 77 | 57 | 72.75 |
| **3** | S4 | Male | Instrumentation | Visayas | 65 | 58 | 91 | 68 | 70.50 |
| **4** | S5 | Male | Communication | Luzon | 59 | 86 | 43 | 88 | 69.00 |

---

### 1. Visayas Communication Dataframe

#### Requirement:
Filter Visayas students in the Communication track, calculate missing Average values, and keep the `(Name, Gender, Math, Electronics, Average)` columns.

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
**Output:**

| Index | Name | Gender | Math | Electronics | Average |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **10** | S11 | Female | 48 | 56 | 54.75 |
| **11** | S12 | Male | 89 | 67 | 76.00 |
| **17** | S18 | Male | 81 | 40 | 63.50 |
| **21** | S22 | Female | 64 | 39 | 62.50 |
| **27** | S28 | Male | 85 | 53 | 67.75 |

```text
Number of rows in VisComm: 5
```

### 2. Visayas Female Dataframe
### Requirement:
Extract female students from Visayas with columns `(Name, Track, GEAS, Electronics, Average)` into VisFemale, then output a temporary filtered view for Average >= 60 without altering `VisFemale`.

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
**Output:**

**VisFemale DataFrame:**
| Index | Name | Track | GEAS | Electronics | Average |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **5** | S6 | Microelectronics | 86 | 45 | 75.50 |
| **10** | S11 | Communication | 48 | 56 | 54.75 |
| **20** | S21 | Microelectronics | 68 | 51 | 68.50 |
| **21** | S22 | Communication | 89 | 39 | 62.50 |
| **23** | S24 | Microelectronics | 60 | 45 | 57.75 |
| **25** | S26 | Instrumentation | 83 | 47 | 65.75 |

**VisFemale Students with Average >= 60:**
| Index | Name | Track | GEAS | Electronics | Average |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **5** | S6 | Microelectronics | 86 | 45 | 75.50 |
| **20** | S21 | Microelectronics | 68 | 51 | 68.50 |
| **21** | S22 | Communication | 89 | 39 | 62.50 |
| **25** | S26 | Instrumentation | 83 | 47 | 65.75 |


### 3. Category-Average Visualization
### Requirement:
Compute categorical means across `(Track, Gender, Hometow)`, display them on side-by-side axes normalized from 0 to 100, and algorithmically identify the highest-performing groups.

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

**Summary Tables Output:**

**Mean Average by Track:**
| Track | Average |
| :--- | :--- |
| Communication | 67.975000 |
| Instrumentation | 65.225000 |
| Microelectronics | 67.500000 |

**Mean Average by Gender:**
| Gender | Average |
| :--- | :--- |
| Female | 66.616667 |
| Male | 67.183333 |

**Mean Average by Hometown:**
| Hometown | Average |
| :--- | :--- |
| Luzon | 68.083333 |
| Mindanao | 66.678571 |
| Visayas | 65.750000 |

**Printed Summary Output:**
```text
1. Track: The category with the highest sample mean average is 'Communication' with a mean score of 67.97.
2. Gender: The category with the highest sample mean average is 'Male' with a mean score of 67.18.
3. Hometown: The category with the highest sample mean average is 'Luzon' with a mean score of 68.08.
```
