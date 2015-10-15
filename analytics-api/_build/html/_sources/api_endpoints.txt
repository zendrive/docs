API Endpoints
-------------

List Driver Groups in a Fleet
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. sourcecode:: bash

   curl https://api.zendrive.com/v2/groups?apikey={ZENDRIVE_ANALYTICS_API_KEY}

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

   curl https://api.zendrive.com/v2/drivers?apikey={ZENDRIVE_ANALYTICS_API_KEY}

.. note:: All query parameters except ``apikey`` are optional.



+---------------------------+-------------------------------------------------------------------------------------------+
| Request Parameter         | Description                                                                               |
+===========================+===========================================================================================+
| start_date                | Lookup active drivers in this date range. See :ref:`date-range-label` for description     |
+---------------------------+-------------------------------------------------------------------------------------------+
| end_date                  | Lookup active drivers in this date range. See :ref:`date-range-label` for description     |
+---------------------------+-------------------------------------------------------------------------------------------+
| fields                    | Comma separated list of fields to lookup. If nothing is specified, all data is returned.  |
|                           |                                                                                           |
|                           | - info : Returns information about driver [ Total kilometers driven,Drive time etc ]      |
|                           | - score: Returns driving behavior scores [ cautious, focused, control etc]                |
+---------------------------+-------------------------------------------------------------------------------------------+
| limit                     | See :ref:`pagination-label` section for description.                                      |
+---------------------------+-------------------------------------------------------------------------------------------+
| offset                    | See :ref:`pagination-label` section for description.                                      |
+---------------------------+-------------------------------------------------------------------------------------------+
| ids                       | Comma separated list of driver ids for which data should be returned. Even if these       |
|                           | drivers are not active between start_date and end_date the response will contain info and |
|                           | score sections for these drivers.                                                         |
+---------------------------+-------------------------------------------------------------------------------------------+
| group_id                  | If specified the list of drivers returned will be restricted to drivers that belong to    |
|                           | the specified group. If a list of driver ids is explicitly given as query parameter then  |
|                           | group_id parameter has no impact on result set.                                           |
+---------------------------+-------------------------------------------------------------------------------------------+
| order_by                  | Sort list by score_type or info fields. See :ref:`sorting-label` section for details.     |
+---------------------------+-------------------------------------------------------------------------------------------+
| order_type                | Sort order. See :ref:`sorting-label` section for details.                                 |
+---------------------------+-------------------------------------------------------------------------------------------+


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
    "drivers[i].info.drive_time_hours", "Total drive time of the driver across all trips in HH:MM format."
    "drivers[i].info.driver_start_date", "The first time we saw data from this driver."
    "drivers[i].info.attributes", "Additional attributes of the driver if it was provided during setup of the Zendrive SDK. The attributes are returned here as a json string. This is **NA** if no attributed were provided."
    "drivers[i].rank", "The rank of the driver within the fleet or group (if group_id was specified) based on the driver's Zendrive score."
    "drivers[i].score", "A collection of various scores for the driver during the interval specified. Note that each score here is an average of daily scores for the driver over the given interval."
    "drivers[i].score.cautious_score", "The Cautious score of this driver at the end of the given date range."
    "drivers[i].score.control_score", "The Control Score of this driver at the end of the given date range."
    "drivers[i].score.focused_score", "The Focused Score of this driver at the end of the given date range."
    "drivers[i].score.zendrive_score", "Zendrive score of this driver at the end of the given date range."
    "drivers[i].recommendation", "A human friendly textual recommendation for the driver to improve his/her Zendrive score."

Sample Response
"""""""""""""""

.. sourcecode:: bash

    {
        "end_date": "2014-11-23",
        "start_date": "2014-11-17"
        "drivers": [
            {
                "info": {
                    "num_trips": 15,
                    "drive_time_hours": "03:35",
                    "distance_km": "167.0",
                    "driver_start_date": "2014-10-01",
                    "attributes": "NA"
                },
                "driver_id": "10101672903689391",
                "rank": "6/8",
                "score": {
                    "zendrive_score": 48,
                    "control_score": 78,
                    "cautious_score": 68,
                    "focused_score": 73
                },
                "recommendation": "Phone use is distracting your performance."
            },
            {
                "info": {
                    "num_trips": 8,
                    "drive_time_hours": "00:42",
                    "distance_km": "20.0",
                    "driver_start_date": "2014-10-31",
                    "attributes": "NA"
                },
                "driver_id": "1023232",
                "rank": "3/8",
                "score": {
                    "zendrive_score": 60,
                    "control_score": 86,
                    "cautious_score": 77,
                    "focused_score": 80
                },
                "recommendation": "Keep your hands off the phone while driving and avoid over-speeding."
            },
        ],
    }


Fleet Scores
^^^^^^^^^^^^

.. sourcecode:: bash

   curl https://api.zendrive.com/v2/score?apikey={ZENDRIVE_ANALYTICS_API_KEY}

.. note:: All query parameters except ``apikey`` are optional.


+---------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------+
| Request Parameter         | Description                                                                                                                                            |
+===========================+========================================================================================================================================================+
| start_date                | Lookup fleet score in this date range. See :ref:`date-range-label` for description                                                                     |
+---------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------+
| end_date                  | Lookup fleet score in this date range. See :ref:`date-range-label` for description                                                                     |
+---------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------+
| fields                    | Comma separated list of fields to lookup. If nothing is specified, info and score are returned.                                                        |
|                           |                                                                                                                                                        |
|                           | - info : Returns information about the fleet [ Total kilometers driven, Drive time etc ].                                                              |
|                           | - score: Returns driving behavior scores [ cautious, focused, control etc] averaged over all trips.                                                    |
|                           | - score_statistics: Returns daily driving behavior scores [ cautious, focused, control etc] along with distributions over hour of day, week and driver.|
|                           | - daily_scores: Returns scores [ cautious, focused, control etc] for all the days in the given date range                                              |
+---------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------+
| group_id                  | The fleet score is computed based on data from the specified group_id within the fleet.                                                                |
+---------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------+

.. csv-table::
    :header: "Response field", "Description"
    :widths: 15, 50

    "start_date", "Start date of the request."
    "end_date", "End date of the request."
    "info.distance_km", "Total distance in km logged by all drivers in the fleet or group."
    "info.drive_time_hours", "Total drive time over all drivers in fleet or group represented in HH:MM format."
    "info.drivers", "Number of drivers in fleet or group active during the given date range."
    "score.focused_score", "Average Focused score across all drivers in the fleet in the given date range."
    "score.control_score", "Average Control score across all drivers in the fleet in the given date range."
    "info.drivers", "Number of drivers in fleet or group active during the given date range."
    "score.cautious_score","Average Cautious score across all drivers in the fleet in the given date range."
    "score.zendrive_score","Average Zendrive score across all drivers in the fleet in the given date range."
    ".daily_scores[i]","Scores for each day in the date range requested."
    "score_statistics.daily_scores[i].date", "Date for which scores are provided in this tuple. Date format is YYYY-MM-DD."
    "score_statistics.daily_scores[i].focused_score", "Average Focused score across all drivers in the fleet on this particular date."
    "score_statistics.daily_scores[i].control_score", "Average Control score across all drivers in the fleet on this particular date."
    "score_statistics.daily_scores[i].cautious_score", "Average Cautious score across all drivers in the fleet on this particular date."
    "score_statistics.daily_scores[i].zendrive_score", "Average Zendrive score across all drivers in the fleet on this particular date."
    "score_statistics.distributions.driver_score_distribution","The relative frequency distribution of a score over all active drivers in the fleet or group in the date range specified. This is available for each score type. Each distribution is an array of 100 values representing the relative frequency for scores from 1 to 100."
    "score_statistics.distributions.day_of_week_distribution", "The distribution of a score type grouped by each day of the week of the trips in a fleet in the given date range. This is available for each score type. Each distribution is a dictionary where the key represents the day of the week (Sunday(0) to Saturday(6))."
    "score_statistics.distributions.hour_of_day_distribution", "The distribution of a score type grouped by each hour of day of trips in a fleet in the given date range. This is available for each score type. Each distribution is a dictionary where the key represents the hour of the day from 0 to 23."

Sample Response
"""""""""""""""

.. sourcecode:: bash

    {
        "info": {
            "drive_time_hours": "100:45",
            "drivers": 12,
            "distance_km": "1694.1"
        },
        "score": {
            "zendrive_score": 75,
            "cautious_score": 71,
            "control_score": 77,
            "focused_score": 80
        },
        "score_statistics": {
            "distributions": {
                "driver_score_distribution": {
                    "zendrive_score": [0.0, 0.13, 0.0, ......... ],
                    ...
                },
                "day_of_week_distribution": { ... },
                "hour_of_day_distribution": { ... }
            },
            "daily_scores": [
                {
                    "zendrive_score": 50,
                    "date": "2014-11-01",
                    "control_score": 78,
                    "cautious_score": 71,
                    "focused_score": 81
                },
                {
                    "zendrive_score": 39,
                    "date": "2014-11-02",
                    "control_score": 91,
                    "cautious_score": 52,
                    "focused_score": 59
                },
                { ... }
            ]
        },
        "start_date": "2014-11-01",
        "end_date": "2014-11-07"
    }


Global Score Distribution
^^^^^^^^^^^^^^^^^^^^^^^^^

.. sourcecode:: bash

   curl https://api.zendrive.com/v2/global_score?apikey={ZENDRIVE_ANALYTICS_API_KEY}

.. note:: This request takes no parameters.

.. csv-table::
    :header: "Response field", "Description"
    :widths: 15, 50

    "distribution", "The relative frequency distribution of a score over the global population. This is available for each score type. Each distribution is an array of 100 values representing the relative frequency of scores from 1 to 100."

Sample Response
"""""""""""""""

.. sourcecode:: bash

    {
        "score": {
            "distributions": {
                "zendrive_score": [0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, ......... ],
                "control_score": [0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, ......],
                "cautious_score": [0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, ......]
                "focused_score": [0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, ......],
            }
        }
    }


Driver Scores
^^^^^^^^^^^^^

.. sourcecode:: bash

   curl https://api.zendrive.com/v2/driver/{driver_id}/score?apikey={ZENDRIVE_ANALYTICS_API_KEY}&fields=info,score,score_statistics

.. note:: All query parameters except ``apikey`` are optional.


+---------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------+
| Request Parameter         | Description                                                                                                                                            |
+===========================+========================================================================================================================================================+
| start_date                | Lookup driver score in this date range. See :ref:`date-range-label` for description                                                                    |
+---------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------+
| end_date                  | Lookup driver score in this date range. See :ref:`date-range-label` for description                                                                    |
+---------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------+
| fields                    | Comma separated list of fields to lookup. If nothing is specified, info and score are returned.                                                        |
|                           |                                                                                                                                                        |
|                           | - info : Returns information about driver [ Total kilometers driven, Drive time etc ]                                                                  |
|                           | - score: Returns latest driving behavior scores [ cautious, focused, control etc]                                                                      |
|                           | - score_statistics: Returns daily driving behavior scores [ cautious, focused, control etc] along with hourly and weekly distributions                 |
+---------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------+

.. csv-table::
    :header: "Response field", "Description"
    :widths: 15, 50

    "start_date", "Starting date for driver score returned in response. Same as request parameter if specified, else the start date considered by the API by default."
    "end_date", "End date for driver score returned in response. Same as request parameter if specified, else the start date considered by the API by default."
    "info.distance_km", "Total distance in km logged by the driver during the specified date range."
    "info.num_trips", "Total number of trips logged by the driver during the specified date range."
    "info.driver_start_date", "The date at which data was first logged by this driver."
    "info.attributes", "Additional attributes of the driver if it was provided during setup of the Zendrive SDK. The attributes are provided as a json string. This is **NA** if no attributed were provided."
    "info.drive_time_hours", " Total drive time over of the driver during the specified date range represented in HH:MM format."
    "score.focused_score", "Focused score of the driver at the end of the given data range."
    "score.control_score", "Control score of the driver at the end of the given date range."
    "score.cautious_score", "Cautious score of the driver at the end of the given data range."
    "score.zendrive_score", "Zendrive score of the driver at the end of the given data range."
    "score_statistics.focused_score", "Focused score of the driver at the end of the given data range coupled with a difference from the same score at the start of the date range."
    "score_statistics.control_score", "Control score of the driver at the end of the data range coupled with a difference from the same score at the start of the date range."
    "score_statistics.cautious_score", "Cautious score of the driver at the end of the given data range coupled with a difference from the same score at the start of the date range."
    "score_statistics.zendrive_score", "Zendrive score of the driver at the end of the given data range coupled with a difference from the same score at the start of the date range."
    "score_statistics.distributions.hour_of_day_distribution", "The distribution of a score type grouped by each hour of the day of each trip in the given date range. This is available for each score type. Each distribution is a dictionary where the key represents the hour of the day from 0 to 23."
    "score.distributions.day_of_week_distribution", "The distribution of a score type grouped by each day of the week of each trip in the given date range. This is available for each score type. Each distribution is a dictionary where the key represents the day of the week (Sunday(0) to Saturday(6))."
    "daily_scores", "Daily scores for the driver. This score is based on trips logged by the driver on each day in the given date range.This is returned as a list with an element for each day where trips were logged by the driver in the given date range. All score types are computed."

Sample Response
"""""""""""""""

.. sourcecode:: bash

    {
        "info": {
            "drive_time_hours": "23:48",
            "num_trips": 10,
            "distance_km": "54.8",
            "driver_start_date": "2014-10-01",
            "attributes": "NA"
        },
        "score": {
            "zendrive_score": 57,
            "control_score": 88,
            "cautious_score": 70,
            "focused_score": 80
        },
        "score_statistics"": {
            "zendrive_score": {
                "score": 57,
                "delta": 0
            },
            "control_score": {
                "score": 88,
                "delta": 0
            },
            "distributions": {
                "hour_of_day_distribution": {
                    "zendrive_score": { ... },
                    "control_score": { ... },
                    "cautious_score": { ... },
                    "focused_score": { ... }
                },
                "day_of_week_distribution": { ... }
            },
            "cautious_score": {
                "score": 70,
                "delta": 0
            },
            "focused_score": {
                "score": 80,
                "delta": 0
            }
        },
        "daily_scores": [
            {
                "zendrive_score": 57,
                "date": "2014-11-04",
                "control_score": 88,
                "cautious_score": 70,
                "focused_score": 80
            },
            { ... }
        ],
        "start_date": "2014-11-01",
        "end_date": "2014-11-07"
    }

List Driver Sessions
^^^^^^^^^^^^^^^^^^^^

.. sourcecode:: bash

   curl https://api.zendrive.com/v2/driver/{driver_id}/sessions?apikey={ZENDRIVE_ANALYTICS_API_KEY}

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

   curl https://api.zendrive.com/v2/driver/{driver_id}/trips?apikey={ZENDRIVE_ANALYTICS_API_KEY}

.. note:: All query parameters except ``apikey`` are optional.

.. csv-table::
    :header: "Request Parameter", "Description"
    :widths: 15, 50

    "start_date", "Lookup trips from a driver in this date range.  See :ref:`date-range-label` for description."
    "end_date", "Lookup trips from a driver in this date range.  See :ref:`date-range-label` for description."
    "fields", "Comma separated list of fields to lookup. If nothing is specified, all data is returned. ``info`` : Returns information about driver [ Total kilometers driven, Drive time etc ]. ``score``: Returns driving behavior scores [ cautious, focused, control etc]"
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
    "trips[i].info.drive_time_hours", "Total duration of the trip in HH:MM format."
    "trips[i].info.start_time", "Start time of trip in ISO format."
    "trips[i].info.end_time", "End time of trip in ISO format."
    "trips[i].info.tracking_id", "Tracking id of the trip if specified in the Zendrive SDK. This is available only for trips recorded in manual mode of the SDK."
    "trips[i].info.session_id", "Id of the session this trip belongs to. This is available if the session was live in the Zendrive SDK when the trip was recorded."
    "trips[i].score.control_score", "Control score of the trip."
    "trips[i].score.cautious_score", "Cautious score of the trip."
    "trips[i].score.focused_score", "Focused score of the trip."
    "trips[i].score.zendrive_score", "Zendrive score of the trip."

Sample Response
"""""""""""""""

.. sourcecode:: bash

    {
        "trips": [
            {
                "info": {
                    "start_time": "2014-11-17 07:36:44-05:00",
                    "session_id": "542ebb4ee98f7c2438f6c140",
                    "distance_km": "13.7",
                    "end_time": "2014-11-17 07:55:24-05:00",
                    "tracking_id": "545e88bdfd3115a27ff5ebb1",
                    "drive_time_hours": "00:18"
                },
                "score": {
                    "zendrive_score": 74,
                    "control_score": 97,
                    "cautious_score": 76,
                    "focused_score": 87
                },
                "trip_id": 1416227804134
            }
        ]
        "end_date": "2014-11-22",
        "start_date": "2014-11-16"
    }

.. _trip-score-label:

Trip Scores
^^^^^^^^^^^

.. sourcecode:: bash

   curl https://api.zendrive.com/v2/driver/{driver_id}/trip/{trip_id}?apikey={ZENDRIVE_ANALYTICS_API_KEY}

.. note:: All query parameters except ``apikey`` are optional.


+---------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------+
| Request Parameter         | Description                                                                                                                                            |
+===========================+========================================================================================================================================================+
| fields                    | Comma separated list of fields to lookup. If nothing is specified, this defaults to "info,score".                                                      |
|                           |                                                                                                                                                        |
|                           | - info : Returns recorded information about the trip [ Total kilometers driven, Drive time etc ].                                                      |
|                           | - simple_path: Returns a coarse GPS trail of the trip. Useful for visualization of the trip path.                                                      |
|                           | - score: Returns driving behavior scores about the trip [ control, focused etc ].                                                                      |
|                           | - speed_profile: Returns the speed profile of the trip as a tuple (Driver's speed in MPH, Timestamp in ms, Speed limit on the road segment).           |
|                           | - events: Returns events detected by Zendrive during the trip. Events like speeding, hard braking, phone use etc are returned.                         |
+---------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------+

.. csv-table::
    :header: "Response Field", "Description"
    :widths: 15, 50

    "info.distance_km", "Distance in km of the trip."
    "info.drive_time_hours", "Total duration of the trip represented in HH:MM format."
    "info.start_time ", "Start time of trip in ISO format."
    "info.end_time", "End time of trip in ISO format"
    "info.session_id", "Session id attached the trip if specified in the Zendrive SDK when the trip was recorded."
    "simple_path", "An array of latitude, longitude, timestamp tuples representing a simplified path of the trip. The timestamp is in ISO format."
    "score.control_score", "Control score of the trip."
    "score.cautious_score", "Cautious score of the trip."
    "score.focus_score", "Focus score of the trip."
    "score.zendrive_score", "Zendrive score of the trip."
    "speed_profile", "An array of tuples containing Driver's speed in MPH, timestamp (Unix timestamp since epoch in milliseconds) and Speed limit on the road segment. The array is in timestamp ascending order."
    "events", "An array containing a list of driving events that happened during the trip. The events are low level details that are reflected in scores. The events can be of a number types like **Speeding, Phone Use, Aggressive Acceleration, Hard Brake**."
    "events[i].latitude_start", "Latitude of location where the event started."
    "events[i].longitude_start", "Longitude of location where the event started."
    "events[i].latitude_end", "Latitude of location where the event ended."
    "events[i].longitude_end Longitude of location where the event ended."
    "events[i].start_time", "Timestamp of when the event started in ISO format."
    "events[i].end_time", "Timestamp of when the event ended in ISO format."
    "events[i].event_type", "Type of driving event."

Sample Response
"""""""""""""""

.. sourcecode:: bash

    {
        "info": {
            "drive_time_hours": "01:17",
            "start_time": "2014-07-29 23:05:09",
            "distance_km": 23,
            "end_time": "2014-07-30 00:22:33"
        },
        "score": {
            "zendrive_score": 62,
            "cautious_score": 92,
            "control_score": 71
        }
    }
