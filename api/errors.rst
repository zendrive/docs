Errors
------
Zendrive API uses conventional HTTP response codes to indicate success or failure of an API request. Response codes in the 2xx range indicate success, codes in the 4xx range indicate an error that resulted from the provided information (e.g. a required parameter was missing etc.) and codes in the 5xx range indicate an error with Zendrive's servers. The error response JSON may have following attributes.

.. csv-table::
    :widths: 15, 50

    "**error**", A human readable message giving more details about the error.
