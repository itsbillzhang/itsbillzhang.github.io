
<div class="featured">
<a href="{{ page.url }}">
<img src="{{site.url}}/images/12 - kids at the church.png" />
</a>
</div>

Class trip to London, kids tripping over the stairs of the Canterbury Cathedral. 

## Context

One of the scariest things I've ever done was to move across the world for 8 months, immersing myself in a completely different language and culture, to teach in a public school for 8 months. There were days, especially at the beginning, where I wondered what I was doing, but over the 8 months, I made amazing memories I'll keep for the rest of my life. Here are moments from my classroom.


<p class="centered-text">
<img class="centered" src="{{site.url}}/images/1 - Powerpoint.png" />
  <figcaption> Preparing to introduce myself to different groups of students with a powerpoint about Life in Canada! </figcaption>.
</p>
Preparing!

<p class="centered-text">
<img class="centered" src="{{site.url}}/images/3 - homework.png" />
</p>



<p class="centered-text">
<img class="centered" src="{{site.url}}/images/backend_companies.png" />
</p>

Kaitlyn comes back from her lunch break and gets an alert from her email

<p class="centered-text">
<img class="centered" src="{{site.url}}/images/email_alert.png" />
</p>

Concerned, Kaitlyn notifies her boss Riley. Riley logs into ThoughtSpot and is able to see historical invoice data, and play around with the linked cloud data immediately and explore any discrepancies. 

<p class="centered-text">
<img class="centered" src="{{site.url}}/images/thoughtspot1.png" />
</p>

## High Level Data Flow

<p class="centered-text">
<img class="centered" src="{{site.url}}/images/high_level_data_flow.png" />
</p>

At a quick glance

- Use of cloud: Instead of running things locally, I opted for cloud hosting of databases and compute resources. An infrastructure that could easily scale to processing one or 1000 invoices was critical.
- To store new and unprocessed invoices in picture form, **Azure Blob storage** was used.
- To store historical and processed invoices, **Azure SQL server** was used. All key information is captured in SQL tables.
- **Databricks** is invoked to process the data we stored. The picture of our invoice goes through a few stages, with the important ones listed here:

- Optical Character Recognition: Important parts of the invoice, such as company name, bank account, phone number, etc, are picked up from the picture, using **Azure Computer Vision** API.
- Comparison with the historical invoices with **rule based flagging.**
- If there is a match of company name, but a critical piece of information is wrong, an **email alert** is sent out.

- Data is connected to **ThoughtSpot**, allowing for easy exploration and visualization of the invoice data, which is useful for identifying trends and recurrent discrepancies

## Deep Dive

### Data Creation

First, I needed a place to put the data. I opted for storage options located on the cloud for durability and scalability. Azure blob storage container and Azure SQL Server were both initiated for picture storage and information storage respectively,

<p class="centered-text">
<img class="centered" src="{{site.url}}/images/azure_db.png" />
</p>

As I don’t have any access to past invoice data, I needed to create some for simulation purposes. For the image of the invoice I was to analyze, I went online and found an invoice template, and changed it to my liking. This was then placed in our blob storage.

A preexisting summary of past invoices, the companies associated with those invoices, and information about those companies was also required, as we needed information to compare the invoice to. As I did not have past invoice data, I artificially created this, using SQL to make up past data on 10 vendors, and inserted that into SQL Server. 

Once that was in place, it’s time to process this in Azure Databricks.

### Data Processing

To begin working with Databricks, one must first start a cluster. This acts as the computational power behind the code we are running. Once I had this set up, I could use this cluster to support my code. It was nice to know that this was scalable - If the amount of data I had to process every increased or different demands for the data popped up, the cluster supplied could change.

<p class="centered-text">
<img class="centered" src="{{site.url}}/images/cluster_summary.png" />
</p>

The invoice flagging job on Databricks has 7 tasks in total. They were:

One: Read the photo of the invoice from Azure blob.

This required mounting blob storage to Databricks. This essentially puts a link between our workspace and Azure, so that any files on Azure blob will have a local reference.

<p class="centered-text">
<img class="centered" src="{{site.url}}/images/azure_mount.png" />
</p>

Here, I specify the location of my storage on the cloud, and allow the photos to be accessed by mnt/invoice_images. 

<p class="centered-text">
<img class="centered" src="{{site.url}}/images/onetwo.png" />
</p>

Two: Running Optical Character Recognition (OCR) on the image. 
- First On Azure, I initiated Azure Computer Vision, and used the generated endpoint and key to enable its cognitive services API in Databricks. 
- The output of whatever was in our blob storage would be passed onto OCR, which turns the image into a block of text which can be parsed.

<p class="centered-text">
<img class="centered" src="{{site.url}}/images/invoice_extracted_unprocessed_text.png" />
</p>

<p class="centered-text">
<img class="centered" src="{{site.url}}/images/twothree.png" />
</p>

Three: Feature generation.

Now that we have the invoice in text format, we need to pinpoint the specific pieces of data we need, because this one piece of text won’t help us. I chose relatively simple features, but more advanced feature can be produced that would allow us to abstract even more from the invoices, such as number of items bought, total amount, and item description.

The feature generation was done through a very crude regex matching. 

<p class="centered-text">
<img class="centered" src="{{site.url}}/images/regex.png" />
</p>

Which allowed the following features to be extracted from our example invoice:

<p class="centered-text">
<img class="centered" src="{{site.url}}/images/regex_features.png" />
</p>
<p class="centered-text">
<img class="centered" src="{{site.url}}/images/threefourfive.png" />
</p>

Four: Read the data of past invoices from Azure SQL.

<p class="centered-text">
<img class="centered" src="{{site.url}}/images/backend_companies_full.png" />
</p>

In a similar vein to how blob was mounted, we pass the credentials of our cloud SQL so that past vendor data can be viewed. In practice, this data would be updated every time a new invoice is uploaded.
    
Five: Flag for validity.

A rule based flagging system based on pattern matching to past invoices is invoked here. If there is a match on name, but the destination of payment has changed, the system voices a concerned. While this is very simple, the infrastructure allows more complex flagging. For example, if a Machine Learning based flagging model were to be put in place, this pipeline easily allows that to happen. We can swap this job with a more complicated flagging system. 
    
<p class="centered-text">
<img class="centered" src="{{site.url}}/images/validity_flagging.png" />
</p>
    

<p class="centered-text">
<img class="centered" src="{{site.url}}/images/fivesixseven.png" />
</p>

Six: Alert sending.

If there is an invoice that is deemed invalid, then an email is automatically sent, alerting of an potentially improper invoice.

Seven: Uploading of data.

A record of the validation attempt and its outcome is recorded and uploaded to our SQL database, allowing recordkeeping. 

After all the data has been processed, we move to our final step:

## ThoughtSpot Data Visualization

To enable ThoughtSpot, a firewall entry needed to be approved in our Azure server. After that was done, any invoice information can be accessed and easily visualized, without diving to low level code. By giving ThoughtSpot access to invoice data, graphs can quickly be made.

For example, here is the total values of approved invoices and unapproved invoices. 

<p class="centered-text">
<img class="centered" src="{{site.url}}/images/thoughtspot2.png" />
</p>

## Conclusions and Next Steps

As someone who had limited exposure to the above technologies, it was both a frustrating and rewarding time learning about all that - from the firewall and credential rules for Azure servers needed for connections to clusters to DBFS for passing around data within a job.

Looking at this project as a whole, there are a couple of things can make this better:

- Validation goes beyond a single customer. What if the database of invoices we compare new invoices against is a generalized one rather than customer-specific? This would allow us to work with a much larger dataset.
- Better features generated. As an example, we can have information not only on components of the invoice, but the individual items of the invoices and recognize their categories. This would allow us to also have more advanced rules on what is on the invoice. For e.g, if food is able to be identified on an invoice, than food say on an Expense Reimbursement Invoice can be flagged if the amount is too large, such as >$100 for what is supposed to be a single person meal expense.
- If multiple people are using the software within a single company, each invoice upload could be linked to the particular person. This would combat against abnormal invoices from a single person.
- Identification of different fraud causes that were not covered here. For e.g, duplication fraud. If two invoices that are identical are given a week apart, are they the same invoice that might be paid twice?
