# HTRC IPython Integration

## Tasks

* IPython on Docker API
* IPython on Docker API Client
* HTRC Python API


## Implementation Plan

* Docker container definition for IPython environment
* REST API which wraps Docker API
  - Create IPython environment for a user
  - Create notebook in user's IPython
  - Returns the real URL of users's IPython notebook
  - API methods for managing notebooks (delete, etc.).
