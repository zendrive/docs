Webhook Notifications API
-------------------------

Zendrive provides the ability to specify a `Webhook <http://en.wikipedia.org/wiki/Webhook>`_ where you can receive notifications of interesting events and alerts from Zendrive. The Webhook URL can be specified under **Settings** in your account, after you login to Zendrive.

The Webhook URL provided must be a HTTPS URL as the notifications contain private information about your fleet. Notifications are sent as a POST request to the Webhook URL with a data block containing a json string. The json data contains the following fields.

.. csv-table::
    :header: "Field", "Description"
    :widths: 15, 50

    "version", "Specifies the version of the notification API being sent. Currently, the value of this field is 3. As the notification API evolves, this field will be incremented."
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
| version                   | 	Currently, the value of this field is 3.                                                                                                       		 |
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

Sample Notification
"""""""""""""""""""

.. sourcecode:: bash

   curl -X POST -H "Content-Type: application/json" -d
   '{
    	"version": 3,
    	"type": "TRIP_SCORED",
    	"driver_id": "10101672903689391",
    	"trip_id": "1416227804134",
    	"data": {}
    }' https://webhook'

   curl -X POST -H "Content-Type: application/json" -d
   '{
    	"version": 3,
    	"type": "TRIP_SCORED",
    	"driver_id": "10101672903689391",
    	"trip_id": "1416227804134",
    	"data": {
			"info": {
				"insurance_period": "NA",
				"trip_max_speed_kmph": 84.16792964453629,
				"distance_km": 11.023,
				"end_time": "2017-09-07T15:28:32-04:00",
				"tracking_id": "37",
				"duration_seconds": 8769,
				"start_time": "2017-09-07T15:08:10-04:00",
				"session_id": "1590149475"
			},
			"driving_behavior": {
				"score": {
					"zendrive_score": 90
				},
				"event_rating": {
					"hard_brake_rating": 3,
					"phone_use_rating": 4,
					"rapid_acceleration_rating": 3,
					"overspeeding_rating": 5
				}
			},
			"simple_path": [],
			"trip_id": "1504811290303",
			"events": [{
					"event_type": 3,
					"event_type_name": "OVERSPEEDING",
					"latitude_end": 40.9145881796243,
					"longitude_end": -74.2668195549313,
					"longitude_start": -74.2656172626412,
					"latitude_start": 40.9139556857207,
					"average_driver_speed_kmph": 49.535568691545045,
					"max_driver_speed_kmph": 78.98654033076772,
					"end_time": "2017-09-07T15:16:45-04:00",
					"posted_speed_limit_kmph": 32.186854250516596,
					"start_time": "2017-09-07T15:15:56-04:00"
			}]
    	}
    }' https://webhook
