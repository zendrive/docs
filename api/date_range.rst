.. _date-range-label:

Date Range
----------

All endpoints allow lookup of data in a specific date range. The date range can be specified using the ``start_date`` and ``end_date`` query parameters. Specifying a date range is optional. If a date range is not specified. API endpoints return data for the last one week.

.. csv-table::
    :header: "Query Parameter", "Description"
    :widths: 15, 50

    "start_date", "Lookup data from active drivers between start_date and end_date [ both inclusive ]. Date format is YYYY-MM-DD. Time zone is in UTC."
    "end_date", "Lookup data from active drivers between start_date and end_date [ both inclusive ]. Date format is YYYY-MM-DD. Time zone is in UTC."
