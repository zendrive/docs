API Endpoints
-------------

List Driver Groups in a Fleet
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. sourcecode:: bash

   curl https://api.zendrive.com/v3/groups?apikey={ZENDRIVE_ANALYTICS_API_KEY}

.. note:: This call takes no query parameters other than ``apikey``.

.. csv-table::
    :header: "Response field", "Description"
    :widths: 15, 50

    "group_ids", "A collection of all group ids that belong to the fleet. A group id is specified in Zendrive SDK initialization to tag a driver as part of a group."

Sample Response
"""""""""""""""

.. sourcecode:: bash

    {
        "group_ids" : [
            "san francisco",
            "new york",
            "chigaco"
        ]
    }

List Drivers in a Fleet
^^^^^^^^^^^^^^^^^^^^^^^

Lookup active drivers in the given date range. An active driver is one who has at least one recorded trip in the given date range. This endpoint also allows look up of specific drivers using the ids parameter.

.. sourcecode:: bash

   curl https://api.zendrive.com/v3/drivers?apikey={ZENDRIVE_ANALYTICS_API_KEY}

.. note:: All query parameters except ``apikey`` are optional.



+---------------------------+---------------------------------------------------------------------------------------------------------------------------+
| Request Parameter         | Description                                                                                                               |
+===========================+===========================================================================================================================+
| start_date                | Lookup active drivers in this date range. See :ref:`date-range-label` for description                                     |
+---------------------------+---------------------------------------------------------------------------------------------------------------------------+
| end_date                  | Lookup active drivers in this date range. See :ref:`date-range-label` for description                                     |
+---------------------------+---------------------------------------------------------------------------------------------------------------------------+
| fields                    | Comma separated list of fields to lookup. If nothing is specified, all data is returned.                                  |
|                           |                                                                                                                           |
|                           | - info : Returns information about driver [ Total kilometers driven,Drive time etc ]                                      |
|                           | - driving_behavior: Returns driver score and event ratings calculated over the interval specified.                        |
|                           | - daily_driving_behavior: Returns driver score and event ratings calculated for each day during the interval specified.   |
+---------------------------+---------------------------------------------------------------------------------------------------------------------------+
| limit                     | See :ref:`pagination-label` section for description.                                                                      |
+---------------------------+---------------------------------------------------------------------------------------------------------------------------+
| offset                    | See :ref:`pagination-label` section for description.                                                                      |
+---------------------------+---------------------------------------------------------------------------------------------------------------------------+
| ids                       | Comma separated list of driver ids for which data should be returned. Even if these                                       |
|                           | drivers are not active between start_date and end_date the response will contain info and                                 |
|                           | driving_behavior sections for these drivers.                                                                              |
+---------------------------+---------------------------------------------------------------------------------------------------------------------------+
| group_id                  | If specified the list of drivers returned will be restricted to drivers that belong to                                    |
|                           | the specified group. If a list of driver ids is explicitly given as query parameter then                                  |
|                           | group_id parameter has no impact on result set.                                                                           |
+---------------------------+---------------------------------------------------------------------------------------------------------------------------+
| order_by                  | Sort list by score_type or info fields. See :ref:`sorting-label` section for details.                                     |
+---------------------------+---------------------------------------------------------------------------------------------------------------------------+
| order_type                | Sort order. See :ref:`sorting-label` section for details.                                                                 |
+---------------------------+---------------------------------------------------------------------------------------------------------------------------+


.. csv-table::
    :header: "Response field", "Description"
    :widths: 15, 50

    "start_date", "Start date of the request."
    "end_date", "End date of the request."
    "next_offset", "This is the value that should be passed in offset parameter to get the next page of response. If not present in response, it implies that this is the last page."
    "drivers", "Array of drivers."
    "drivers[i].driver_id", "Id of the driver. This is the ID specified when initializing the Zendrive SDK in the mobile application."
    "drivers[i].info", "Various metrics of the driver."
    "drivers[i].info.num_trips", "Number of trips."
    "drivers[i].info.distance_km", "Distance travelled in kilometers."
    "drivers[i].info.duration_seconds", "Total drive time of the driver across all trips in seconds"
    "drivers[i].info.driver_start_date", "The first time we saw data from this driver."
    "drivers[i].info.attributes", "Additional attributes of the driver if it was provided during setup of the Zendrive SDK. The attributes are returned here as a json string. This is **NA** if no attributed were provided."
    "drivers[i].driving_behavior", "Returns driver score and event ratings calculated over the interval specified."
    "drivers[i].driving_behavior.score.zendrive_score", "Zendrive score of this driver at the end of the given date range."
    "drivers[i].driving_behavior.event_rating", "A collection of various events for the driver during the interval specified. Note that each event here is an average of daily event ratings for the driver over the given interval."
    "drivers[i].driving_behavior.event_rating.hard_brake_rating", "The hard brake rating of this driver at the end of the given date range."
    "drivers[i].driving_behavior.event_rating.phone_use_rating", "The phone use rating of this driver at the end of the given date range."
    "drivers[i].driving_behavior.event_rating.rapid_acceleration_rating", "The acceleration rating of this driver at the end of the given date range."
    "drivers[i].driving_behavior.event_rating.hard_turn_rating", "The hard turn rating of this driver at the end of the given date range."
    "drivers[i].driving_behavior.event_rating.overspeeding_rating", "The overspeeding rating of this driver at the end of the given date range."

Sample Response
"""""""""""""""

.. sourcecode:: bash

   {
      "drivers": [{
          "info": {
              "trip_count": 10,
              "device_info": [{
                  "missing_data": ["gyroscope"],
                  "model": "samsung-SM-T567V",
                  "version": "25"
              }],
              "distance_km": 9.041,
              "duration_seconds": 4519.34,
              "attributes": {
                  "phone": "+19999999999",
                  "first_name": "John",
                  "last_name": "User",
                  "email": "user@example.com"
              },
              "driver_start_date": "2018-01-18"
          },
          "driving_behavior": {
              "score": {
                  "zendrive_score": 100
              },
              "event_rating": {
                  "hard_brake_rating": 5,
                  "phone_use_rating": 5,
                  "rapid_acceleration_rating": 5,
                  "hard_turn_rating": 5,
                  "overspeeding_rating": 5
              }
          },
          "driver_id": "adyuv4hd83"
      }],
      "next_offset": 50,
      "start_date": "2018-01-18",
      "end_date": "2018-01-25"
  }



Fleet Scores
^^^^^^^^^^^^

.. sourcecode:: bash

   curl https://api.zendrive.com/v3/score?apikey={ZENDRIVE_ANALYTICS_API_KEY}

.. note:: All query parameters except ``apikey`` are optional.


+---------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------+
| Request Parameter         | Description                                                                                                                                            |
+===========================+========================================================================================================================================================+
| start_date                | Lookup fleet score in this date range. See :ref:`date-range-label` for description                                                                     |
+---------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------+
| end_date                  | Lookup fleet score in this date range. See :ref:`date-range-label` for description                                                                     |
+---------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------+
| fields                    | Comma separated list of fields to lookup. If nothing is specified, info, daily_driving_behavior and driving_behavior are returned.                     |
|                           |                                                                                                                                                        |
|                           | - info : Returns information about the fleet [ Total kilometers driven, Drive time etc ].                                                              |
|                           | - driving_behavior: Returns driver score and event ratings calculated over the interval specified.                                                     |
|                           | - daily_driving_behavior: Returns driver score and event ratings calculated for each day during the interval specified.                                |
+---------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------+
| group_id                  | The fleet score is computed based on data from the specified group_id within the fleet.                                                                |
+---------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------+

.. csv-table::
    :header: "Response field", "Description"
    :widths: 15, 50

    "start_date", "Start date of the request."
    "end_date", "End date of the request."
    "info.distance_km", "Total distance in km logged by all drivers in the fleet or group."
    "info.duration_seconds", "Total drive time over all drivers in fleet or group represented in seconds"
    "info.driver_count", "Number of drivers in fleet or group active during the given date range."
    "info.last_trip_date", "Date since the last recorded trip."
    "info.missing_data_driver_count", "Number of drivers missing one or more data types (usually missing Gyroscope)."
    "driving_behavior", "Returns driver score and event ratings calculated over the interval specified."
    "driving_behavior.score.zendrive_score", "Average Zendrive score across all drivers in the fleet in the given date range."
    "driving_behavior.event_rating", "A collection of various events for the driver during the interval specified. Note that each event here is an average of daily event ratings for the driver over the given interval."
    "driving_behavior.event_rating.hard_brake_rating", "Average hard brake rating across all drivers in the fleet in the given date range."
    "driving_behavior.event_rating.phone_use_rating", "Average phone use rating across all drivers in the fleet in the given date range"
    "driving_behavior.event_rating.rapid_acceleration_rating", "Average acceleration rating across all drivers in the fleet in the given date range."
    "driving_behavior.event_rating.hard_turn_rating", "Average hard turn rating across all drivers in the fleet in the given date range."
    "driving_behavior.event_rating.overspeeding_rating", "Average overspeeding rating across all drivers in the fleet in the given date range."
    "daily_driving_behavior[i]", "Scores and Event Ratings for each day in the date range requested."
    "daily_driving_behavior[i].date", "Date for which scores are provided in this tuple. Date format is YYYY-MM-DD."
    "daily_driving_behavior[i].score", "A collection of various scores for the driver during the interval specified. Note that each score here is an average of daily scores for the driver over the given interval."
    "daily_driving_behavior[i].score.zendrive_score", "Average Zendrive score across all drivers in the fleet on this particular date."
    "daily_driving_behavior[i].event_rating", "A collection of various events for the driver during the interval specified. Note that each event here is an average of daily event ratings for the driver over the given interval."
    "daily_driving_behavior[i].event_rating.hard_brake_rating", "Average hard brake rating across all drivers in the fleet on this particular date."
    "daily_driving_behavior[i].event_rating.phone_use_rating", "Average phone use rating across all drivers in the fleet on this particular date."
    "daily_driving_behavior[i].event_rating.rapid_acceleration_rating", "Average rapid acceleration rating across all drivers in the fleet on this particular date."
    "daily_driving_behavior[i].event_rating.hard_turn_rating", "Average hard turn rating across all drivers in the fleet on this particular date."
    "daily_driving_behavior[i].event_rating.overspeeding_rating", "Average overspeeding rating across all drivers in the fleet on this particular date."

Sample Response
"""""""""""""""

.. sourcecode:: bash


    {
        "info": {
            "duration_seconds": 8400.32,
            "driver_count": 0,
            "distance_km": 0.0,
            "missing_data_driver_count": 0,
            "last_trip_date": "2018-11-03"
        },
        "driving_behavior": {
            "score": {
                "zendrive_score": 80
            },
            "event_rating": {
                "hard_brake_rating": 3,
                "phone_use_rating": 3,
                "rapid_acceleration_rating": 5,
                "hard_turn_rating": 5,
                "overspeeding_rating": 4
            }
        },
        "daily_driving_behavior": [{
            "date": "2017-09-12",
            "score": {
                "zendrive_score": 75
            },
            "event_rating": {
                "hard_brake_rating": 3,
                "phone_use_rating": 3,
                "rapid_acceleration_rating": 3,
                "hard_turn_rating": 3,
                "overspeeding_rating": 3
            }
        },
         { ... }
        ],
        "end_date": "2017-12-11",
        "start_date": "2017-09-12"
    }


Driver Scores
^^^^^^^^^^^^^

.. sourcecode:: bash

   curl https://api.zendrive.com/v3/driver/{driver_id}/score?apikey={ZENDRIVE_ANALYTICS_API_KEY}

.. note:: All query parameters except ``apikey`` are optional.


+---------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------+
| Request Parameter         | Description                                                                                                                                            |
+===========================+========================================================================================================================================================+
| start_date                | Lookup driver score in this date range. See :ref:`date-range-label` for description                                                                    |
+---------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------+
| end_date                  | Lookup driver score in this date range. See :ref:`date-range-label` for description                                                                    |
+---------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------+
| fields                    | Comma separated list of fields to lookup. If nothing is specified, info daily_driving_behavior and driving_behavior are returned.                      |
|                           |                                                                                                                                                        |
|                           | - info : Returns information about driver [ Total kilometers driven, Drive time etc ]                                                                  |
|                           | - driving_behavior: Returns driver score and event ratings calculated over the interval specified.                                                     |
|                           | - daily_driving_behavior: Returns driver score and event ratings calculated for each day during the interval specified.                                |
+---------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------+

.. csv-table::
    :header: "Response field", "Description"
    :widths: 15, 50

    "start_date", "Starting date for driver score returned in response. Same as request parameter if specified, else the start date considered by the API by default."
    "end_date", "End date for driver score returned in response. Same as request parameter if specified, else the start date considered by the API by default."
    "info.distance_km", "Total distance in km logged by the driver during the specified date range."
    "info.trip_count", "Total number of trips logged by the driver during the specified date range."
    "info.driver_start_date", "The date at which data was first logged by this driver."
    "info.attributes", "Additional attributes of the driver if it was provided during setup of the Zendrive SDK. The attributes are provided as a json string. This is **NA** if no attributed were provided."
    "info.duration_seconds", "Total drive time of the driver during the specified date range represented in seconds"
    "info.highway_ratio",  "Indicates the fraction of trips recorded on highways (value lies within 0 & 1)"
    "info.night_driving_fraction", "Indicates the fraction of night (12:00 AM to 4:00 AM local time) driving (value lies within 0 & 1)"
    "info.device_info", "Devices that the driver has used (model name and version number). The missing_data key lists the essential sensors that is missing (like Gyroscope) in the device."
    "driving_behavior", "Returns driver score and event ratings calculated over the interval specified."
    "driving_behavior.score.zendrive_score", "Zendrive score of the driver at the end of the given data range."
    "driving_behavior.event_rating", "A collection of various events for the driver during the interval specified. Note that each event here is an average of daily event ratings for the driver over the given interval."
    "driving_behavior.event_rating.hard_brake_rating", "Average hard brake rating of the driver at the end of the given date range."
    "driving_behavior.event_rating.phone_use_rating", "Average phone use rating of the driver at the end of the given date range."
    "driving_behavior.event_rating.hard_turn_rating", "Average hard turn rating of the driver at the end of the given date range."
    "driving_behavior.event_rating.overspeeding_rating", "Average overspeeding rating of the driver at the end of the given date range."
    "daily_driving_behavior[i]","Scores for each day in the date range requested."
    "daily_driving_behavior[i].date", "Date for which scores are provided in this tuple. Date format is YYYY-MM-DD."
    "daily_driving_behavior[i].score", "A collection of various scores for the driver during the interval specified. Note that each score here is an average of daily scores for the driver over the given interval."
    "daily_driving_behavior[i].score.zendrive_score", "Average Zendrive score of this driver at the end of the given date range."
    "daily_driving_behavior[i].event_rating", "A collection of various events for the driver during the interval specified. Note that each event here is an average of daily event ratings for the driver over the given interval."
    "daily_driving_behavior[i].event_rating.hard_brake_rating", "Average hard brake rating across all drivers in the fleet on this particular date."
    "daily_driving_behavior[i].event_rating.phone_use_rating", "Average phone use rating across all drivers in the fleet on this particular date."
    "daily_driving_behavior[i].event_rating.rapid_acceleration_rating", "Average rapid acceleration rating across all drivers in the fleet on this particular date."
    "daily_driving_behavior[i].event_rating.hard_turn_rating", "Average hard turn rating across all drivers in the fleet on this particular date."
    "daily_driving_behavior[i].event_rating.overspeeding_rating", "Average overspeeding rating across all drivers in the fleet on this particular date."

Sample Response
"""""""""""""""

.. sourcecode:: bash

    {
        "info": {
            "trip_count": 4,
            "night_driving_fraction": 0.0,
            "device_info": [],
            "distance_km": 35.389,
            "duration_seconds": 9468.40,
            "attributes": {
                "first_name": "John",
                "last_name": " User",
                "email": "user@example.com"
            },
            "driver_start_date": "2017-04-25"
        },
        "driving_behavior": {
            "score": {
                "zendrive_score": 79
            },
            "event_rating": {
                "hard_brake_rating": 5,
                "phone_use_rating": 4,
                "rapid_acceleration_rating": 3,
                "hard_turn_rating": 3,
                "overspeeding_rating": 4
            }
        },
        "daily_driving_behavior": [{
            "date": "2017-09-12",
            "score": {
                "zendrive_score": 75
            },
            "event_rating": {
                "hard_brake_rating": 3,
                "phone_use_rating": 3,
                "rapid_acceleration_rating": 3,
                "hard_turn_rating": 3,
                "overspeeding_rating": 3
            }
        }],
        "end_date": "2017-09-06",
        "start_date": "2017-09-01"
    }

List Driver Sessions
^^^^^^^^^^^^^^^^^^^^

.. sourcecode:: bash

   curl https://api.zendrive.com/v3/driver/{driver_id}/sessions?apikey={ZENDRIVE_ANALYTICS_API_KEY}

.. note:: All query parameters except ``apikey`` are optional.

.. csv-table::
    :header: "Request Parameter", "Description"
    :widths: 15, 50

    "start_date", "Lookup driver sessions in this date range.  See :ref:`date-range-label` for description."
    "end_date", "Lookup driver sessions in this date range.  See :ref:`date-range-label` for description."
    "limit", "See :ref:`pagination-label` section for description."
    "offset", "See :ref:`pagination-label` section for description."


.. csv-table::
    :header: "Response Field", "Description"
    :widths: 15, 50

    "start_date", "Start date of the request."
    "end_date", "End date of the request."
    "sessions", "List of session ids."
    "sessions[i].session_id", "The session id provided by the application in the Zendrive SDK."

Sample Response
"""""""""""""""

.. sourcecode:: bash

    {
        "next_offset": 10,
        "start_date": "2014-11-16",
        "end_date": "2014-11-22",
        "sessions": [
            {
                "session_id": "542ebb4ee98f7c2438f6c140bb"
            },
            {
                "session_id": "542ebb4ee98f7c2438f6c140bb"
            },
            { ... }
        ]
    }


List Driver Trips
^^^^^^^^^^^^^^^^^

.. sourcecode:: bash

   curl https://api.zendrive.com/v3/driver/{driver_id}/trips?apikey={ZENDRIVE_ANALYTICS_API_KEY}

.. note:: All query parameters except ``apikey`` are optional.

.. csv-table::
    :header: "Request Parameter", "Description"
    :widths: 15, 50

    "start_date", "Lookup trips from a driver in this date range.  See :ref:`date-range-label` for description."
    "end_date", "Lookup trips from a driver in this date range.  See :ref:`date-range-label` for description."
    "fields", "Comma separated list of fields to lookup. If nothing is specified, all data is returned. ``info`` : Returns information about driver [ Total kilometers driven, Drive time etc ]. ``driving_behavior``: Returns driver score and event ratings."
    "session_ids", "A comma separated list of session ids to filter trips by. Only trips tagged by the given session ids are returned. Sessions are specified in the Zendrive SDK. The mobile application can provide session ids to the SDK to tag trips with."
    "tracking_ids", "A comma separated list of tracking ids to filter trips by. Only trips tagged by the given tracking ids are returned. Tracking ids are specified by the mobile application when recording a trip in manual mode."
    "limit", "See :ref:`pagination-label` section for description."
    "offset", "See :ref:`pagination-label` section for description."
    "order_by", "Sort list by score_type or info fields. See :ref:`sorting-label` section for details."
    "order_type", "Sort order. See :ref:`sorting-label` section for details."

.. csv-table::
    :header: "Response Field", "Description"
    :widths: 15, 50

    "start_date", "Start date of the request."
    "end_date", "End date of the request."
    "trips", "List of trips from the driver in the given date range."
    "trips[i].trip_id", "Unique Id of the trip assigned by Zendrive."
    "trips[i].info.distance_km", "Length of the trip in km."
    "trips[i].info.duration_seconds", "Total duration of the trip in seconds."
    "trips[i].info.trip_max_speed_kmph", "Maximum speed reached during the duration of this trip"
    "trips[i].info.start_time", "Start time of trip in ISO format."
    "trips[i].info.end_time", "End time of trip in ISO format."
    "trips[i].info.tracking_id", "Tracking id of the trip if specified in the Zendrive SDK. This is available only for trips recorded in manual mode of the SDK."
    "trips[i].info.session_id", "Id of the session this trip belongs to. This is available if the session was live in the Zendrive SDK when the trip was recorded."
    "driving_behavior.score.zendrive_score", "Zendrive score of the trip."
    "driving_behavior.event_rating", "A collection of various events for the driver during the interval specified. Note that each event here is an average of daily event ratings for the driver over the given interval."
    "driving_behavior.event_rating.hard_brake_rating", "Hard brake rating of the trip."
    "driving_behavior.event_rating.phone_use_rating", "Phone use of rating"
    "driving_behavior.event_rating.rapid_acceleration_rating", "Rapid acceleration rating of the trip."
    "driving_behavior.event_rating.hard_turn_rating", "Hard turn rating of the trip."
    "driving_behavior.event_rating.overspeeding_rating", "Overspeeding rating of the trip."

Sample Response
"""""""""""""""

.. sourcecode:: bash

  {
        "trips": [{
            "info": {
                "insurance_period": "NA",
                "trip_max_speed_kmph": 96.40792534541973,
                "distance_km": 25.78,
                "end_time": "2017-09-29T13:55:16-04:00",
                "tracking_id": "39",
                "duration_seconds": 400.39,
                "start_time": "2017-09-29T13:16:11-04:00",
                "session_id": "1613190012"
            },
            "driving_behavior": {
                "score": {
                    "zendrive_score": 84
                },
                "event_rating": {
                    "hard_brake_rating": 3,
                    "phone_use_rating": 4,
                    "rapid_acceleration_rating": 4,
                    "hard_turn_rating": 4,
                    "overspeeding_rating": 3
                }
            },
            "trip_id": "1506705371408"
        }],
        "end_date": "2017-09-30",
        "start_date": "2017-09-01"
    }

.. _trip-score-label:

Trip Scores
^^^^^^^^^^^

.. sourcecode:: bash

   curl https://api.zendrive.com/v3/driver/{driver_id}/trip/{trip_id}?apikey={ZENDRIVE_ANALYTICS_API_KEY}

.. note:: All query parameters except ``apikey`` are optional.


+---------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------+
| Request Parameter         | Description                                                                                                                                            |
+===========================+========================================================================================================================================================+
| fields                    | Comma separated list of fields to lookup. If nothing is specified, this defaults to 'info,driving_behavior'.                                           |
|                           |                                                                                                                                                        |
|                           | - info : Returns recorded information about the trip [ Total kilometers driven, Drive time etc ].                                                      |
|                           | - simple_path: Returns a coarse GPS trail of the trip. Useful for visualization of the trip path.                                                      |
|                           | - driving_behavior: Returns driver score and event ratings calculated over the interval specified.                                                     |
|                           | - speed_profile: Returns the speed profile of the trip as a tuple (Driver's speed in MPH, Timestamp in ms, Speed limit on the road segment).           |
|                           | - events: Returns events detected by Zendrive during the trip. Events like OverSpeeding, PhoneUse, AggressiveAcceleration, HardBrake, HardTurn and     |
|                           |   Collision are returned. *PhoneScreenInteraction is currently in beta.*                                                                               |
+---------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------+

.. csv-table::
    :header: "Response Field", "Description"
    :widths: 15, 50

    "trip_max_speed", "Maximum speed reached during the duration of this trip"
    "trip_id", "Unique Id assigned by Zendrive for the trip where the event occurred."
    "info.distance_km", "Distance in km of the trip."
    "info.duration_seconds", "Total duration of the trip represented in seconds"
    "info.start_time ", "Start time of trip in ISO format."
    "info.end_time", "End time of trip in ISO format"
    "info.session_id", "Session id attached the trip if specified in the Zendrive SDK when the trip was recorded."
    "info.insurance_period", "This is valid only for trips recorded by Fairmatic customers using the SDK. This is the Fairmatic insurance period associated with this trip. This will be 'NA' if Fairmatic insurance does not apply to the customer or the trip."
    "simple_path", "An array of latitude, longitude, timestamp tuples representing a simplified path of the trip. The timestamp is in ISO format."
    "driving_behavior", Returns driver score and event ratings calculated over the interval specified."
    "driving_behavior.score.zendrive_score", "Zendrive score of the trip."
    "driving_behavior.event_rating", "A collection of various events for the driver during the interval specified. Note that each event here is an average of daily event ratings for the driver over the given interval."
    "driving_behavior.event_rating.hard_brake_rating", "Hard brake rating of the trip"
    "driving_behavior.event_rating.phone_use_rating", "Phone use rating of the trip"
    "driving_behavior.event_rating.rapid_acceleration_rating", "Acceleration rating of the trip"
    "driving_behavior.event_rating.hard_turn_rating", "Hard turn rating of the trip"
    "driving_behavior.event_rating.overspeeding_rating", "Overspeeding rating of the trip"
    "speed_profile", "An array of tuples containing Driver's speed in MPH, timestamp (Unix timestamp since epoch in milliseconds) and Speed limit on the road segment. The array is in timestamp ascending order."
    "events", "An array containing a list of driving events that happened during the trip. The events are low level details that are reflected in scores."
    "events[i].latitude_start", "Latitude of location where the event started."
    "events[i].longitude_start", "Longitude of location where the event started."
    "events[i].latitude_end", "Latitude of location where the event ended."
    "events[i].longitude_end", "Longitude of location where the event ended."
    "events[i].start_time", "Timestamp of when the event started in ISO format."
    "events[i].end_time", "Timestamp of when the event ended in ISO format."
    "events[i].event_type", "Numeric value associated with event. The possible values are 0 for 'HARD_BRAKE, 1 for RAPID_ACCELERATION, 2 for PHONE_USE,  3 for OVERSPEEDING, 4 for COLLISION, 5 for HARD_TURN, 6 for PHONE_SCREEN_INTERACTION"
    "events[i].event_type_name", "Type of driving event. The possible types are **OVERSPEEDING, PHONE_USE, RAPID_ACCELERATION, HARD_BRAKE, HARD_TURN, PHONE_SCREEN_INTERACTION and COLLISION**."
    "events[i].average_driver_speed_kmph", "Average speed of the driver during the event. This is valid only for OVERSPEEDING event."
    "events[i].max_driver_speed_kmph", "Maximum speed of the driver during the event. This is valid only for OVERSPEEDING event"
    "events[i].posted_speed_limit_kmph", "Posted legal speed limit where the event occurred. This is valid only for OVERSPEEDING event"

Sample Response
"""""""""""""""

.. sourcecode:: bash

    {
        "info": {
            "insurance_period": "NA",
            "trip_max_speed_kmph": 84.16792964453629,
            "distance_km": 11.023,
            "end_time": "2017-09-07T15:28:32-04:00",
            "tracking_id": "37",
            "duration_seconds": 987.45,
            "start_time": "2017-09-07T15:08:10-04:00",
            "session_id": "1590149475"
        },
        "driving_behavior": {
            "score": {
                "zendrive_score": 88
            },
            "event_rating": {
                "hard_brake_rating": 3,
                "phone_use_rating": 4,
                "rapid_acceleration_rating": 4,
                "hard_turn_rating": 4,
                "overspeeding_rating": 5
            }
        },
        "simple_path": [{
                "latitude": 40.7048046,
                "timestamp": "2018-01-25T16:28:35.318000-05:00",
                "longitude": -73.7980828,
                "time_millis": 1516915715318
            },{
                "latitude": 40.7048046,
                "timestamp": "2018-01-25T16:29:10.805000-05:00",
                "longitude": -73.7980314,
                "time_millis": 1516915750805
            }
        ],
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
        }, {
            "latitude_end": 40.9182284027719,
            "latitude_start": 40.9182284027719,
            "longitude_end": -74.2960023321903,
            "event_type": 0,
            "event_type_name": "HARD_BRAKE",
            "start_time": "2017-09-07T15:20:28-04:00",
            "longitude_start": -74.2960023321903,
            "end_time": "2017-09-07T15:20:28-04:00"
        }, {
            "latitude_end": 40.9182284027719,
            "latitude_start": 40.9182284027719,
            "longitude_end": -74.2960023321903,
            "event_type": 6,
            "event_type_name": "PHONE_SCREEN_INTERACTION",
            "start_time": "2017-09-07T15:20:28-04:00",
            "longitude_start": -74.2960023321903,
            "end_time": "2017-09-07T15:20:28-04:00"
        }, {
            "latitude_start": 39.7209858723,
            "event_type": 2,
            "event_type_name": "PHONE_USE",
            "start_time": "2015-03-10T18:31:58-06:00",
            "longitude_end": -104.932609689,
            "longitude_start": -104.931542739,
            "latitude_end": 39.7209808562,
            "end_time": "2015-03-10T18:32:07-06:00"
        }],
        "speed_profile":[
            [
                0.6710820000000001,
                1516915715318,
                "NA"
            ],
            [
                1.0961006,
                1516915716798,
                "NA"
            ],
            [
                0.6710820000000001,
                1516915718789,
                "NA"
            ]
        ]
    }


.. _trip-delete-label:

Delete Trip
^^^^^^^^^^^

This API endpoint should be used if you want to ignore an existing trip and all its data from future API responses and driver score computations. Once deleted a trip has no impact on the driver's scores any more and hence deletion will lead to change in driver scores.

Note that a trip MUST already exist in Zendrive system for it to be successfully deleted (if a trip and its data is not yet uploaded to server it cannot be deleted). This API endpoint typically should be called after existence of trip is verified by a GET call or after a Webhook callback has been invoked.

.. sourcecode:: bash

   curl -X DELETE https://api.zendrive.com/v3/driver/{driver_id}/trip/{trip_id}?apikey={ZENDRIVE_ANALYTICS_API_KEY}

.. note:: ``apikey`` is the only query parameter.


Response in case of Success
"""""""""""""""""""""""""""

.. sourcecode:: bash

    {
        "success": true
    }


Sample Response in case of Failure
""""""""""""""""""""""""""""""""""

.. sourcecode:: bash

    {
        "error": "trip_id 1426131047984 is not valid"
    }
