List Endpoints
--------------

Several Zendrive API end points return a list of resources like drivers or trips. Such lists of resources can be large depending on your fleet size (number of drivers or number of trips) and time window of the query. The API provides some common parameters to process responses from these List endpoints. All of them are optional.

.. _pagination-label:

Pagination
^^^^^^^^^^
The list API methods share a common structure for pagination. Zendrive uses cursor-based pagination, using the parameters ``offset`` and ``limit``.

.. csv-table::
    :header: "Query Parameter", "Default", "Description"
    :widths: 15, 15, 50

    "offset", "0", "A cursor for use in pagination. ``offset`` is the index of object that defines your place in the list. For instance, if you make a list request and receive first 20 objects, your subsequent call should include ``offset=20`` in order to fetch the next page of the result list. The list API responses also contain ``next_offset`` parameter which can be used to fetch next page. When there are no more elements left in the list ``next_offset`` is missing from the response."
    "limit", "10", "A limit on the number of objects to be returned. Limit can range between 1 and 50."

.. _sorting-label:

Sorting
^^^^^^^
The list API methods also allow sorting the list by different numerical attributes in both ascending and descending order. This can be done by the ``order_by`` and ``order_type`` query parameters.


.. csv-table::
    :header: "Query Parameter", "Default", "Description"
    :widths: 15, 15, 50

    "order_by", "none", "Specifies the numerical attribute by which to sort the list of resources in the response. This is supported for the zendrive score and the various event_rating types in the **driving_behavior** section of the response (e.g: **zendrive_score**, **overspeeding_rating**, **phone_use_rating**) and
    numerical attributes in the **info** section of the response (e.g: **distance_km**, **duration_seconds**). **start_time** is also a valid field. By default, lists in responses are unsorted."
    "order_type", "asc", "Specifies the sort order. **asc** for sorting in ascending order. **desc** for sorting in descending order."
