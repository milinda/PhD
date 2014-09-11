# Structure and Organisation of HTRC Source Code

## Main Components

1. Portal - Front End
2. Data API - Front End for Corpus
3. Registry API - Front End for Persistent Repository for User Content
4. Agent - Job Manager
5. Solr Proxy - Search Front End
6. Identity and Registry Server - Identity Provider and User Content Storage
7. Black Light - Front End for Solr and Allows Creation of User Content
8. Bookworm - Front End

## Proposed Structure

Below is the minimal set of git repositories required. Main arguments for below structure are:

* Community related code should go to a single repo.
* Libraries which we will use across HTRC platform should go to a single repo.
* All the front facing components should get its own repo.
* Internal service like Agent, Data API should go to a single repo.

1. HTRC-Community: Samples, UnCamp Code, etc. comes under this.
2. Deprecated: All the old code (currently unused) goes here
3. HTRC-Commons: Reusable Libraries for HTRC Platform. One good example is OAuth2 Servelet Filter.
4. HTRC-Portal
5. HTRC-BlackLight
6. HTRC-Bookworm
7. HTRC-DataAPI
8. HTRC-SolrProxy
9. HTRC-Foundation-Services: Services, APIs internal to HTRC. Agent, Registry API goes here.
10. HTRC-ToolsAndUtilities: Internal tools like log-analyzer will reside here.
