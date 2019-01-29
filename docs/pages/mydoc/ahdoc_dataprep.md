---
title: Data Preparation for Pencil
permalink: ahdoc_dataprep.html
keywords: getting_started
last_updated: December 21 2018
tags:
- getting_started
summary: If you want to understand more detail about preparing datasets for upload,
  or if you want to merge and upload complex datasets,  these guidelines will be of
  help.
sidebar: ahdoc_sidebar
folder: ahdoc
---

<p style=".readingtime"> X Minute Read </style></p>

Datasets can be uploaded either by direct database connection, or via manual dataload of <a href="#" data-toggle="tooltip" data-original-title="{{site.data.glossary.csv}}">CSV</a> files. All of the data formatting requirements described here are applicable to both methods.

For a manually-loaded dataset, <a href="#" data-toggle="tooltip" data-original-title="{{site.data.glossary.pencil}}">Pencil</a> requires input via a single data table (also called a 'flat file') in CSV format.

{% include note.html content="If you're planning on uploading a dataset through a flat file, make sure it has all the columns required to meet the proposed use case." %}

## Four Required Types of Column
Pencil works best with data summarising events over time, where changes are driven by transactions. While the details will vary from case to case, transactional data will typically have:
1. **Date**: a properly-formatted date
2. **Segment** (e.g. department, product type, customer category, etc)
3. **Measure** (e.g. number of sales or units or kilometers)
4. **Unique value/s** (optional columns containing identifiers such as Product ID, Basket ID, Item Type, etc)

The first three of these are mandatory – if you don’t have at least one of each of these types, the dataset will be rejected.   
Data sets usually also have a range of unique identifiers as well.
When datasets are loaded each column is classified as one of these types.
{% include image.html file="column_summary.png"  alt="Column Summary" caption="Column summary showing types of columns uploaded" %}


### File Size
The recommended file size limit for flat-file upload is 2Gb. There is no hard limit on larger file uploads but for very large files, upload times become a factor. If larger file uploads are regularly required then transfer via direct database connection is strongly recommended.
### Data Quality
Good quality data forms the foundation of reliable and accurate insights - the popular saying “garbage in, garbage out” applies to Pencil uploads also!  So for the best results, follow these guidelines.
### Data Integrity
*  In flat file uploads, the delimiter and any quotation marks used should not conflict with data values
*  Header rows / column names must be included
*  Records (i.e. rows) ought not to be duplicated.
Valid Formats, Field Entries and Column Names
*  Date field must be formatted as “YYYY-MM-DD”
*  Numeric fields may not contain thousands-separator, special characters or currency signs (e.g. <comma>, %, #, AUD) which should be removed from numeric fields prior to data load.
*  Consistent values in data (e.g. a single way of spelling company “ABC Pty. Ltd.” as opposed to also having “abc pty ltd”)
*  Do not include columns with significant proportion of missing values (e.g. columns that are more than 90% empty should be omitted.)
*  Columns with single unique attribute should be ommited (e.g. a customer status with only one value for “Active”)
*  Exclude unstructured data (e.g. tags, free form text should be removed. See Unsupported data section for some suggested treatment for different types of data).

### Numeric Columns
Numeric columns are typically the **key performance indicators** (KPIs) in the dataset. Examples include: revenue amount, asset balance amount, transaction amount, number of sales, amount of payments.

However, not all numeric fields are KPIs. For examples, product unit price and customer tenure should be treated as segments as they are characteristics rather than performance measures.

### Unique Identifiers
Anna supports more complicated calculations on unique identifiers -  such as ‘unique number of customers’. For example, instead of number of rows, a business would want to understand how many unique customers they have, which is calculated on-the-fly by Anna.

### Data Completeness
Include all relevant data, both in terms of rows & columns, required by the use case.
For example, a payment dataset should include columns for:
*  payment day (date)
*  amount of payment (measure)
*  payment method (segment) & product name (segment)
*  customer ID (unique identifier)
*  Store ID (unique identifier)

### Generating Top Insights
One of Hyper Anna’s key features is Top Insights. For Top Insights to be generated, in addition to the above attributes, the dataset needs at least one date field with:
* time-scales monthly or less (i.e. monthly, fortnightly, weekly or daily only)
* at least four consecutive months of records.

## Using Natural Language (NL) Descriptors
Users interact with Anna using natural language (NL), so it is recommended that both column names and values ought to be as free of coded values and abbreviations, and be as descriptive as possible, so as to ensure the optimal NL experience to end-users.

Often a source dataset will have:
*  Coded abbreviations such as department code, currency code, country code
*  System-generated product codes
*  Specialised terminology or jargon

To enable natural language, these should be converted to a descriptive name so that they are intuitive for end-users.
{% include tip.html content="Use the column description as a basis for the column name, as it is often more natural-language-friendly than a system name" %}

**Also:**
*  It is highly recommended that column names be less than seven words
*  No special characters in column names (except underscore or space)
*  No number(s) at start of column names.

Should coded column names be present, they need to be converted to their natural language equivalent as per the example below:

{% include image.html file="NL_Headings.png"  alt="Natural Language headings" caption="Converting system-generated headings to Natural Language equivalents" %}

### Special characters
Column values containing special characters (e.g. &, $, #, %) should be converted to include only alphanumeric characters, underscores and spaces.

### Data Dictionary
The Data Dictionary accompanies every data upload. It comprises two worksheets which provide entries describing every column name and data-source, along with a useful summary.  This is uploaded in PDF format and shows what the dataset contains, where it originated, and what the data is about. (Click [here](http://#) to download a copy of the template.)

## Data Aggregation (advanced topic)
In cases where data is required from different data sources, these data sources need to be **aggregated** into a single table, either through appending or merging rows (using a set of unique merging keys).

Aggregated data can be used in Pencil provided the aggregation is consistent across the data set.

Levels of detail for a data set are described in terms of ‘granularity’ where higher granularity means greater level of detail.

When loading aggregated data, columns derived from a higher level of granularity should be removed to avoid data duplication. In other words all the data has to be entered at a consistent level of detail.

### Merging Multiple Data Sources
When a use-case requires multiple core datasets with different levels of granularity, these first need to be aggregated to the same level of granularity (normally to the higher granularity) before performing the join.

The recommended way to merge multiple data sources is as follows:

**Step 1**: Bearing the business use-case in mind, find the core dataset that contains the key numeric measure(s), date(s) and segments, at the level of granularity required by the use-case.

**Step 2**: Convert non-core datasets to the same level of granularity as the core dataset, then left-join with the core  dataset.

{% include note.html content="the number of rows of the core dataset should be the same after joining with non-core datasets if the merging keys are unique" %}

See the following graphic for an example:

{% include image.html file="Combine_Sources.png"  alt="Combining Data Sources" caption="Combining data sources" %}

### Unsupported Data Types and Workarounds
The following data types are currently not supported in Pencil, and should be either transformed or removed prior to uploading:
  * Coded values
  * Detailed addresses
  * Tags
  * Unstructured data

#### Coded Values
As mentioned above, column names or other coded values that are written as abbreviations or acronyms should be converted to the natural language equivalent in simple English.
For example, instead of having country codes in three-letter acronyms, the full country name should be used to ensure a natural language experience.

#### Address Detail
The suggested treatment for address detail is to use separate fields (columns) for suburbs, state and postcode, instead of entering the detail in a single field.

#### Tags
Tags are typically used to associate data records with keywords, where one data record could be associated with multiple keywords (or tags), and the number of keywords may differ across records.

Suggested treatment for tags is to select the most important keywords required by the use case and expand them into multiple columns with each column referring to one particular keyword.

#### Unstructured Data

_Unstructured data_ is information that either does not have a pre-defined format (such as call-centre conversation logs). Unstructured information is typically text-heavy, but may contain data such as dates, numbers, and facts as well. This results in irregularities and ambiguities that make it difficult to understand using traditional programs as compared to data stored in fielded form in databases or annotated (semantically tagged) in documents.

Anna/Pencil is not able to upload unstructured data of this kind.
