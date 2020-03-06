---
title: "First Midterm"
output: 
  html_document: 
    keep_md: yes
---

Due by 11:59 pm on Sunday, Oct. 20th.

### Overview
You will be submitting three items: [1] a README.md and [2] an R Markdown/Markdown report through GitHub, and [3] a reflection on Bb.
This is essentially a take-home exam, so you should no discuss this with anyone except your professor.
You are allowed (and expected, if necessary) to use appropriate online resources.
Be sure to cite any resource that you get help from and explain where/how you implemented it - this is part of your [reflection](#3-reflection).
You are also free to ask questions/clarifications of me as if I am the client, but I have no experience with R or GitHub.

You should start this **as early as possible** to ensure that you have time to walk away when things are not working to provide you with an opportunity to refocus with fresh eyes.
If things are not working, try starting with smaller chunks and make sure the results are doing what you think you told R to do.

Your final report should interweave code, output, and narrative.
You should not do any data manipulations manually.
The goal is to demonstrate that you can tackle all tasks with R coding, unless otherwise approved by your professor.

#### Evaluation

Your midterm project report will be evaluated using the [Midterm and Team Project Grading Standards](https://sta518.github.io/syllabus/assessment/#midterm-and-team-projects).
Your README and reflection will be evaluated using the [Meeting Preparation Grading Standards](https://sta518.github.io/syllabus/assessment/#meeting-preparation-1)
If either of your README or reflection earns an Unsatisfactory (U), your overall mark will be lowered one mark.

### Create Your `first-midterm` Repo

GitHub Classroom will automatically make a repository for you, called `first-midterm-your_github_username`, for you to work on your assignment.
Follow these instructions to get the repo:

1. Sign-in to [Bb](https://mybb.gvsu.edu)
2. Go to the **First Midterm** assignment page.

Here, you will find a url leading to a page on GitHub Classroom.
Visit this page and follow the instructions.
When you obtain your repository, it should contain a copy of this `first-midterm-instructions.md` file, a blank `README.md` file, a starter `first-midterm.Rmd` file, and a `data` folder containing three csv files: `county_city.csv`, `GTD.csv`, and `population.csv`.

#### 1. Edit `README.md` 

Your task here is to edit the `README.md` file in your repository to contain a sample of GitHub-flavored markdown features.
Specifically, your README should contain a brief description as to what the repo is, so that an unknown visitor landing on the repo can orient themselves.
You should also help the visitor navigate your repository (in whatever way you think is most appropriate).

Remember the resources you were directed to view during your work in the class activities and Meeting Preparations.
These are also organized in the [Additional Resources/Markdown](https://sta518.github.io/resources/markdown/) section of this site.

You can edit the README in your browser like we did in class, but I encourage you to experiment with editing it from within RStudio.
**FYI, this will be a private repo - only you and I will be able to see it.**

#### 2. Midterm Report


Efficient or "elegant" coding will result in higher marks than inefficient coding.
There is more than one way to accomplish these tasks and you should try to pick a more efficient way, when possible.
Higher marks will also be reserved for successful implementation of the `tidyverse` whenever possible.
Efficient code that accomplishes the tasks will be considered *satisfactory*, regardless of your use of the `tidyverse`.

While I have not enforced the notion of comments, you should embrace using them in this midterm project.
See Hadley's statements on comments in R for [general syntax](https://style.tidyverse.org/syntax.html#comments) and [functions](https://style.tidyverse.org/functions.html#comments-1).

Knit your completed `.Rmd` file and commit your **entire** project folder from RStudio Cloud to GitHub.

##### Data

There are four data sets in the `data` folder of this repo.
The primary data set is a subset of the Global Terrorism Database (GTD), years 2013-2016.
Two data sets deal with the population and poverty of U.S. cities.
A fourth data set contains county information of U.S. cities.

Descriptions of the variables in `GTD.csv` can be found in the [codebook](https://www.start.umd.edu/gtd/downloads/Codebook.pdf) (starting on page 12).
This document also contains infomation on other definitions, data collection methodology, etc.
You are only provided with a subset of the variables in the GTD database, so you will need to scroll through some variables that are not present in the data.
Understanding what these variables will be important for your [analysis](#21-assemble-final-data-set).

In the `poverty.csv` data set, the variable `PCTPOVALL_2015` is the percent of all residents under the poverty level in the county, `PCTPOV017_2015` is the percent of residents aged 0 to 17 under the poverty level in the county, and `MEDHHINC_2015` is the median household income in the county.

I believe that the `population.csv` and `county_city.csv` variables should be self-explanatory, but do not hesitate to ask for clarification.

##### 2.1 Assemble Final Data Set

You will write a final data set as a csv file to the `data` folder.
Name this file `midterm-data-your_github_username.csv`.
The number of rows should be the same as the number of rows in the GTD data set.

When you are working on this part, you should clearly explain everything that you did.
Tell the story of how you attacked the problem of assembling the final data set, in a way that a client that does not have experience with R would understand.
This goes beyond writing comments for your code.
I think that it is important for you to approach this task in your own way.
However, you should make all of what you did and why you did it very easy to follow.

You will need to see if any recoding needs to be done.
For example, missing values may be coded as numbers like `9` or `-99`, or as blank values.
As another example, in the poverty data set, the county is listed using the word "county" in each row, but it is not in the county/city data set (`county_city.csv`).

When you filter the poverty and population data sets for only relevant counties/cities in `GTD`, you'll need to make sure that county/city names are not spelled differently, and that there are not any case sensitive issues causing merging problems.
This will be evident if you cannot find a match for cities in the GTD with the poverty and population data sets.

Note that some cities in the population data set have a suffics of "`(pt.)`" as well.
It may be the case that you do not need any of these cities, but if you have problems merging, that could be a problem that needs to be dealt with.

Data cleaning after successful merging should involve making univariate summaries of each variable in the final data set.
For categorical variables, frequency tables for each category are a good way to catch any issues with the same category spelled two different ways.
For quantitative variables, numerical and graphical summaries are a good way to catch any odd values.
You will not need to turn in all of these, but you will need to supply those where a data cleaning decision was made (e.g., where categories need to be collapsed).

The variables `summary`, `propcomment`, and `scite1` are present to help you understand context (e.g., understanding outliers), and so I would not summarize these.
However, keeping them in the final data set is probably a good idea so they are handy for your analysis.

Dates from the GTD should be put in the "`yyyy-mm-dd`" format as you will be asked to plot some variables over time in your [analysis](#21-assemble-final-data-set).

I am purposely not giving you the structure of what you will turn in for this task, as that is what you would encounter in the "real" world.
Compared to most clients that I have worked with, I have been very generous with this initial information.

In summary, the specific details that will be looked for when assessing your midterm project are:

- How easy/logical your narrative is to read;
- Sufficent use of comments in the code;
- Efficient use of code (especially using the `tidyverse`);
- Recoding missing values (even for variables that ultimately are not used in your analysis);
- Explanation/documentation of problems/challenges in the original data sets (aside from missing values);
- Correct merging;
- Univariate numerical and graphical summaries to justify decisions, as needed; and
- Dates in format consistent with "`yyyy-mm-dd`".

##### 2.2 Analysis Tasks

You aren't required to do anything statistically sophisticated for this task, such as building a regression model.
All problems involve making appropriate graphs and tables.
You should make these figures as easy to read and understand for someone who is not as statistically sophisticated as you.
If you were the client, and wanted to understand the message behind the data as easily as possible, what would you like to see in the figures?

For each of these, you should supply a brief description of what conclusions can be drawn.
The point is to be efficient in explaining your findings to the client, especially which variables are more important than others, if there are any.
This is real data, and these are the questions the client would like answered, and sometimes the answer is we do not see any relationship (as long as you can justify it).

1. Make a plot of the U.S. with each incident present on the map according to its location.
  You should incorporate in the graph the city’s population size.
  Repeat for the three poverty variables. 
2. Make a plot of the number of incidents by month, according to type of incident as described by the variable `individual`, and separately by the variable `claimed`.
3. If we consider the number of wounded and killed combined to be of primary interest, explore the relationships of population size and the three poverty variables to this combined count.
  Label the most interesting data point in one of the graphs with the group name (`gname`) that committed the incident.
4. If we consider the variable `attacktype1_txt` to be of primary interest, explore the relationships of population size and the three poverty variables to `attacktype1_txt`.
  You will need to collapse any categories of `attacktype1_txt` that have 3 or few incidents into an “other” category.
5. Explore the relationship between your collapsed version of `attacktype1_txt` from the previous problem and the variable `property`, and separately the variable `individual`.

For this portion of the task, the specific details that will be looked for when assessing your midterm project are:

- How easy/logical your narrative is to read;
- Sufficent use of comments in the code;
- Efficient use of code (especially using the `tidyverse`);
- Effective use of numerical and graphical summaries; and
- Completeness of the five requested analytical tasks.

#### 3. Reflection
  
After you have completed Tasks 1 and 2 go back to the **First Midterm** assignment page on [Bb](https://mybb.gvsu.edu).
You will submit your reflection here that minimally includes:

- A *clickable* link to your First Midterm repository.
- A reflection on what was hard/easy, problems you came across and how you solved them, helpful tutorials you read, etc.
