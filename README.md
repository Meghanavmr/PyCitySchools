
# PyCity Schools Analysis

* As a whole, schools with higher budgets, did not yield better test results. By contrast, schools with higher spending per student actually (\$645-675) underperformed compared to schools with smaller budgets (<\$585 per student).

* As a whole, smaller and medium sized schools dramatically out-performed large sized schools on passing math performances (89-91% passing vs 67%).

* As a whole, charter schools out-performed the public district schools across all metrics. However, more analysis will be required to glean if the effect is due to school practices or the fact that charter schools tend to serve smaller student populations per school. 
---

### Note
* Instructions have been included for each segment. You do not have to follow them exactly, but they are included to help you think through the steps.


```python
# Dependencies and Setup
import pandas as pd
import numpy as np

# File to Load (Remember to Change These)
school_data_to_load = "Resources/schools_complete.csv"
student_data_to_load = "Resources/students_complete.csv"

# Read School and Student Data File and store into Pandas Data Frames
school_data = pd.read_csv(school_data_to_load)
student_data = pd.read_csv(student_data_to_load)

school_df = pd.DataFrame(school_data)

total_schools = school_df["School ID"].count()
total_students = school_df["size"].sum()
total_budget= school_df["budget"].sum()

school_df.head()
print(total_schools, total_budget, total_students)
```

    15 24649428 39170
    


```python
school_data = school_data.rename(columns={"name":"school name"})
student_data = student_data.rename(columns={"school":"school name"})
# Combine the data into a single dataset
school_data_complete = pd.merge(school_data, student_data, how="left", on=["school name"])

school_data_complete.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>School ID</th>
      <th>school name</th>
      <th>type</th>
      <th>size</th>
      <th>budget</th>
      <th>Student ID</th>
      <th>name</th>
      <th>gender</th>
      <th>grade</th>
      <th>reading_score</th>
      <th>math_score</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>Huang High School</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
      <td>0</td>
      <td>Paul Bradley</td>
      <td>M</td>
      <td>9th</td>
      <td>66</td>
      <td>79</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0</td>
      <td>Huang High School</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
      <td>1</td>
      <td>Victor Smith</td>
      <td>M</td>
      <td>12th</td>
      <td>94</td>
      <td>61</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0</td>
      <td>Huang High School</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
      <td>2</td>
      <td>Kevin Rodriguez</td>
      <td>M</td>
      <td>12th</td>
      <td>90</td>
      <td>60</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0</td>
      <td>Huang High School</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
      <td>3</td>
      <td>Dr. Richard Scott</td>
      <td>M</td>
      <td>12th</td>
      <td>67</td>
      <td>58</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0</td>
      <td>Huang High School</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
      <td>4</td>
      <td>Bonnie Ray</td>
      <td>F</td>
      <td>9th</td>
      <td>97</td>
      <td>84</td>
    </tr>
  </tbody>
</table>
</div>



## District Summary

* Calculate the total number of schools

* Calculate the total number of students

* Calculate the total budget

* Calculate the average math score 

* Calculate the average reading score

* Calculate the overall passing rate (overall average score), i.e. (avg. math score + avg. reading score)/2

* Calculate the percentage of students with a passing math score (70 or greater)

* Calculate the percentage of students with a passing reading score (70 or greater)

* Create a dataframe to hold the above results

* Optional: give the displayed data cleaner formatting


```python
# total_schools = school_data_complete["School ID"].value_counts()
# total_students = school_data_complete["Student ID"].sum()
# total_budget= school_data_complete["budget"].sum()
avgmathscore = school_data_complete["math_score"].mean()
avgreadingscore = school_data_complete["reading_score"].mean()
passingmath = school_data_complete.query('math_score >70')["School ID"].count()/ total_students*100
passingreading = school_data_complete.query('reading_score >70')["School ID"].count()/ total_students*100
overallpassrate = (avgmathscore + avgreadingscore)/2
District_summary_df = pd.DataFrame({"Total Schools":[total_schools],
                                   "Total Students":[total_students],
                                   "Total Budget":[total_budget],
                                   "Average Math Score":[avgmathscore],
                                   "Average Reading Score":[avgreadingscore],
                                   "% Passing Math":[passingmath],
                                   "% Passing Reading":[passingreading],
                                   "% Overall Passing Rate":[overallpassrate]
})



District_summary_df = District_summary_df[["Total Schools",
"Total Students",
"Total Budget",
"Average Math Score",
"Average Reading Score",
"% Passing Math",
"% Passing Reading",
"% Overall Passing Rate"]]


District_summary_df

District_summary_df["Average Math Score"] = District_summary_df["Average Math Score"].map("{:.2f}".format)
District_summary_df["Average Reading Score"] = District_summary_df["Average Reading Score"].map("{:.2f}".format)
District_summary_df["Total Budget"] = District_summary_df["Total Budget"].map("{:,}".format)
District_summary_df["% Passing Math"] = District_summary_df["% Passing Math"].map("{:.2f}".format)
District_summary_df["% Passing Reading"] = District_summary_df["% Passing Reading"].map("{:.2f}".format)
District_summary_df["% Overall Passing Rate"] = District_summary_df["% Overall Passing Rate"].map("{:.2f}".format)


District_summary_df

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Total Schools</th>
      <th>Total Students</th>
      <th>Total Budget</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing Rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>15</td>
      <td>39170</td>
      <td>24,649,428</td>
      <td>78.99</td>
      <td>81.88</td>
      <td>72.39</td>
      <td>82.97</td>
      <td>80.43</td>
    </tr>
  </tbody>
</table>
</div>



## School Summary

* Create an overview table that summarizes key metrics about each school, including:
  * School Name
  * School Type
  * Total Students
  * Total School Budget
  * Per Student Budget
  * Average Math Score
  * Average Reading Score
  * % Passing Math
  * % Passing Reading
  * Overall Passing Rate (Average of the above two)
  
* Create a dataframe to hold the above results

## Top Performing Schools (By Passing Rate)

* Sort and display the top five schools in overall passing rate

## Bottom Performing Schools (By Passing Rate)

* Sort and display the five worst-performing schools

## Math Scores by Grade

* Create a table that lists the average Reading Score for students of each grade level (9th, 10th, 11th, 12th) at each school.

  * Create a pandas series for each grade. Hint: use a conditional statement.
  
  * Group each series by school
  
  * Combine the series into a dataframe
  
  * Optional: give the displayed data cleaner formatting

## Reading Score by Grade 

* Perform the same operations as above for reading scores

## Scores by School Spending

* Create a table that breaks down school performances based on average Spending Ranges (Per Student). Use 4 reasonable bins to group school spending. Include in the table each of the following:
  * Average Math Score
  * Average Reading Score
  * % Passing Math
  * % Passing Reading
  * Overall Passing Rate (Average of the above two)


```python
# Sample bins. Feel free to create your own bins.
spending_bins = [0, 585, 615, 645, 675]
group_names = ["<$585", "$585-615", "$615-645", "$645-675"]
```

## Scores by School Size

* Perform the same operations as above, based on school size.


```python
# Sample bins. Feel free to create your own bins.
size_bins = [0, 1000, 2000, 5000]
group_names = ["Small (<1000)", "Medium (1000-2000)", "Large (2000-5000)"]
```

## Scores by School Type

* Perform the same operations as above, based on school type.
