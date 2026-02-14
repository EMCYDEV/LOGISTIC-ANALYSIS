# LOGISTIC-ANALYSIS
# Logistic Dashboard Usding DAX function
# Calender DAX
Calendar = 
ADDCOLUMNS(
    CALENDARAUTO(),
    "Year", YEAR([Date]),
    "Month", FORMAT([Date], "mmm"),
    "Monthnum", MONTH([Date]),
    "Weekday", FORMAT([Date], "ddd"),
    "Weeknum", WEEKNUM([Date], 1), -- Use WEEKNUM for calendar week number (Sunday start)
    "WeekdayNum", WEEKDAY([Date], 2), -- Add numerical weekday (Monday=1, Sunday=7)
    "Qtr", "Q." & FORMAT([Date], "Q"),
    "WeekType", IF(
        WEEKDAY([Date], 2) = 6 || WEEKDAY([Date], 2) = 7, -- Consistent with WeekdayNum for Saturday/Sunday
        "Weekend",
        "Weekday"
    )
)

# Active Shipment
Active Shipment = CALCULATE(COUNTROWS(Shipment), Shipment[Status] = "Active")
# Avg. Colour of Delivery Time
Avg. Colour of Delivery Time = SWITCH(TRUE(), [Avg.DeliveryTime] <= 4,"#BCEEB7",[Avg.DeliveryTime] <=10,"#BDE1F6", [Avg.DeliveryTime] >10,"#F9D24")
# Avg.DeliveryTime
Avg.DeliveryTime = DIVIDE([Dilevery Time],[Completed Shipments])
# Completed Shipments
Completed Shipments = CALCULATE(COUNTROWS(Shipment), Shipment[Status] = "Completed")
# Dilevery Time 
Dilevery Time = CALCULATE(SUM(Shipment[Delivery Time]))
#  Per. Active Shipment 
Per. Active Shipment = DIVIDE([Active Shipment],[Total Shipments])
# Per. Completed Shipment 
Per. Completed Shipment = DIVIDE([Completed Shipments],[Total Shipments])
# Per. Returned Shipment
Per. Returned Shipment = DIVIDE([Returned Shipment],[Total Shipments])
# Returned Shipmen 
Returned Shipment = CALCULATE(COUNTROWS(Shipment), Shipment[Status] = "Returned")
# Revenue 
Revenue = CALCULATE(SUM(Shipment[Sales]), Shipment[Status] = "Completed")
#  Target %
Target % = [Revenue] / 1000000
# Target Diff. 
Target Diff. = 1 - [Target %]
# Total Shipments 
Total Shipments = CALCULATE(COUNTROWS(Shipment))
# Field Parameter
Parameter = {
    ("Month", NAMEOF('Calendar'[Month]), 0),
    ("Year", NAMEOF('Calendar'[Year]), 1)

OverView :-
This logistics dashboard provides a comprehensive overview of shipment performance and revenue within a specified date range, from January 1, 2022, to December 31, 2022. The total revenue for this period is $2 million. The dashboard highlights key shipment statuses: 62% of shipments are completed (3,000 units), 33% are active (2,000 units), and 5% are returned (248 units). It also tracks total shipments (5,000) and the average delivery time (10 days). The "Avg. Delivery by Geography" chart indicates Electronics comprise 68% of shipments by category, Office Equipment 18%, Audio 5%, and Computing 34%. Japan has an average delivery time of 18 days, Australia 20 days, China 15 days, India 12 days, Germany 10 days, France 6 days, Brazil 4 days, Canada 2 days, and the USA 1 day. A "Profile" section details sales persons, their total shipments, active shipments, and completed shipments. 
