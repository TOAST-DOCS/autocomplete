## Search > Autocomplete > Overview

- Autocomplete is provided for the input of search words on search window.
    - Enter data for Autocomplete by using index REST API.
    - Get results for Autocomplete by using Autocomplete REST API.

### Developing Autocomplete Service

Service is configured as below:

![img](http://static.toastoven.net/prod_autocomplete/block_diagrm-en-20200304.png)

Development Process

1. Create Services

    - The Autocomplete service is created.

2. Indexing

    - Create JSON data according to the Autocomplete input format.
    - Use REST API to enter created JSON data for Autocomplete.  

3. Autocomplete

    - Configure the front page with the result of Autocomplete REST API.
