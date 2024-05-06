Hello,

I'm working on improving the response time of fetching datasets in the population UI. Previously, native queries were used, but I've switched to Hibernate queries with join fetch to optimize performance. However, there hasn't been a significant improvement, with only a 10-second reduction.

To address this, I'm considering a different approach:

When calling getdatasets, I'll only include necessary fields in the UI and send the attribute count as the number of columns instead of all attributes.
This change might impact the getdatasets call during namespace creation. I'll update getdatasetsById in the UI code after fetching datasets from the getdatasets call.
After making changes to namespace removal, I'll update all getdatasets calls accordingly.
