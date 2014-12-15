month-view allocation procedure
================================

This will describe the process of allocating event widgets on the new month-view drawn [here][1]
## Must do

+ Longer events has higher priority to be shown
+ Broken events has higher priority than everyone else but their parents
+ If a broken part of an event cannot be shown the entire event should be hidden (*a*)
+ If the initial part of an event cannot be shown, the entire event should be hidden (*a*)

## Data structures

+ A list of multidays events ordered by duration
+ A hash of single day events with days as keys, ordered by all-day, then start time, then duration

## Procedure

+ Ordering of the data structures is made at `::add`/`::remove`
+ Allocate first multidays events
  * If event has to be broken: queue allocation of its parts
  * If any parts should be hidden, hide all the parts
+ Allocate then single-day events by its order

## Notes

+ (*a*) Consult this with behaviorist specialist

[1]: https://github.com/erick2red/calendar-docs/raw/master/pics/lapo/month-view-final.png
