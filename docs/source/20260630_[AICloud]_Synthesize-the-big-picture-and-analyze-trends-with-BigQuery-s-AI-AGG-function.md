---
source_url: "https://cloud.google.com/blog/products/data-analytics/deep-dive-into-bigquery-ai-agg-function/"
source_name: "Google Cloud Blog"
original_title: "Synthesize the big picture and analyze trends with BigQuery's AI.AGG function"
published_at: "2026-06-30T01:00:00+09:00"
collected_at_kst: "2026-06-30T08:03:19+09:00"
collection: ai-cloud-daily-news
content_hash: "8720571992be924a845ed1ebaa2bc2cc6de5293f7f2d89a2b05dee5dc1446079"
fetch_method: "direct urllib"
language: "en"
tags: ["google-cloud", "bigquery", "ai-agg"]
---

# Synthesize the big picture and analyze trends with BigQuery's AI.AGG function

- Source URL: https://cloud.google.com/blog/products/data-analytics/deep-dive-into-bigquery-ai-agg-function/
- Source name: Google Cloud Blog
- Published at: 2026-06-30T01:00:00+09:00
- Collected at KST: 2026-06-30T08:03:19+09:00
- Fetch method: direct urllib
- Content hash: `8720571992be924a845ed1ebaa2bc2cc6de5293f7f2d89a2b05dee5dc1446079`

## 원문 추출

Deep dive into BigQuery AI.AGG function | Google Cloud Blog 
Jump to Content Cloud Blog Contact sales Get started for free Cloud Blog 
Solutions & technology 
AI & Machine Learning 

API Management 

Application Development 

Application Modernization 

Chrome Enterprise 

Compute 

Containers & Kubernetes 

Data Analytics 

Databases 

DevOps & SRE 

Maps & Geospatial 

Security 
Security & Identity 

Threat Intelligence 

Infrastructure 

Infrastructure Modernization 

Networking 

Productivity & Collaboration 

SAP on Google Cloud 

Storage & Data Transfer 

Sustainability 

Ecosystem 
IT Leaders 

Industries 
Financial Services 

Healthcare & Life Sciences 

Manufacturing 

Media & Entertainment 

Public Sector 

Retail 

Supply Chain 

Telecommunications 

Partners 

Startups & SMB 

Training & Certifications 

Inside Google Cloud 

Google Cloud Next & Events 

Google Cloud Consulting 

Google Maps Platform 

Google Workspace 

Developers & Practitioners 

Transform with Google Cloud 
Contact sales Get started for free 

Data Analytics 
Synthesize the big picture and analyze trends with BigQuery's AI.AGG function 
June 30, 2026 

Thomas Anchor 
Software Engineer 
Alicia Williams 
Developer Advocate 
Try Gemini Enterprise Business Edition today 
The front door to AI in the workplace 
Try now 

We recently announced the preview of the BigQuery AI.AGG() function. With AI.AGG() , you can use natural-language instructions within a single line of SQL to summarize or synthesize information over millions of rows of unstructured or even multimodal data. 

While BigQuery already offers powerful AI functions that help you analyze individual rows of data , analyzing unstructured data at scale requires a different approach. AI.AGG() lets you ask questions from unstructured data such as logs and documents, for example: 

What are the top three feature requests among the negative product reviews? 

What kind of errors are users seeing most frequently, and how should I start investigating them? 

In which specific scenarios is our automated agent consistently failing to resolve customer issues? 

In this post, we'll dive deeper into the AI.AGG() function and look at a few of the use cases that it unlocks, including how it can be used in combination with BigQuery’s other managed AI functions for complex, intelligent data analysis. 

Analyzing system logs with AI.AGG() 

A great example of the power of AI.AGG() is analyzing system logging. Log messages, warnings, errors, and stack traces can contain extremely useful information for improving your service, but it can be time- and labor-intensive to investigate them manually — especially if you operate at scale and have thousands of them to review. 

With AI.AGG() , you can easily analyze many logs at once, grouping and prioritizing them to decide which ones to dig deeper into first. In fact, our BigQuery engineering team used this exact approach while developing AI.AGG() — using the function to help identify edge cases related to input handling for the feature itself! 

To demonstrate this, let’s analyze a public dataset of Apache Spark standard INFO logs available from Loghub . Often, clusters can run into issues like memory thrashing, clock drift, or broadcast bottlenecks without ever throwing a FATAL error. You can use AI.AGG() to analyze these seemingly normal logs for hidden inefficiencies. You can load the sample data file into BigQuery using any of the supported methods, such as the UI, CLI, or client libraries . The following example assumes you’ve loaded the log file into a dataset called bq_logs_demo and table named spark_logs_unstructured . 

Notice how we construct the prompt here. We explicitly give the model permission to say "everything is fine," which prevents it from hallucinating errors, while instructing it to hunt for specific anomalies: 

Loading... SELECT
 Component AS spark_component,
 COUNT(*) AS log_count,
 AI.AGG(
 Content,
 'Analyze these Spark system INFO logs. Provide a 2-sentence summary: First, describe the normal operation of this component. Second, explicitly identify any hidden inefficiencies, latency spikes, repeated retries, or unusual patterns.'
 ) AS performance_analysis
FROM
 `bq_logs_demo.spark_logs_structured`
GROUP BY
 Component
ORDER BY
 log_count DESC; 

You can see in these results that AI.AGG() successfully acknowledges the "operating normally" messages while surfacing the critical diagnostic insights: 

The query results pane showing the insights generated by AI.AGG() over the logs dataset. 

Extracting categories from unstructured text and image data 

Now, let’s look at some more use cases that demonstrate the flexibility of AI.AGG() , using one of BigQuery’s public datasets, cymbal_pets , a fictional pet supply shop. It includes a catalog of products carried by the store, with unstructured data like product names, descriptions, and images, making it a great example of the power of AI functions for handling unstructured data. 

For example, let’s say you want to categorize the products in the dataset. The first hurdle in this case isn't applying labels to your products, but discovering what categories exist across the product catalog. With AI.AGG() , you can ask the model to analyze the raw product names and descriptions to identify the overarching categories for you. 

Loading... -- Identify categories of products from product name and description
SELECT
 AI.AGG(
 ('Product: ', product_name, ' - Description: ', description),
 'What are the major categories of these products?' 
 ) AS category_description
FROM
 `bigquery-public-data.cymbal_pets.products`; 

This query returns a simple plaintext list of categories: 

The plaintext result of categories determined by AI.AGG() over our products dataset. 

This initial query is great for discovery, but a simple plaintext string isn't enough to build a reliable, automated data pipeline. To actually tag your data, you need to instruct AI.AGG() to return a structured format, like a JSON array. Then, you can use the structured categories as a parameter within another AI function, AI.CLASSIFY() , to actually label each product with its category. 

The following SQL statement completes each of these steps in one script: 

Loading... -- 1. Declare a variable to hold the array of categories
DECLARE generated_labels ARRAY<STRING>;

-- 2. Create a dataset to store the results
CREATE SCHEMA IF NOT EXISTS categorized_cymbal_pets;

-- 3. Generate the JSON string with AI.AGG and extract it into the variable
SET generated_labels = (
 SELECT 
 JSON_VALUE_ARRAY(
 AI.AGG(
 ('Product: ', product_name, ' - Description: ', description), 
 'Identify the major product categories. Return exactly one valid JSON array of strings. Do not include markdown code blocks, backticks, or conversational text.'
 )
 )
 FROM `bigquery-public-data.cymbal_pets.products`
);

-- 4. Feed the variable directly into AI.CLASSIFY
CREATE OR REPLACE TABLE `categorized_cymbal_pets.categorized_products` AS (
SELECT 
 product_name,
 description,
 AI.CLASSIFY(
 ('Product: ', product_name, ' - Description: ', description),
 generated_labels
 ) AS assigned_category
FROM 
 `bigquery-public-data.cymbal_pets.products`
); 

You can now view the resulting table, which includes an assigned_category : 

A preview of the categorized_products table which includes the new assigned_category column created by AI.AGG() and AI.CLASSIFY(). 

If you look closely at the intermediate table, you'll notice the structured categories changed slightly from the initial plaintext results. This happens for two reasons: First, LLMs are nondeterministic, meaning that they don't always give the exact same response to the same prompt. Second, the prompt was adjusted to accommodate the new output structure. 

The returned product categories are structured as JSON by AI.AGG() as requested as part of the prompt. 

With the table now labeled by category, you can group by the categories to do traditional SQL aggregation, or use AI.AGG() to consider each category separately. 

For example, the following query fetches traditional metrics (like row counts) right alongside a synthesized AI summary of what those specific grouped products have in common: 

Loading... -- Synthesize insights grouped by our newly assigned categories
SELECT 
 assigned_category,
 COUNT(*) AS item_count,
 AI.AGG(
 ('Product: ', product_name, ' - Description: ', description),
 'Write a concise, one-sentence summary describing the common characteristics or purpose of the products in this category.'
 ) AS category_summary
FROM 
 `categorized_cymbal_pets.categorized_products`
GROUP BY 
 assigned_category
ORDER BY 
 item_count DESC; 

Query results showing analyzing with AI.AGG() alongside more traditional SQL methods. 

Unstructured data isn't limited to text. Because AI.AGG() natively supports multimodal inputs, you can return aggregated insights directly from image files. 

The cymbal_pets Google Cloud project also contains a Cloud Storage bucket full of product photos. By creating an external object table, you can securely pass the image URIs directly into AI.AGG() and ask the model to summarize the visual content of the entire collection. 

Loading... -- Summarize content of images in the object table
SELECT
 AI.AGG(
 STRUCT(OBJ.GET_ACCESS_URL(ref, 'r')),
 'What are the major categories of these images?'
 ) AS category_description
FROM
 `bigquery-public-data.cymbal_pets.product_images`; 

Query results showing AI.AGG() surface product categories by analyzing the product images located in Google Cloud Storage. 

How AI.AGG() works and best practices 

To use AI.AGG() effectively in your own environment, it helps to understand how it processes data behind the scenes. Here’s what you need to know about context windows, error handling, and optimizing your pipelines. 

1. Context windows and multi-level aggregation 
LLMs have a specific context window and can have a hard time handling massive amounts of input. AI.AGG() solves this problem by automatically dividing your input rows into batches, aggregating those batches, and then aggregating the results of those batches into a final answer. This means you don’t have to worry about manually managing the context window when passing in large numbers of rows. Note that AI.AGG() won’t split up a row of data across batches, so make sure that each individual row is smaller than the context window, to avoid the row being skipped. Many smaller rows will give AI.AGG() more flexibility with how to batch each row. 

2. Token usage with multi-level aggregation 
Because AI.AGG() uses a multi-level aggregation structure, the total input tokens sent to the model may be higher than the raw tokens in your starting table (depending on how many rounds of aggregation are required). As a best practice, always reduce the number of input tokens by using LIMIT or pre-filtering your data upstream before passing it to AI.AGG() . 

3. Specifying your model endpoint 
If you don’t specify a model endpoint, AI.AGG() will default to a recent model. However, for production pipelines, you often want explicit control: 

Short-form names: You can use a short-form endpoint (e.g., gemini-2.5-flash ), in which case AI.AGG() will use that model in the query execution region: 

Loading... AI.AGG(
 input_data,
 instructions => 'Your instructions here.',
 endpoint => 'gemini-2.5-flash' 
) 

Fully-qualified names: If the query execution region doesn’t support your desired model, or you prefer to use a global or multiregional endpoint, provide the fully qualified model name: 

Loading... AI.AGG(
 input_data,
 instructions => 'Your instructions here.',
 endpoint => 'https://aiplatform.googleapis.com/v1/projects/[YOUR_PROJECT]/locations/global/publishers/google/models/gemini-3.5-flash'
) 

4. Input and output modalities 

Inputs: AI.AGG() supports text (via strings or references to text files) and image data. It also supports arrays of these types, though you should refer to the known issues documentation for edge cases regarding arrays of images. 

Outputs: The function will always return a string . While you can prompt the model in your instructions to format the output as JSON or Markdown, keep in mind that the database engine does not strictly enforce this. Multimodal output (e.g., generating an image) is not currently supported. 

5. Treatment of NULL s 
AI.AGG() automatically skips NULL input rows without processing them. However, you must be careful when passing structured data. Like other BigQuery AI functions, AI.AGG() concatenates STRUCT fields similarly to the standard CONCAT() function. This means if even one field within your STRUCT is NULL , the entire row is treated as NULL and will be skipped. 

Let's revisit our first categorization query. What if several rows of our products table are missing their description ? Because of the NULL concatenation rule, those rows would be silently dropped from the analysis entirely. Here is how we can use IFNULL() to provide a fallback string, guaranteeing that every product is taken into account even if its description is blank: 

Loading... -- Identify categories of products from product name and (optional) description
SELECT
 AI.AGG(
 ('Product: ', product_name, ' - Description: ', IFNULL(description, 'No description provided')),
 'What are the major categories of these products?' 
 ) AS category_description
FROM
 `bigquery-public-data.cymbal_pets.products`; 

6. Error handling 
If AI.AGG() receives invalid input, or encounters an error during LLM processing, it will attempt to provide partial results. Rows containing invalid input or which were rejected by the LLM model will not be considered in the final results. 

You can review exactly how many rows failed to process by checking your BigQuery job statistics, exactly as you would for scalar managed AI functions like AI.IF() . 

information showing an example of Gen AI function error details. 

Give it a try! 

These are just a few examples of the ways AI.AGG() can help analyze unstructured data. The AI.AGG() function is in preview in BigQuery now, so it’s available to all BigQuery users. Try it out on your own use cases! 

You may also be interested in checking out BigQuery's other managed AI functions , AI.CLASSIFY() , AI.IF() , and AI.SCORE() , as well as general-purpose functions like AI.GENERATE() . We look forward to seeing what you build with them. 

Posted in 
Data Analytics 

AI & Machine Learning 

BigQuery 

Related articles 

Data Analytics 
Scaling Network Analysis for Fraud Prevention with BigQuery Graph 

By Remy Pereira • 4-minute read 

Data Analytics 
Boost BigQuery with Python: Managed Python UDFs now generally available 

By Sandeep Karmarkar • 4-minute read 

Inside Google Cloud 
From AI potential to agentic reality: Driving the UK’s next chapter 

By Maureen Costello • 8-minute read 

AI & Machine Learning 
How Siemens "slices the elephant," advancing agentic workflows for industrial software development 

By Anant Nawalgaria • 5-minute read 

Footer Links 

Follow us 

Google Cloud 

Google Cloud Products 

Privacy 

Terms 

Cookies management controls 

Help 

Language ‪English‬ ‪Deutsch‬ ‪Français‬ ‪한국어‬ ‪日本語‬
