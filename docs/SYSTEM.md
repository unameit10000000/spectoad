# System

An overview of how the SpecToad system is going to work

## Development stack
### Backend
- ***Language***: Python
- ***Framework:*** Fastapi
- ***Scheduling:*** Apscheduler
### Frontend
- To be decided

## Specs tracking

### Idea
The goal is to fetch a webpage's source and detect changes by comparing its hash to the stored hash in the database (if any):
- If the new hash differs from the stored one or no hash exists, the page is new or has been updated.
- This triggers the spec extraction process, after which the updated hash is stored

The extractions process is as follows:
- Parse the webpage
- Strip away as many code elements (html, css, js) as possible
- Start ai extraction
- Get a response back, ideally formatted in json
- Validate the output
- If the output can't be parsed trigger an error
- On error, restart ai extraction process, adding any errors that might have occured
- On success, store both the updated data and the page hash in the database

### Potential Challenges and solutions
#### Dynamic Content & Noise

- ***Challenge***: Many modern platforms (like X) serve highly dynamic pages with frequently changing elements (timestamps, tracking parameters, ads, etc.).
Hashing the entire page source might detect changes that aren't meaningful.
- ***Solution***: Consider stripping non-essential elements (e.g., script tags, tracking parameters) before hashing.

#### Fetching Strategy

- ***Challenge***: Pages like X may block or rate-limit automated requests (e.g., through bot detection or CAPTCHAs).
- ***Solution:*** Might need proxies, session cookies, or APIs (if available) to fetch pages reliably.