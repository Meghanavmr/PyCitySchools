
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

District_summary_df["Average Math Score"] = District_summary_df["Average Math Score"]
District_summary_df["Average Reading Score"] = District_summary_df["Average Reading Score"]
District_summary_df["Total Budget"] = District_summary_df["Total Budget"].map("${:,}".format)
District_summary_df["% Passing Math"] = District_summary_df["% Passing Math"]
District_summary_df["% Passing Reading"] = District_summary_df["% Passing Reading"]
District_summary_df["% Overall Passing Rate"] = District_summary_df["% Overall Passing Rate"]


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
      <td>$24,649,428</td>
      <td>78.985371</td>
      <td>81.87784</td>
      <td>72.392137</td>
      <td>82.971662</td>
      <td>80.431606</td>
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


```python
school_data_complete_new = school_data_complete[["School ID", "school name", "type", "size", "budget", "Student ID", "name",
                                                 "gender", "grade", "reading_score","math_score"]].copy()
school_data_complete_new.head()
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




```python
# school_data_complete_new.head()
school_data_complete_new["School ID"].count()
```




    39170




```python
for_sch_budget = school_df.groupby("name")
total_budget_sch_wise = for_sch_budget["budget"].sum()


total_budget_sch_wise.head()
```




    name
    Bailey High School      3124928
    Cabrera High School     1081356
    Figueroa High School    1884411
    Ford High School        1763916
    Griffin High School      917500
    Name: budget, dtype: int64




```python

grouped_school_data = school_data_complete_new.groupby(['school name', "type"])
# grouped_school_data.drop_duplicates(subset=[budget], keep=False)


total_students_grp = grouped_school_data["Student ID"].count()
total_budget_grp = school_df["budget"].sum()
# total_budget_grp = grouped_school_data["budget"].sum()
per_stu_budget_grp =  (total_budget_grp/ total_students_grp)/100
avgmathscore_grp = grouped_school_data["math_score"].mean()
avgreadingscore_grp = grouped_school_data["reading_score"].mean()
passingmath_grp = school_data_complete_new.query('math_score >70')["School ID"].count()/ total_students_grp * 100
passingreading_grp = school_data_complete_new.query('reading_score >70')["School ID"].count()/ total_students_grp * 100
overallpassrate_grp = (avgmathscore_grp + avgreadingscore_grp)/2
  
# Converting a GroupBy object into a DataFrame
grouped_school_data_df = pd.DataFrame({"Total Students":total_students_grp,
                                       "Total School Budget": total_budget_grp, 
                                       "Per Student Budget":per_stu_budget_grp,
                                       "Average Math Score": avgmathscore_grp,
                                       "Average Reading Score": avgreadingscore_grp,   
                                       "% Passing Math":passingmath_grp,
                                       "% Passing Reading":passingmath_grp,
                                        "Overall Passing Rate":overallpassrate_grp          
                                    
                                      
})

grouped_school_data_df = grouped_school_data_df[[  "Total Students",
    "Total School Budget",
    "Per Student Budget",
    "Average Math Score",
    "Average Reading Score",
     "% Passing Math",   
     "% Passing Math",                                             
    "Overall Passing Rate"]]


grouped_school_data_df



grouped_school_data_df.head(10)
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
      <th></th>
      <th>Total Students</th>
      <th>Total School Budget</th>
      <th>Per Student Budget</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Math</th>
      <th>Overall Passing Rate</th>
    </tr>
    <tr>
      <th>school name</th>
      <th>type</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Bailey High School</th>
      <th>District</th>
      <td>4976</td>
      <td>24649428</td>
      <td>49.536632</td>
      <td>77.048432</td>
      <td>81.033963</td>
      <td>569.855305</td>
      <td>569.855305</td>
      <td>79.041198</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <th>Charter</th>
      <td>1858</td>
      <td>24649428</td>
      <td>132.666459</td>
      <td>83.061895</td>
      <td>83.975780</td>
      <td>1526.157158</td>
      <td>1526.157158</td>
      <td>83.518837</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <th>District</th>
      <td>2949</td>
      <td>24649428</td>
      <td>83.585717</td>
      <td>76.711767</td>
      <td>81.158020</td>
      <td>961.546287</td>
      <td>961.546287</td>
      <td>78.934893</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <th>District</th>
      <td>2739</td>
      <td>24649428</td>
      <td>89.994261</td>
      <td>77.102592</td>
      <td>80.746258</td>
      <td>1035.268346</td>
      <td>1035.268346</td>
      <td>78.924425</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <th>Charter</th>
      <td>1468</td>
      <td>24649428</td>
      <td>167.911635</td>
      <td>83.351499</td>
      <td>83.816757</td>
      <td>1931.607629</td>
      <td>1931.607629</td>
      <td>83.584128</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <th>District</th>
      <td>4635</td>
      <td>24649428</td>
      <td>53.181074</td>
      <td>77.289752</td>
      <td>80.934412</td>
      <td>611.779935</td>
      <td>611.779935</td>
      <td>79.112082</td>
    </tr>
    <tr>
      <th>Holden High School</th>
      <th>Charter</th>
      <td>427</td>
      <td>24649428</td>
      <td>577.269977</td>
      <td>83.803279</td>
      <td>83.814988</td>
      <td>6640.749415</td>
      <td>6640.749415</td>
      <td>83.809133</td>
    </tr>
    <tr>
      <th>Huang High School</th>
      <th>District</th>
      <td>2917</td>
      <td>24649428</td>
      <td>84.502667</td>
      <td>76.629414</td>
      <td>81.182722</td>
      <td>972.094618</td>
      <td>972.094618</td>
      <td>78.906068</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <th>District</th>
      <td>4761</td>
      <td>24649428</td>
      <td>51.773636</td>
      <td>77.072464</td>
      <td>80.966394</td>
      <td>595.589162</td>
      <td>595.589162</td>
      <td>79.019429</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <th>Charter</th>
      <td>962</td>
      <td>24649428</td>
      <td>256.231060</td>
      <td>83.839917</td>
      <td>84.044699</td>
      <td>2947.609148</td>
      <td>2947.609148</td>
      <td>83.942308</td>
    </tr>
  </tbody>
</table>
</div>



## Top Performing Schools (By Passing Rate)

* Sort and display the top five schools in overall passing rate


```python
Top_schools = grouped_school_data_df.sort_values(
    ["Overall Passing Rate"], ascending=False)
Top_schools.head()
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
      <th></th>
      <th>Total Students</th>
      <th>Total School Budget</th>
      <th>Per Student Budget</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Math</th>
      <th>Overall Passing Rate</th>
    </tr>
    <tr>
      <th>school name</th>
      <th>type</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Pena High School</th>
      <th>Charter</th>
      <td>962</td>
      <td>24649428</td>
      <td>256.231060</td>
      <td>83.839917</td>
      <td>84.044699</td>
      <td>2947.609148</td>
      <td>2947.609148</td>
      <td>83.942308</td>
    </tr>
    <tr>
      <th>Wright High School</th>
      <th>Charter</th>
      <td>1800</td>
      <td>24649428</td>
      <td>136.941267</td>
      <td>83.682222</td>
      <td>83.955000</td>
      <td>1575.333333</td>
      <td>1575.333333</td>
      <td>83.818611</td>
    </tr>
    <tr>
      <th>Holden High School</th>
      <th>Charter</th>
      <td>427</td>
      <td>24649428</td>
      <td>577.269977</td>
      <td>83.803279</td>
      <td>83.814988</td>
      <td>6640.749415</td>
      <td>6640.749415</td>
      <td>83.809133</td>
    </tr>
    <tr>
      <th>Thomas High School</th>
      <th>Charter</th>
      <td>1635</td>
      <td>24649428</td>
      <td>150.761028</td>
      <td>83.418349</td>
      <td>83.848930</td>
      <td>1734.311927</td>
      <td>1734.311927</td>
      <td>83.633639</td>
    </tr>
    <tr>
      <th>Wilson High School</th>
      <th>Charter</th>
      <td>2283</td>
      <td>24649428</td>
      <td>107.969461</td>
      <td>83.274201</td>
      <td>83.989488</td>
      <td>1242.049934</td>
      <td>1242.049934</td>
      <td>83.631844</td>
    </tr>
  </tbody>
</table>
</div>



## Bottom Performing Schools (By Passing Rate)

* Sort and display the five worst-performing schools


```python
Bottom_schools = grouped_school_data_df.sort_values(
    ["Overall Passing Rate"], ascending=True)
Bottom_schools.head()
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
      <th></th>
      <th>Total Students</th>
      <th>Total School Budget</th>
      <th>Per Student Budget</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Math</th>
      <th>Overall Passing Rate</th>
    </tr>
    <tr>
      <th>school name</th>
      <th>type</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Rodriguez High School</th>
      <th>District</th>
      <td>3999</td>
      <td>24649428</td>
      <td>61.638980</td>
      <td>76.842711</td>
      <td>80.744686</td>
      <td>709.077269</td>
      <td>709.077269</td>
      <td>78.793698</td>
    </tr>
    <tr>
      <th>Huang High School</th>
      <th>District</th>
      <td>2917</td>
      <td>24649428</td>
      <td>84.502667</td>
      <td>76.629414</td>
      <td>81.182722</td>
      <td>972.094618</td>
      <td>972.094618</td>
      <td>78.906068</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <th>District</th>
      <td>2739</td>
      <td>24649428</td>
      <td>89.994261</td>
      <td>77.102592</td>
      <td>80.746258</td>
      <td>1035.268346</td>
      <td>1035.268346</td>
      <td>78.924425</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <th>District</th>
      <td>2949</td>
      <td>24649428</td>
      <td>83.585717</td>
      <td>76.711767</td>
      <td>81.158020</td>
      <td>961.546287</td>
      <td>961.546287</td>
      <td>78.934893</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <th>District</th>
      <td>4761</td>
      <td>24649428</td>
      <td>51.773636</td>
      <td>77.072464</td>
      <td>80.966394</td>
      <td>595.589162</td>
      <td>595.589162</td>
      <td>79.019429</td>
    </tr>
  </tbody>
</table>
</div>



## Math Scores by Grade

* Create a table that lists the average Reading Score for students of each grade level (9th, 10th, 11th, 12th) at each school.

  * Create a pandas series for each grade. Hint: use a conditional statement.
  
  * Group each series by school
  
  * Combine the series into a dataframe
  
  * Optional: give the displayed data cleaner formatting


```python
students_gr_grade_df = student_data.groupby(['school name','grade']).reading_score.mean().unstack(level=1)



students_gr_grade_df




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
      <th>grade</th>
      <th>10th</th>
      <th>11th</th>
      <th>12th</th>
      <th>9th</th>
    </tr>
    <tr>
      <th>school name</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Bailey High School</th>
      <td>80.907183</td>
      <td>80.945643</td>
      <td>80.912451</td>
      <td>81.303155</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>84.253219</td>
      <td>83.788382</td>
      <td>84.287958</td>
      <td>83.676136</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>81.408912</td>
      <td>80.640339</td>
      <td>81.384863</td>
      <td>81.198598</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>81.262712</td>
      <td>80.403642</td>
      <td>80.662338</td>
      <td>80.632653</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>83.706897</td>
      <td>84.288089</td>
      <td>84.013699</td>
      <td>83.369193</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>80.660147</td>
      <td>81.396140</td>
      <td>80.857143</td>
      <td>80.866860</td>
    </tr>
    <tr>
      <th>Holden High School</th>
      <td>83.324561</td>
      <td>83.815534</td>
      <td>84.698795</td>
      <td>83.677165</td>
    </tr>
    <tr>
      <th>Huang High School</th>
      <td>81.512386</td>
      <td>81.417476</td>
      <td>80.305983</td>
      <td>81.290284</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>80.773431</td>
      <td>80.616027</td>
      <td>81.227564</td>
      <td>81.260714</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <td>83.612000</td>
      <td>84.335938</td>
      <td>84.591160</td>
      <td>83.807273</td>
    </tr>
    <tr>
      <th>Rodriguez High School</th>
      <td>80.629808</td>
      <td>80.864811</td>
      <td>80.376426</td>
      <td>80.993127</td>
    </tr>
    <tr>
      <th>Shelton High School</th>
      <td>83.441964</td>
      <td>84.373786</td>
      <td>82.781671</td>
      <td>84.122642</td>
    </tr>
    <tr>
      <th>Thomas High School</th>
      <td>84.254157</td>
      <td>83.585542</td>
      <td>83.831361</td>
      <td>83.728850</td>
    </tr>
    <tr>
      <th>Wilson High School</th>
      <td>84.021452</td>
      <td>83.764608</td>
      <td>84.317673</td>
      <td>83.939778</td>
    </tr>
    <tr>
      <th>Wright High School</th>
      <td>83.812757</td>
      <td>84.156322</td>
      <td>84.073171</td>
      <td>83.833333</td>
    </tr>
  </tbody>
</table>
</div>



## Reading Score by Grade 

* Perform the same operations as above for reading scores


```python
students_gr_grade_df = student_data.groupby(['school name','grade']).math_score.mean().unstack(level=1)



students_gr_grade_df
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
      <th>grade</th>
      <th>10th</th>
      <th>11th</th>
      <th>12th</th>
      <th>9th</th>
    </tr>
    <tr>
      <th>school name</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Bailey High School</th>
      <td>76.996772</td>
      <td>77.515588</td>
      <td>76.492218</td>
      <td>77.083676</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>83.154506</td>
      <td>82.765560</td>
      <td>83.277487</td>
      <td>83.094697</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>76.539974</td>
      <td>76.884344</td>
      <td>77.151369</td>
      <td>76.403037</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>77.672316</td>
      <td>76.918058</td>
      <td>76.179963</td>
      <td>77.361345</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>84.229064</td>
      <td>83.842105</td>
      <td>83.356164</td>
      <td>82.044010</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>77.337408</td>
      <td>77.136029</td>
      <td>77.186567</td>
      <td>77.438495</td>
    </tr>
    <tr>
      <th>Holden High School</th>
      <td>83.429825</td>
      <td>85.000000</td>
      <td>82.855422</td>
      <td>83.787402</td>
    </tr>
    <tr>
      <th>Huang High School</th>
      <td>75.908735</td>
      <td>76.446602</td>
      <td>77.225641</td>
      <td>77.027251</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>76.691117</td>
      <td>77.491653</td>
      <td>76.863248</td>
      <td>77.187857</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <td>83.372000</td>
      <td>84.328125</td>
      <td>84.121547</td>
      <td>83.625455</td>
    </tr>
    <tr>
      <th>Rodriguez High School</th>
      <td>76.612500</td>
      <td>76.395626</td>
      <td>77.690748</td>
      <td>76.859966</td>
    </tr>
    <tr>
      <th>Shelton High School</th>
      <td>82.917411</td>
      <td>83.383495</td>
      <td>83.778976</td>
      <td>83.420755</td>
    </tr>
    <tr>
      <th>Thomas High School</th>
      <td>83.087886</td>
      <td>83.498795</td>
      <td>83.497041</td>
      <td>83.590022</td>
    </tr>
    <tr>
      <th>Wilson High School</th>
      <td>83.724422</td>
      <td>83.195326</td>
      <td>83.035794</td>
      <td>83.085578</td>
    </tr>
    <tr>
      <th>Wright High School</th>
      <td>84.010288</td>
      <td>83.836782</td>
      <td>83.644986</td>
      <td>83.264706</td>
    </tr>
  </tbody>
</table>
</div>



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


```python
per student budget

ins=[575,600,625,650,675]
binnames=["575 to 600","600 to 625","625 to 650","650 to 675"]
School_summary_df2["Spending ranges Per student"] = pd.cut(School_summary_df2["Per Student Budget"],bins,labels=binnames)

df = whatever cols needed
School_summary_df = School_summary_df2[["Average Math Score","Average Reading Score","% Passing Math","% Passing Reading",
                                      "Overall Passing Rate", "Spending ranges Per student"]]
School_summary_df = School_summary_df3.groupby('Spending ranges Per student')

```


      File "<ipython-input-13-cff1499cf0d4>", line 1
        per student budget
                  ^
    SyntaxError: invalid syntax
    


## Scores by School Size

* Perform the same operations as above, based on school size.


```python
# Sample bins. Feel free to create your own bins.
size_bins = [0, 1000, 2000, 5000]
group_names = ["Small (<1000)", "Medium (1000-2000)", "Large (2000-5000)"]
```

## Scores by School Type

* Perform the same operations as above, based on school type.
