Webhook Notifications API
-------------------------

List Driver Groups in a Fleet
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Zendrive provides the ability to specify a `Webhook<http://en.wikipedia.org/wiki/Webhook>` where you can receive notifications of interesting events and alerts from Zendrive. The Webhook URL can be specified under **Settings** in your account, after you login to Zendrive.

The Webhook URL provided must be a HTTPS URL as the notifications contain private information about your fleet. Notifications are sent as a POST request to the Webhook URL with a data block containing a json string. The json data contains the following fields.

.. csv-table::
    :header: "Field", "Description"
    :widths: 15, 50

    "version", "Specifies the version of the notification API being sent. Currently, the value of this field is 1. As the notification API evolves, this field will be incremented."
    "type", "The type of this notification as a string. Your application can use this to handle different types of notifications as needed. New notification types may be added incrementally in the API without a bump in the version."
    "...", "Additional fields present are notification type specific. They are described along with the description of the different notification types."


Retries
"""""""
Zendrive will retry notifications to the Webhook if it receives a Server Error from your Webhook handler i.e HTTP response codes 500-599. Notifications will not be retried for response codes 300-499. Notifications will be retried every 2 minutes.

While it may happen rarely, it is possible that your Webhook receive duplicates of the same notification from Zendrive. Your Webhook handler should take care to handle this correctly as required.

Trip Scored Notification
^^^^^^^^^^^^^^^^^^^^^^^^

This notification is sent by Zendrive when a trip uploaded by your application is completely uploaded and scored by the Zendrive backend. Your application can now query the Zendrive API for :ref:`trip-score-label` for this trip. The notification data block contains the following fields.

.. csv-table::
    :header: "Field", "Description"
    :widths: 15, 50

    "version", "Currently, the value of this field is always 1."
    "type", "type for this notification is **TRIP_SCORED**"
    "driver_id", "Id of the driver whose trip was just scored."
    "trip_id", "Id of the trip which was just scored."

Sample Response
"""""""""""""""

.. sourcecode:: bash

   curl -X POST -H "Content-Type: application/json" -d
   '{"version":1,"type":"TRIP_SCORED","driver_id":"10101672903689391","trip_id": 1416227804134}' https://webhook'
