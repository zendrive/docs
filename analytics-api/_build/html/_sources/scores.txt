Zendrive Scores
---------------

The safety and efficiency of your fleet drivers can make or break your bottom line. Zendrive SDK was built to give fleet managers insight into the quality and safety of their drivers based on detailed driving behavior. By accessing phone sensors and combining that data with context about the drive time and place, Zendrive can identify both safe and risky driving - allowing fleet managers to take action before poor driving becomes a liability. The main areas of interest are cautiousness, control and focus. Zendrive mobile SDK automatically detects the start and end of driving (called trips) and collects sensor data (GPS / accelerometer / gyroscope etc) throughout the trip. Zendrive employs state of the art data analysis and machine learning algorithms to compute various driver behavior scores. The scores are expressed as a number between 0 to 100 or -1 if they are not yet available. High values correspond to better driving practices.


.. csv-table::
    :header: "Score Name", "Description"
    :widths: 15, 50

    "Cautious Score", "The Cautious score reflects the driver’s tendency to follow the rules of the road. This includes adherence to speed limits and the tendency to avoid creating dangerous or hazardous situations for other drivers. Drivers who score poorly on Cautious are more likely to be booked for `moving violations <http://en.wikipedia.org/wiki/Moving_violation>`_."
    "Control Score", "The Control Score measures the driver’s tendency to accelerate or brake beyond normal rates. The acceptable limits for high acceleration and severe brake are 8 mph/sec. A driver frequently pushing those limits is more likely to lose control of his vehicle."
    "Focused Score", "The Focused Score captures the driver’s attention level. This score measures phone use while driving or while engaging is other risky driving behavior. Distracted drivers are more prone to create hazardous or unsafe situations on the road while driving."
    "Zendrive Score", "Each driver receives a Zendrive Score. This is a combination of the other scores - Cautious, Control, and Focus scores. The index reflects the driver’s accident risk. High scores in these three indicate safer drivers. Low scores reflect hazardous or risky driving patterns. Preventive or corrective actions should be prescribed in extreme cases."

.. note:: Since the score calculation requires high fidelity data, scores for a trip are only computed when the trips are logged by the SDK in analytics mode. This is specified as ZendriveOperationModeDriverAnalytics mode in iOS. In the android SDK, all trips are recorded in analytics mode with high fidelity data.


