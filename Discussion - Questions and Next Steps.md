# Discussion - Questions and Next Steps
This section has some thoughts on future work, improvements and next steps.  Please feel free to [PR](https://github.com/neo4j-partners/hands-on-lab-neo4j-and-azure-ml/pulls) your ideas and suggestions.

## Lab 0 - Signup for Azure
IT departments have a way of significantly restricting Azure access.  To work around this, most of our lab attendees create their own accounts where they're the root user and have broad access.  However, the result is different from what a user experiences in a real environment.  The goal of this lab was to give you hands on end to end experience.  Given that, this seems to be the best way available.

## Lab 1 - Deploy Neo4j
We deployed an IaaS listing for Neo4j.  That uses an ARM template internally which has a variety of pre-requisites.  Neo4j is currently working on a click to deploy version of our managed service for Azure called Aura.  Once that is available, we'll probably transition this lab content to that service, simplifying setup.

## Lab 2 - Connect to Neo4j
We connected over HTTP.  We are working on improving the self signed cert experience for deployment on IaaS.  We'd also like to use Let's Encrypt or something similar to get a proper cert.  Using Aura avoids this issue entirely.

## Lab 3 - Moving Data
We used LOAD CSV to pull data in.  That is one of many ways.  Neo4j [Data Loader](https://data-importer.neo4j.io/) was recently released.  However it doesn't support compound keys on relationships, so we weren't able to use it.
 
You can use some of these connectors from Neo4j to get data in:
- [Spark Connector](https://neo4j.com/docs/spark/current/)
- [Kafka Connector](https://neo4j.com/labs/kafka/4.0/kafka-connect/)

Neo4j Graph Data Science also supports PyArrow which can make data transfers in the GDS much faster.

## Lab 4 - Exploring Data
This section of the lab could be greatly expanded.

## Lab 5 - AzureML
The run time for our Auto ML job is about 30 minutes. For MLFlow Model deployment and Endpoint creation, it can take from 10-15 minutes.

## Lab 6 - Graph Data Science
With a novel data set combined with a novel approach to machine learning, there's enough material here for numerous business applications or academic papers.  Some areas that might be interesting to explore in the future follow...

The data set isn't normalized.  Between these large asset managers, it's quite likely they own a significant portion of the stock outstanding for certain issues.  So rather than measuring shares or value, a more powerful feature might be percentage of float outstanding.

That then leads to a question about what other data could be used to enrich this dataset.  You wouldn't have to go far to enrich it with data from other forms.  For instance, Form 4, consists of filings for high level executives and directors of companies.  We didn't use that data for this lab as it's not particularly connected, so the graphs were disparate.  However, if it were used in combination with the Form 13 data, it might give some pretty interesting graphs.

Other more traditional data could be used to enrich this dataset as well.  Shares outstanding was noted above.  There might well be interesting interactions between dates of filings like a 10-K or 10-Q and changes in asset manager holdings.

That then brings us to the supervised learning problem we explored today.  We looked at what we could compute from this dataset and decided to predict change in holdings over time.  There are many other things we could try to predict.  One obvious thing would be for a given asset manager, trying to predict what securities they will buy in the future based on the current holdings.  If you're in the broker dealer space, it would be pretty easy to introspect holdings and build a recommendation engine off of it.  Of course, one question there is whether you'd want to recommend similar holdings or something diffferent to diversify?

The projection we used consisted only of the nodes.  We could use node properties as well.  GDS currently supports on float valued properties.  But, we have both shares and value as fields we could have used there.  It also would have been easy enough to convert an identifier like CUSIP to a float but that probably wouldn't have much predictive value in the projection.

Regarding the embedding, that is one approach to creating features.  We also could have explored other algorithms like Nearest Neighbor to generate community features.

Some work on tuning the embedding would improve accuracy.  It is interesting that, even without tuning, the embedding provides more valuable features than either reportCalendarOrQuarter or cusip.

## Lab 7 - Cleanup
The cleanup was a largely manual task.  

## Next Steps
We hope you enjoyed these labs.  If you have any questions, feel free to reach out directly to any of us.  We'd love the opportunity to explore and support your use cases.