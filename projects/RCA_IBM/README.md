# **Root Cause Analysis of Financial Consumer Complaints : A Case Study Using IBM Granite on the CFPB Dataset**
Maulana Zulfikar Aziz

maulanazulfikarrz@gmail.com
## **Project Overview**
### Background
The protection of financial consumers has emerged as a critical focus area, receiving growing attention in recent years [1]. Financial institutions handle thousands of consumer complaints every month, many of which are submitted in free-text form. These narratives often contain valuable information about the issues customers face, but their unstructured nature makes them challenging to process, classify, and analyze efficiently. Manual Root Cause Analysis (RCA) requires significant time and resources, potentially delaying resolution and limiting the ability to identify recurring problems. As the volume of consumer complaints continues to rise, there is a pressing need for a more efficient and effective method of handling and resolving these issues.

To address this challenge, this project utilizes complaint data from the Consumer Financial Protection Bureau (CFPB), an independent U.S. government agency established in 2011 to protect consumers from unfair, deceptive, or abusive financial practices [2]. The dataset contains detailed complaint narratives along with product, issue, and company information, providing a rich source for analysis. By applying IBM Granite, a large language model capable of natural language understanding and reasoning, the project aims to automate the classification of complaints into standardized root cause categories. This approach enables faster identification of key problem areas and supports data-driven decision-making for service improvement.

### Problem Statement
Identifying the underlying root causes from a large dataset with free-text complaints is difficult and time-consuming without automation.

### Goals
Automate classification of consumer complaints into root cause categories and provide actionable insights for corrective measures.

### Approach Summary
1. Environment Setup : Install and import required libraries, including IBM Granite Model for natural language processing.

2. Data Understanding & Cleaning : Explore and prepare the CFPB complaint dataset by reviewing product, subproduct, issue, and company fields. Categorize issues into 15 standardized categories using IBM Granite Model.

3. Text Analysis : Analyze consumer complaint narratives through word clouds, text length distribution, and volume trends over time by issue, product, and company.

4. Incident Analysis : Identify complaint spikes and select two for deeper investigation.

5. Root Cause Analysis : Apply IBM Granite Model for automated cause classification and simplified categorization. Generate Pareto charts for spike periods and baseline data to highlight key problem drivers.

## **Data Understanding**
### About Data
The dataset that used in this project is financial consumer complaints from Consumer Financial Protection Bureau (CFPB) between June 2025 - July 2025. The dataset can be accessed at : [CFPB Consumer Complaint Database](https://www.consumerfinance.gov/data-research/consumer-complaints/search/?date_received_max=2025-07-31&date_received_min=2025-06-01&has_narrative=true&page=1&searchField=all&size=25&sort=created_date_desc&tab=List)

Here’s an overview of columns in the CFPB consumer complaints data, based on API documentation:
| Column                 | Description                                                                 |
|----------------------------|-----------------------------------------------------------------------------|
| Date received              | Date CFPB received the complaint                                          |
| Product      | The type of product the consumer identified in the complaint           |
| Sub-product               | The type of sub-product the consumer identified in the complaint |
| Issue          | The issue the consumer identified in the complaint                      |
| Sub-issue                 | The sub-issue the consumer identified in the complaint|
| Consumer complaint narrative         | Consumer complaint narrative is the consumer-submitted description of "what happened" from the complaint. Consumers must opt-in to share their narrative. We will not publish the narrative unless the consumer consents, and consumers can opt-out at any time. The CFPB takes reasonable steps to scrub personal information from each complaint that could be used to identify the consumer.             |
| Company public response                   | The company's optional, public-facing response to a consumer's complaint. Companies can choose to select a response from a pre-set list of options that will be posted on the public database. For example, "Company believes complaint is the result of an isolated error." |
| Company                    | The complaint is about this company                               |
| State                 | The state of the mailing address provided by the consumer |
| ZIP code                  | The mailing ZIP code provided by the consumer |
| Tags                  | Data that supports easier searching and sorting of complaints submitted by or on behalf of consumers. |
| Consumer consent provided?                    | Identifies whether the consumer opted in to publish their complaint narrative. We do not publish the narrative unless the consumer consents and consumers can opt-out at any time. |
| Submitted via              | How the complaint was submitted to the CFPB                         |
| Date sent to company       | The date the CFPB sent the complaint to the company                         |
| Timely response?                  | Whether the company gave a timely response |
| Consumer disputed                 | Whether the consumer disputed the company’s response |
| Complaint ID                  | The unique identification number for a complaint |

The dataset initially contained 45,224 rows. After cleaning (removing duplicates and null values), it now contains 36,194 rows.
## **Insight & Findings**
### Dataset Overview
<div align="center">
  <img src="https://github.com/Maulanaaz/data-science-projects/blob/main/projects/RCA_IBM/Images/RCA_dataset_5.png?raw=True" alt="Dataset Info" width="800"/>
</div>
<p align="center"><em>Figure 1. Dataset View</em></p>
<div align="center">
  <img src="https://github.com/Maulanaaz/data-science-projects/blob/main/projects/RCA_IBM/Images/RCA_info_dataset.png?raw=True" alt="Dataset Info" width="200"/>
</div>
<p align="center"><em>Figure 2. Dataset Summary</em></p>

Figure 1 provides an overview of the dataset, while Figure 2 shows its summary. The dataset contains 45,224 rows and 17 columns. Since not all columns are relevant to our analysis, we first perform column selection and data cleaning (removing duplicates and unnecessary fields) to ensure the analysis runs smoothly and yields reliable results.

For this project, we focus only on the following columns:

- `Date received`
- `Product`
- `Sub-product`
- `Issue`
- `Consumer complaint narrative`
- `Company`

<br>

<div align="center">
  <img src="https://github.com/Maulanaaz/data-science-projects/blob/main/projects/RCA_IBM/Images/RCA_clean_dataset.png?raw=True" alt="Dataset Info" width="450"/>
</div>
<p align="center"><em>Figure 3. Cleaned Dataset Overview</em></p>

<div align="center">
  <img src="https://github.com/Maulanaaz/data-science-projects/blob/main/projects/RCA_IBM/Images/RCA_summary_cleaned_data.png?raw=True" alt="Dataset Info" width="400"/>
</div>
<p align="center"><em>Figure 4. Cleaned Dataset Summary</em></p>

Figure 3 presents the overview of the cleaned dataset, while Figure 4 shows its summary. As seen in Figure 4, the dataset now contains 36,194 rows and 6 columns after column selection and data cleaning. Additionally, the Date received column has been converted to the datetime64 data type. Now that our dataset is cleaned, we’re ready to dive in and uncover some useful insights.
### Complaint Distribution
The first step after cleaning the data is to explore the distribution of the main column, that is `Consumer complaint narrative,` across other key columns : `Product`, `Sub-Product`, `Issue`, an additional column called `simplified_issue` (explained below), and `Company`. This helps us build a clearer understanding of consumer complaints.
#### 1. Complaint Distribution by Product
![Complaint Volume by Product](https://github.com/Maulanaaz/data-science-projects/blob/main/projects/RCA_IBM/Images/complaint_by_product.png?raw=True)
<p align="center"><em>Figure 5. Complaint volume by product.</em></p>

Figure 5 shows that `Credit reporting or other personal consumer reports` has the highest complaint volume with **24,972** complaints. The difference compared to other products is highly significant, indicating that this product should be investigated further. The second highest complaint volume is `Debt collection ` with **3,182** complaints, followed by `Checking or savings account` with **2,429** complaints, and then `Credit card` with **2,083** complaints. The remaining products each received fewer than 1,000 complaints.

#### 2. Complaint Distribution by Sub-product
![Complaint Volume by Sub-product](https://github.com/Maulanaaz/data-science-projects/blob/main/projects/RCA_IBM/Images/complaint_by_subproduct.png?raw=True)
<p align="center"><em>Figure 6. Complaint volume by sub-product.</em></p>

The sub-product column initially had 52 unique values. To simplify the visualization, I focused on the top 13 and grouped the rest under `Other Problem`.

As shown in Figure 6, the sub-product `Credit reporting` has the highest complaint volume with 24,719 complaints. This is a significant number compared to other sub-products, and it aligns with the Product column where `Credit reporting or other personal consumer reports` also shows a high volume. This indicates that Credit reporting should be investigated further, and it will serve as the main focus of our Root Cause Analysis.

#### 3. Complaint Distribution by Issue
![Complaint Volume by Issue](https://github.com/Maulanaaz/data-science-projects/blob/main/projects/RCA_IBM/Images/complaint_by_issue.png?raw=True)
<p align="center"><em>Figure 7. Complaint volume by Issue</em></p>

The `Issue` column initially contained 83 unique values. To simplify the analysis, we focused on the top 13 and grouped the remaining ones under a new category called `Other Problem`.

As shown in Figure 7, two categories dominate the complaints: `Incorrect information on your report` and `Improper use of your report`. Both of these are closely related to reporting issues. This aligns with the complaint distribution by Product and Sub-product, where most complaints are also tied to reporting. Therefore, we will focus on this issue in the next stage of the analysis.

Before moving to the next analysis, we need a clearer categorization of the issues. Instead of only keeping the top 13 and grouping the rest under Other Problem, we will reorganize all 83 issue categories into 15 broader categories, which will be explained in the following section.

#### 4. Complaint Distribution by Simplified Issue

To get a more accurate analysis of complaint distribution by Issue, we reorganized all 83 categories into 15 broader groups. For this task, we used IBM’s Generative AI model, Granite (**granite-3.3-8b-instruct**). Leveraging this model helped us save time compared to manual categorization and reduced potential bias in grouping the issues.
<div align="center">
  <img src="https://github.com/Maulanaaz/data-science-projects/blob/main/projects/RCA_IBM/Images/RCA_Granite1.png?raw=True" alt="Dataset Info" width="250"/>
</div>
<p align="center"><em>Figure 8. Simplified issues categorization</em></p>

By using a well-structured prompt that included clarification, context, examples, a specified format, and adjusted complexity, we obtained the results shown in Figure 8. The model successfully grouped the 83 issues into 15 broader categories.

![Complaint Volume by Simplified Issue](https://github.com/Maulanaaz/data-science-projects/blob/main/projects/RCA_IBM/Images/complaint_by_simplifiedissue.png?raw=True)
<p align="center"><em>Figure 9. Complaint Volume by Simplified Issue</em></p>

After mapping the simplified issues generated by IBM Granite to the dataset (see Figure 9), we found that the highest complaint volume is in the category `Inaccurate Reporting`. This confirms our earlier analysis, showing that the main problem lies in `Inaccurate Reporting` within the `Credit Reporting` product.

#### 5. Complaint Distribution by Company
**Disclaimer**: Company names mentioned in this analysis appear strictly for data analysis purposes. Their inclusion does not imply criticism, endorsement, or disapproval.

The dataset contains complaints against 958 unique companies. To simplify the analysis, we will focus only on the top 5 companies with the highest complaint volumes.


![Complaint Volume by Company](https://github.com/Maulanaaz/data-science-projects/blob/main/projects/RCA_IBM/Images/complaint_by_company1.png?raw=True)
<p align="center"><em>Figure 10. Top 5 Companies by Complaints</em></p>

As shown in Figure 10, three companies stand out with the highest complaint volumes, together accounting for about 61.2% of all complaints. Next, we will examine whether most of these complaints are related to the `Inaccurate Reporting` issue.

![Complaint Volume by Company](https://github.com/Maulanaaz/data-science-projects/blob/main/projects/RCA_IBM/Images/complaint_by_company2.png?raw=True)
<p align="center"><em>Figure 11. Top 5 Companies by Complaints (Inaccurate Reporting) </em></p>

As shown in Figure 11, the same three companies also account for a large share of complaints related to inaccurate reporting and together making up about 84.2%. This indicates that these companies have a significant impact on the high volume of customer complaints, especially regarding the `Inaccurate Reporting` issue.

### Consumer Narrative Insights
#### 1. WordClouds
![WordClouds](https://github.com/Maulanaaz/data-science-projects/blob/main/projects/RCA_IBM/Images/wordclouds.png?raw=True)
<p align="center"><em>Figure 12. WordClouds of Consumer Narrative Complaints </em></p>

Wordclouds show the frequency of words in the dataset, where larger words appear more often. As seen in Figure 12, the most frequent words include `Credit` , `report`, `Account`, `identity`, `theft`, `balance`, `Number`, and `Reporting`. This suggests that credit report-related issues are the most common theme in consumer complaint narratives, which aligns with our previous analysis.

#### 2. Distribution of Words and Characters
![Distribution of Words and Characters](https://github.com/Maulanaaz/data-science-projects/blob/main/projects/RCA_IBM/Images/wordschar_dist.png?raw=True)
<p align="center"><em>Figure 13. Distribution of Words and Characters</em></p>

Before we use the `Consumer complaint narratives` column for deeper analysis, it’s important to first examine the distribution of words and characters in that column to ensure smoother analysis. As shown in Figure 13, the data has a right-skewed distribution with some outliers, specifically, a few rows have word counts exceeding 1,000 and character counts over 10,000.

#### 3. Trends Over Time
The next step is to analyze the trends in `Consumer complaint narratives` to identify specific insights, such as which dates have the highest number of complaints and whether any anomalies are present.

![Trends Over Time](https://github.com/Maulanaaz/data-science-projects/blob/main/projects/RCA_IBM/Images/trens_complaints.png?raw=True)
<p align="center"><em>Figure 14. Daily Consumer Complaint Narratives </em></p>

As shown in Figure 14, complaint volumes spiked from early June to mid-June, compared to other dates. Afterward, the number of complaints remained relatively stable until the end of July. Therefore, we will focus our analysis on these spikes to better understand why they occurred.

We will break down the `Consumer complaint narratives` trends over time by other columns namely `Simplified Issue`, `Issue`, `Product`, `Sub-product`, and `Company` to gain a clearer and more comprehensive view of what is happening.

##### **a. Daily Complaint Volume by Simplified Issue**
![Simplified Issue](https://github.com/Maulanaaz/data-science-projects/blob/main/projects/RCA_IBM/Images/daily_simplified_issue.png?raw=True)

<p align="center"><em>Figure 15. Daily Complaint Volume by Simplified Issue </em></p>

Figure 15 shows that most complaints come from Inaccurate Reporting, which occurs frequently between June 1 and June 17, as indicated by the red areas in the heatmap. This suggests that the spike is largely dominated by the `Inaccurate Reporting` issue.

##### **b. Daily Complaint Volume by Issue**
![Issue](https://github.com/Maulanaaz/data-science-projects/blob/main/projects/RCA_IBM/Images/daily_issue.png?raw=True)

<p align="center"><em>Figure 16. Daily Complaint Volume by Issue </em></p>

Consistent with our earlier analysis, Figure 16 shows that the issues `Improper use of your report` and `Incorrect information on your report` recorded high complaint volumes between June 1 and June 17. This reinforces the insight that reporting-related issues are the primary drivers of the spike.

##### **c. Daily Complaint Volume by Product**
![Product](https://github.com/Maulanaaz/data-science-projects/blob/main/projects/RCA_IBM/Images/daily_product.png?raw=True)

<p align="center"><em>Figure 17. Daily Complaint Volume by Product </em></p>

As shown in Figure 17, the product driving the spike is `Credit reporting or other personal information reports`, which recorded a high volume of complaints between June 1 and June 17. This further highlights the significance of reporting-related issues up to this point.

##### **d. Daily Complaint Volume by Sub-product**
![Sub-product](https://github.com/Maulanaaz/data-science-projects/blob/main/projects/RCA_IBM/Images/daily_subproduct.png?raw=True)

<p align="center"><em>Figure 18. Daily Complaint Volume by Sub-product </em></p>

Breaking down the Product column, Figure 18 shows that the most complained Sub-product is `Credit reporting`. This Sub-product recorded a high volume of complaints during the spike period, indicating that it is likely the key area where we need to focus our root cause analysis.

##### **e. Daily Complaint Volume by Company**
![Sub-product](https://github.com/Maulanaaz/data-science-projects/blob/main/projects/RCA_IBM/Images/daily_company.png?raw=True)

<p align="center"><em>Figure 19. Daily Complaint Volume by Company</em></p>

Based on the Company view, the spike appears to split into two phases: from 1 June to 8 June, when most companies experienced a higher complaint volume, and from 9 June to 17 June, when a single company recorded a disproportionately high number of complaints. This may suggest the presence of company-specific factors contributing to the spike during the latter period.

### Spike Analysis
From the previous analysis, we can see two main spikes in daily complaints: the first from June 1–8, and the second from June 9–17. To dig deeper into what’s driving these jumps, we will focus on each spike and use the `Simplified Issue` column to get a better sense of what’s really happening behind the higher complaint volumes. In addition, we define the Baseline period as all dates outside of Spike 1 and Spike 2.

#### 1. Spike 1
![Spike 1](https://github.com/Maulanaaz/data-science-projects/blob/main/projects/RCA_IBM/Images/RCA_Spike1.png?raw=True)
<p align="center"><em>Figure 20. Complaint Spike 1 Analysis</em></p>

As shown in Figure 20, `Inaccurate Reporting` was the biggest contributor to the spike, making up 67.37% of the cases that is 16.71% higher than the baseline. The top spike phrases for this issue include “credit,” “account,” “report,” “information,” and “accounts,” which align well with the category itself.

#### 2. Spike 2
![Spike 2](https://github.com/Maulanaaz/data-science-projects/blob/main/projects/RCA_IBM/Images/RCA_Spike2.png?raw=True)
<p align="center"><em>Figure 21. Complaint Spike 2 Analysis</em></p>

Similar to Spike 1, `Inaccurate Reporting` was also the main driver of Spike 2, accounting for 67.37% of the cases, that is 16.77% higher than the baseline. The top spike phrases were essentially the same as those in Spike 1.

This suggests that `Inaccurate Reporting` is a serious issue that needs deeper investigation to uncover its root cause. For the next analysis, we will focus specifically on `Inaccurate Reporting` as the main driver behind the complaint spikes.


### Root Cause Analysis
Now that we have identified `Inaccurate Reporting` as the main problem, we will conduct a Root Cause Analysis specifically on this issue to develop recommendations for preventing it from recurring in the future. For this analysis, we will be working with three datasets: the `Spike 1` period dataset, the `Spike 2` period dataset, and the `Baseline` period dataset.

We will be using the IBM Granite Model, specifically **granite-3.3-8b-instruct**, to automatically identify the causes within each consumer complaint narrative. Since resources are limited, we will only take 750 rows from each dataset : `Spike 1`, `Spike 2`, and `Baseline` using random sampling.

By using a well-structured prompt that complete with clarifications, context, examples, a defined format, and adjusted complexity, we were able to extract the causes from each consumer complaint narrative across the `Spike 1`, `Spike 2`, and `Baseline` datasets. The next step is to group the causes from each dataset into 10 broader categories. While this gives us up to 30 categories across Spike 1, Spike 2, and Baseline, some of them may overlap or be similar. This overlap is useful, as it helps us capture not only unique issues in each period but also recurring themes that appear across multiple datasets. To handle this efficiently, we willll once again use the IBM Granite model to automate the process.

#### 1. Root Cause Analysis : Spike 1
<div align="center">
  <img src="https://github.com/Maulanaaz/data-science-projects/blob/main/projects/RCA_IBM/Images/RCA_Spike1_Cat.png?raw=True" alt="Dataset Info" width="450"/>
</div>
<p align="center"><em>Figure 22. Spike 1 Cause Categorization</em></p>

![Pareto Spike 1](https://github.com/Maulanaaz/data-science-projects/blob/main/projects/RCA_IBM/Images/pareto_spike1.png?raw=True)
<p align="center"><em>Figure 23. Pareto Chart of Spike 1</em></p>


By using the IBM Granite Model, we generated 10 cause categories, as shown in Figure 22, and then mapped the complaint volumes across them in a Pareto Chart (Figure 23). The chart reveals that 80% of complaints come from just five key categories: `Identity Theft Consequences`, `Inaccurate Credit Reporting, Inaccurate Account Information`, `Fraudulent Accounts`, and `Unauthorized Account`. This means that by focusing on these areas, we could address the majority of complaint issues, while the remaining categories, though still relevant, have a smaller overall impact.


#### 2. Root Cause Analysis : Spike 2
<div align="center">
  <img src="https://github.com/Maulanaaz/data-science-projects/blob/main/projects/RCA_IBM/Images/RCA_Spike2_Cat.png?raw=True" alt="Dataset Info" width="450"/>
</div>
<p align="center"><em>Figure 24. Spike 2 Cause Categorization</em></p>

![Pareto Spike 1](https://github.com/Maulanaaz/data-science-projects/blob/main/projects/RCA_IBM/Images/pareto_spike2.png?raw=True)
<p align="center"><em>Figure 25. Pareto Chart of Spike 2</em></p>

Similar to Spike 1, we categorized the causes for Spike 2 as shown in Figure 24. The Pareto Chart in Figure 25 shows that 80% of complaints come from just six categories: `Inaccurate Credit Reporting Practices`, `Unauthorized Credit Report Entries`, `Identity Theft-Related Fraud`, `Data Breach-Related Identity Theft`, `Unresolved Disputes and Investigations`, and `Inaccurate Account Information`. This highlights that a relatively small set of recurring issues is driving the majority of complaints during this period.

#### 3. Root Cause Analysis : Baseline
<div align="center">
  <img src="https://github.com/Maulanaaz/data-science-projects/blob/main/projects/RCA_IBM/Images/RCA_Baseline_Cat.png?raw=True" alt="Dataset Info" width="450"/>
</div>
<p align="center"><em>Figure 26. Baseline Cause Categorization</em></p>

![Pareto Baseline](https://github.com/Maulanaaz/data-science-projects/blob/main/projects/RCA_IBM/Images/pareto_baseline.png?raw=True)
<p align="center"><em>Figure 27. Pareto Chart of Baseline</em></p>

Similar to Spike 2, the Baseline cause categorization is shown in Figure 26. The Pareto Chart in Figure 27 reveals that 80% of complaints fall into six categories: `Credit Reporting Violations`, `Credit Bureau Reporting Mistakes`, `Identity Theft-Related Issues`, `Dispute Resolution Failures`, `Fraudulent Account Opening`, and `Unauthorized Credit Inquiries`. These categories closely resemble those found in both Spike 1 and Spike 2, suggesting that the core issues driving complaints remain consistent over time.

## **Final Conlusion & Recommendations**
### Conclusion
After preparing the dataset, exploring its structure, and conducting the Root Cause Analysis, we can summarize the key takeaways for each period:

The Spike 1 period (1 June - 8 June 2025) :  When most companies experienced a higher complaint volume. The most contributed cause categories were:
1. `Identity Theft Consequences`, 
2. `Inaccurate Credit Reporting`,
3. `Inaccurate Account Information`, 
4. `Fraudulent Accounts`, and
5. `Unauthorized Account`

The Spike 2 period (9 June - 17 June 2025) : When a single company recorded a disproportionately high number of complaints. The most contributed cause categories were:
1. `Inaccurate Credit Reporting Practices`, 
2. `Unauthorized Credit Report Entries`, 
3. `Identity Theft-Related Fraud`, 
4. `Data Breach-Related Identity Theft`, 
5. `Unresolved Disputes and Investigations`, and 
6. `Inaccurate Account Information`

The Baseline period : All dates outside of Spike 1 and Spike 2. The most contributed cause categories were:
1. `Credit Reporting Violations`, 
2. `Credit Bureau Reporting Mistakes`, 
3. `Identity Theft-Related Issues`, 
4. `Dispute Resolution Failures`, 
5. `Fraudulent Account Opening`, and 
6. `Unauthorized Credit Inquiries`

<br></br>
Spike 1 highlights a surge in complaints linked to fraud and consumer security, suggesting that multiple companies faced similar challenges around fraudulent activities during this period.

Spike 2 points to a company-specific issue, with recurring themes of inaccurate reporting and identity theft-related fraud, hinting at possible internal process or system failures in that company.

The Baseline period shows that even outside of spikes, credit reporting errors and identity theft remain persistent issues, indicating that these problems are systemic rather than isolated events.

Across all three periods : Spike 1, Spike 2, and Baseline, the most recurring issues are tied to **Inaccurate Credit Reporting** and **Identity Theft–related problems**. These two themes consistently appear as major contributors to complaint volumes, whether during widespread spikes or in the more stable baseline period. This suggests that the challenges are systemic rather than isolated, and addressing them would have the greatest overall impact

### Recommendation
Based on the Root Cause Analysis, two issues stand out as the most persistent drivers of consumer complaints: **Inaccurate Credit Reporting** and **Identity Theft–related problems**. To address these causes, the following steps are recommended:
1. Improve Credit Reporting Accuracy
    - Strengthen data validation processes before submitting information to credit bureaus.
    - Implement regular audits of credit report data to detect and correct errors early.
    - Enhance communication channels between consumers, companies, and credit bureaus for faster dispute resolution.

2. Enhance Identity Theft Protection
    - Invest in stronger authentication and fraud detection systems to prevent unauthorized activities.
    - Provide consumers with better monitoring tools and alerts to quickly identify suspicious activity.
    - Collaborate with regulators and law enforcement to address large-scale identity theft and data breaches.

3. Focus on Company-Specific Risks
    - For companies that show disproportionate spikes (as seen in Spike 2), conduct targeted internal reviews of complaint-handling and reporting processes.
    - Identify whether these spikes are due to systemic failures, operational lapses, or compliance gaps.

4. Continuous Monitoring and Analysis
    - Establish ongoing monitoring of complaint trends to detect emerging issues before they escalate.
    - Use machine learning and natural language processing models, like IBM Granite, to automate root cause detection at scale.

By acting on these recommendations, stakeholders can not only reduce complaint volumes but also rebuild consumer trust through more reliable credit reporting and stronger fraud protection.

## **AI Support Explanation**
### 1. Categorizing 83 Issue Categories into broader 15 Issue Categories
To streamline the analysis, the IBM Granite Model was used to consolidate 83 detailed issue categories into 15 broader categories. This step was crucial in reducing complexity and highlighting the most significant complaint patterns. By grouping similar issues together, the model ensures the analysis remains both comprehensive and interpretable.

The results show that the model successfully produced 15 well-defined categories, which now serve as the foundation for deeper insights. The prompt used for categorization and the corresponding output are illustrated in Figure 28 and Figure 29 below.

![](https://github.com/Maulanaaz/data-science-projects/blob/main/projects/RCA_IBM/Images/RCA_AI_1.png?raw=True)
<p align="center"><em>Figure 28. Prompt for Issue Categorization</em></p>
<div align="center">
  <img src="https://github.com/Maulanaaz/data-science-projects/blob/main/projects/RCA_IBM/Images/RCA_AIOUT_1.png?raw=True" alt="Dataset Info" width="250"/>
</div>
<p align="center"><em>Figure 29. Output for Issue Categorization</em></p>

### 2. Mapping the issue categories to the broader issue categories

After consolidating the 83 detailed issues into 15 broader categories, the IBM Granite Model was then applied to map each individual row of data into the appropriate broader category. This automation significantly reduces the time and effort compared to manual categorization, while ensuring a more consistent and scalable classification process.

The example prompt used for mapping and the corresponding output are illustrated in Figure 30 and Figure 31 below.

![](https://github.com/Maulanaaz/data-science-projects/blob/main/projects/RCA_IBM/Images/RCA_AI_2.png?raw=True)
<p align="center"><em>Figure 30. Prompt for Mapping the Issue</em></p>
<div align="center">
  <img src="https://github.com/Maulanaaz/data-science-projects/blob/main/projects/RCA_IBM/Images/RCA_AIOUT_2.png?raw=True" alt="Dataset Info" width="400"/>
</div>
<p align="center"><em>Figure 31. Output for Mapping the Issue</em></p>

### 3. Spotting Root Causes from The Consumer Narrative Complaints
Here, the IBM Granite Model was utilized to extract the root causes directly from the free-text consumer complaint narratives. This approach not only saves time and reduces complexity but also ensures consistent and reliable identification of cause-related text. 

The example prompts used to identify the causes from consumer narrative complaints and the corresponding output are illustrated in Figure 32 and Figure 33 below.

![](https://github.com/Maulanaaz/data-science-projects/blob/main/projects/RCA_IBM/Images/RCA_AI_3.png?raw=True)
<p align="center"><em>Figure 32. Prompt to identify the causes from consumer narrative complaints</em></p>
<div align="center">
  <img src="https://github.com/Maulanaaz/data-science-projects/blob/main/projects/RCA_IBM/Images/RCA_AIOUT_3_1.png?raw=True" alt="Dataset Info" width="600"/>
</div>
<p align="center"><em>Figure 33. Output to identify the causes from consumer narrative complaints</em></p>

### 4. Categorizing the Root Causes into 10 broader Categories
After identifying the root causes from the consumer narrative complaints, the IBM Granite Model was applied for a categorization task. It categorized the root causes of each dataset (Spike 1, Spike 2, and Baseline) into 10 broader categories. This approach is highly beneficial as it simplifies subsequent analysis, saves time, and ensures consistency.

The example prompts used to categorize the root causes and corresponding output are illustrated in Figure 34 and Figure 35 below.

![](https://github.com/Maulanaaz/data-science-projects/blob/main/projects/RCA_IBM/Images/RCA_AI_4.png?raw=True)
<p align="center"><em>Figure 34. Prompt to categorize the root causes</em></p>
<div align="center">
  <img src="https://github.com/Maulanaaz/data-science-projects/blob/main/projects/RCA_IBM/Images/RCA_AIOUT_4.png?raw=True" alt="Dataset Info" width="300"/>
</div>
<p align="center"><em>Figure 35. Output to categorize the root causes</em></p>

## **References**
[1] M. Siering, "Explainability and fairness of RegTech for regulatory enforcement: Automated monitoring of consumer complaints," Decision Support Systems, vol. 158, p. 113782, Jul. 2022.

[2] Consumer Financial Protection Bureau, “Consumer Financial Protection Bureau,” https://www.consumerfinance.gov/, [Accessed: Aug. 12, 2025].

