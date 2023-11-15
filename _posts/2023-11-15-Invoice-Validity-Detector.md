---
published: true
---

<div class="featured">
<a href="{{ page.url }}">
<img src="{{site.url}}/images/high_level_data_flow.png" />
</a>
</div>

Code found [here](https://github.com/itsbillzhang/InvoiceValidity/blob/main/Entire%20Project.py).

## Motivation

While investigating the suite of features offered by procurement companies, I was intrigued by invoice processing, partly piqued by recalling a startling fraud case: a man who ingeniously deceived industry titans Google and Facebook out of $100 million with [counterfeit invoices](https://www.npr.org/2019/03/25/706715377/man-pleads-guilty-to-phishing-scheme-that-fleeced-facebook-google-of-100-million). This incident underscores a paradox; the larger the organization, the greater the susceptibility to coordination lapses and, consequently, the higher the risk of such oversights. It’s a compelling reminder of the vital need for robust and intelligent invoice verification mechanisms, even—or especially—for the most technologically advanced firms.

So I tried to build my own.

## Table of Contents:

1. Example Usage
2. High Level Data Flow
3. Deep Dive
4. Conclusions and Next Steps

Example Usage

Kaitlyn does HR for an international pet adoption agency called Claws to Paws. She just got hired because recently Claws to Paws has been expanding rapidly. She gets this invoice for a company they’ve worked for in the past: TechSolvers. It looks like this:

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/ed97681d-6be6-4e25-9e44-9fb8a03346cc/59b09c24-2feb-4349-8bdc-21b51deb7b9f/Untitled.png)

Instead of manually recording this invoice with whatever software from the 2000s, she uses an automatically invoice processing software. She uploaded a picture of this invoice, and leaves for lunch.

Behind the scenes, important information from the picture is automatically extracted using computer vision.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/ed97681d-6be6-4e25-9e44-9fb8a03346cc/5e83366c-6f91-4916-9d44-92173f394935/Untitled.png)

More so, this isn’t the first time Claws to Paws have dealt with TechSolvers. The software keeps a list of the information gathered on the vendors from the previously uploaded invoices and compares the new invoice to that list: 

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/ed97681d-6be6-4e25-9e44-9fb8a03346cc/11fe23e3-2fb2-4740-9df6-132a1c1cf605/Untitled.png)

Kaitlyn comes back from her lunch break and gets an alert from her email

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/ed97681d-6be6-4e25-9e44-9fb8a03346cc/be406565-1c4a-4eb9-aa6d-6adb05bddc88/Untitled.png)

Concerned, Kaitlyn notifies her boss Riley. Riley logs into ThoughtSpot and is able to see historical invoice data, and play around with the linked cloud data immediately and explore any discrepancies. 

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/ed97681d-6be6-4e25-9e44-9fb8a03346cc/a2f0920c-dcc7-4257-8ec3-7ae2be81b912/Untitled.png)

******************High Level Data Flow******************

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/ed97681d-6be6-4e25-9e44-9fb8a03346cc/7c0bb919-8cdd-4f79-ba46-ac1aeb906d97/Untitled.png)

At a quick glance

- Use of cloud: Instead of running things locally, I opted for cloud hosting of databases and compute resources. An infrastructure that could easily scale to processing one or 1000 invoices was critical.
- To store new and unprocessed invoices in picture form, **Azure Blob storage** was used.
- To store historical and processed invoices, **Azure SQL server** was used. All key information is captured in SQL tables.
- **Databricks** is invoked to process the data we stored. The picture of our invoice goes through a few stages, with the important ones listed here:
    - Optical Character Recognition: Important parts of the invoice, such as company name, bank account, phone number, etc, are picked up from the picture, using **Azure Computer Vision** API.
    - Comparison with the historical invoices with **********************rule based flagging.**********************
    - If there is a match of company name, but a critical piece of information is wrong, an **email alert** is sent out.
- Data is connected to ************************ThoughtSpot************************, allowing for easy exploration and visualization of the invoice data, which is useful for identifying trends and recurrent discrepancies

**Deep Dive**

**************************Data Creation**************************

First, I needed a place to put the data. I opted for storage options located on the cloud for durability and scalability. Azure blob storage container and Azure SQL Server were both initiated for picture storage and information storage respectively,

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/ed97681d-6be6-4e25-9e44-9fb8a03346cc/22bfe34b-8377-4b1d-bb47-4b19ae6ea4fc/Untitled.png)

As I don’t have any access to past invoice data, I needed to create some for simulation purposes. For the image of the invoice I was to analyze, I went online and found an invoice template, and changed it to my liking. This was then placed in our blob storage.

A preexisting summary of past invoices, the companies associated with those invoices, and information about those companies was also required, as we needed information to compare the invoice to. As I did not have past invoice data, I artificially created this, using SQL to make up past data on 10 vendors, and inserted that into SQL Server. 

Once that was in place, it’s time to process this in Azure Databricks.

******************************Data Processing******************************

To begin working with Databricks, one must first start a cluster. This acts as the computational power behind the code we are running. Once I had this set up, I could use this cluster to support my code. It was nice to know that this was scalable - If the amount of data I had to process every increased or different demands for the data popped up, the cluster supplied could change.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/ed97681d-6be6-4e25-9e44-9fb8a03346cc/1a93fff5-8886-4ef7-9339-a3faac63c882/Untitled.png)

The invoice flagging job on Databricks has 7 tasks in total. They were:

1. Read the photo of the invoice from Azure blob.
    1. This required mounting blob storage to Databricks. This essentially puts a link between our workspace and Azure, so that any files on Azure blob will have a local reference.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/ed97681d-6be6-4e25-9e44-9fb8a03346cc/c435e67b-eca4-4beb-9bf7-072ca43d783c/Untitled.png)

Here, I specify the location of my storage on the cloud, and allow the photos to be accessed by mnt/invoice_images. 

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/ed97681d-6be6-4e25-9e44-9fb8a03346cc/53854ba6-2d65-4c0e-a86a-4ea786aab408/Untitled.png)

1. Running Optical Character Recognition (OCR) on the image. 
    1. On Azure, I initiated Azure Computer Vision, and used the generated endpoint and key to enable its cognitive services API in Databricks. 
    2. The output of whatever was in our blob storage would be passed onto OCR, which turns the image into a block of text which can be parsed.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/ed97681d-6be6-4e25-9e44-9fb8a03346cc/73ed79e5-2560-4915-8020-bdc79311a0e1/Untitled.png)

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/ed97681d-6be6-4e25-9e44-9fb8a03346cc/d7df8a32-5b51-4b61-986b-f2044e48fad0/Untitled.png)

1. Feature generation.

Now that we have the invoice in text format, we need to pinpoint the specific pieces of data we need, because this one piece of text won’t help us. I chose relatively simple features, but more advanced feature can be produced that would allow us to abstract even more from the invoices, such as number of items bought, total amount, and item description.

The feature generation was done through a very crude regex matching. 

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/ed97681d-6be6-4e25-9e44-9fb8a03346cc/e48035ec-a150-4f0a-8767-0bdf889086ce/Untitled.png)

Which allowed the following features to be extracted from our example invoice:

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/ed97681d-6be6-4e25-9e44-9fb8a03346cc/6cafd4b0-6832-44e7-8b50-39b02faff5e3/Untitled.png)

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/ed97681d-6be6-4e25-9e44-9fb8a03346cc/0359b35f-4777-4dfd-93a9-9cd956be4780/Untitled.png)

1. Read the data of past invoices from Azure SQL.
    1. In a similar vein to how blob was mounted, we pass the credentials of our cloud SQL so that past vendor data can be viewed.
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/ed97681d-6be6-4e25-9e44-9fb8a03346cc/6f7e6fd5-2e75-4ccc-a2f6-1967029ce0cb/Untitled.png)
    
    In practice, this data would be updated every time a new invoice is uploaded.
    
2. Flag for validity.
    1. A rule based flagging system based on pattern matching to past invoices is invoked here. If there is a match on name, but the destination of payment has changed, the system voices a concerned.
    2. While this is very simple, the infrastructure allows more complex flagging. For example, if a Machine Learning based flagging model were to be put in place, this pipeline easily allows that to happen. We can swap this job with a more complicated flagging system. 
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/ed97681d-6be6-4e25-9e44-9fb8a03346cc/d57a2cd8-4076-4473-862b-ffc70d96d309/Untitled.png)
    

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/ed97681d-6be6-4e25-9e44-9fb8a03346cc/4a3a64fc-dda9-4d02-8749-50822a1ba3a8/Untitled.png)

1. Alert sending.

If there is an invoice that is deemed invalid, then an email is automatically sent, alerting of an potentially improper invoice.

1. Uploading of data.

A record of the validation attempt and its outcome is recorded and uploaded to our SQL database, allowing recordkeeping. 

After all the data has been processed, we move to our final step:

**ThoughtSpot Data Visualization**

To enable ThoughtSpot, a firewall entry needed to be approved in our Azure server. After that was done, any invoice information can be accessed and easily visualized, without diving to low level code. By giving ThoughtSpot access to invoice data, graphs can quickly be made.

For example, here is the total values of approved invoices and unapproved invoices. 

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/ed97681d-6be6-4e25-9e44-9fb8a03346cc/dbc46660-225a-4eb3-a22a-23bfb02da8bf/Untitled.png)

****************************************************Conclusions and Next Steps****************************************************

As someone who had limited exposure to the above technologies, it was both a frustrating and rewarding time learning about all that - from the firewall and credential rules for Azure servers needed for connections to clusters to DBFS for passing around data within a job.

Looking at this project as a whole, there are a couple of things can make this better:

- Validation goes beyond a single customer. What if the database of invoices we compare new invoices against is a generalized one rather than customer-specific? This would allow us to work with a much larger dataset.
- If multiple people are using the software within a single company, each invoice upload could be linked to the particular person. This would combat against abnormal invoices from a single person.
- Better features generated.
    - For e.g, tagging the things bought on the invoices with categories. For e.g, if food is able to be identified on an invoice, than food say on an Expense Reimbursement Invoice ****can be flagged if the amount is too large, such as >$100 for what is supposed to be a single person meal expense.
    - Allows for more complicated rules.
- Identification of different fraud causes that were not covered here.
    - For e.g, duplication fraud. If two invoices that are identical are given a week apart, are they the same invoice that might be paid twice?
## A New Post

Enter text in [Markdown](http://daringfireball.net/projects/markdown/). Use the toolbar above, or click the **?** button for formatting help.
