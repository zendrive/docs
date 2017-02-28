Webhook Notifications API
-------------------------

Zendrive provides the ability to specify a `Webhook <http://en.wikipedia.org/wiki/Webhook>`_ where you can receive notifications of interesting events and alerts from Zendrive. The Webhook URL can be specified under **Settings** in your account, after you login to Zendrive.

The Webhook URL provided must be a HTTPS URL as the notifications contain private information about your fleet. Notifications are sent as a POST request to the Webhook URL with a data block containing a json string. The json data contains the following fields.

.. csv-table::
    :header: "Field", "Description"
    :widths: 15, 50

    "version", "Specifies the version of the notification API being sent. Currently, the value of this field is 1. As the notification API evolves, this field will be incremented."
    "type", "The type of this notification as a string. Your application can use this to handle different types of notifications as needed. New notification types may be added incrementally in the API without a bump in the version."
    "...", "Additional fields present are notification type specific. They are described along with the description of the different notification types."


Retries
"""""""
Zendrive will retry notifications to the Webhook in case of all HTTP Errors, Connection Timeouts, SSL Errors, TooManyRedirectErrors. The interval between consecutive retrials increases from 2 min to 15 mins in an exponential fashion, gets capped at 15 mins, is expired after 1 day is elapsed since the first retry.

While it may happen rarely, it is possible that your Webhook receive duplicates of the same notification from Zendrive. Your Webhook handler should take care to handle this correctly as required.

Trip Scored Notification
^^^^^^^^^^^^^^^^^^^^^^^^

This notification is sent by Zendrive when a trip uploaded by your application is completely uploaded and scored by the Zendrive backend. Your application can now query the Zendrive API for :ref:`trip-score-label` for this trip. The notification data block contains the following fields.

+---------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------+
| Request Parameter         | Description                                                                                                                                            |
+===========================+========================================================================================================================================================+
| version                   | Currently, the value of this field is always 1.                                                                                                        |
+---------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------+
| type                      | type for this notification is **TRIP_SCORED**.                                                                                                         |
+---------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------+
| driver_id                 | Id of the driver whose trip was just scored.                                                                                                           |
+---------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------+
| trip_id                   | Id of the trip which was just scored.                                                                                                                  |
+---------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------+
| data                      | A dictionary with the following details about the trip. This is available only to applications who are set up to receive trip details via webhook.     |
|                           | Contact support@zendrive.com to set this up for your application. Else this field will be an empty dictionary {}                                       |
|                           |                                                                                                                                                        |
|                           | - info : Recorded information about the trip [ Total kilometers driven, Drive time etc ].                                                              |
|                           | - simple_path: A coarse GPS trail of the trip. Useful for visualization of the trip path.                                                              |
|                           | - score: Driving behavior scores about the trip [ control, focused etc ].                                                                              |
|                           | - speed_profile: The speed profile of the trip as a tuple (Driver's speed in MPH, Timestamp in ms, Speed limit on the road segment).                   |
|                           | - events: Events detected by Zendrive during the trip. Events like speeding, hard braking, phone use etc are returned.                                 |
+---------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------+

Sample Response
"""""""""""""""

.. sourcecode:: bash

   curl -X POST -H "Content-Type: application/json" -d
  '{"version":1,"type":"TRIP_SCORED","driver_id":"10101672903689391","trip_id": 1416227804134,"data": {}}' https://webhook'

   curl -X POST -H "Content-Type: application/json" -d
  '{"version":1,"type":"TRIP_SCORED","driver_id":"10101672903689391","trip_id": 1416227804134,"data": {"info": {"start_time": 1416227804134, "end_time": 1416227805000, "trip_type": "drive", "drive_time_hours": "01:02", "distance_km": 2.1, "session_id": "701f6868e7e4", "tracking_id": "56250c0f1adf6054dab4f3ed"}, "score": {"cautious_score": 88, "fuel_efficiency_score": -1, "control_score": 88, "focused_score": 90, "zendrive_score": 89}, "events": [{"event_type": "HardBrake", "start_time": 1416227804136, "end_time": 1416227804560, "latitude_start": 72.12345, "longitude_start": 11:1234, "latitude_end": 72.12354, "longitude_end": 11.1235}], "simple_path": [{"latitude": 72.12345, "longitude": 11.1234, 'time_millis': 1416227804134, "timestamp": "2016-01-26T14:59:43+05:30"}], "speed_profile": [[30.12, 1416227804134, 55]]}}' https://webhook
