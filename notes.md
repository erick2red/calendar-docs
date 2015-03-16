Notes
======

1. We load calendar sources from Online Accounts, and the default sources.
   (So we will need a view for adding sources)

2. Using sexp for getting objects between two dates:
   (occur-in-time-range? (make-time "20001116T230000Z") (make-time "20001217T230000Z") "America/Havana")

3. We will merge all the description from `e_cal_component_get_description_list`
   into one big description for an event. Actually the thing is for now we just handle `E_CAL_COMPONENT_EVENT`
   any other VType will cause unknown behavior

4. I have done a lot of simplication from what the API of e-d-s offers to how we
   show alarms of events. The code is marked with comments inside GcalManager.
   Among other stuff we show only reminders set before the event, and just
   before the start time of the event.

5. All-day events are those that fall under one of these conditions:
   - Both, start date and end date are marked with is_date

6. `icaltime_as_timet` doesn't include `icaltimetype` timezone. You have to use `icaltime_as_timet_with_zone`

7. `uuid` across the whole code refers to a string composed by "`source_uid`:`event_uid`"

8. Passing of events data around will be made using `ECalComponent` struct from eds.
   Date/time data will be using `ECalComponentDateTime` struct until we managed to get something better like GDateTime.

9. Date/time show on every view on the application will me local time, if the event has a different timezone set, it will be converted
