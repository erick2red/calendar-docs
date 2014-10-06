Specifications
===============

* *Application*
  1. Has command-line processing for open an specified view and
     on a specified date
  2. Has gsettings for storing stuff:
     1. Last date viewed
	 2. Last view used
	 3. Last window's position and size
  3. A `#GcalManager` object as calendar backend

* *Views*
  1. Units on YearView will be months
  2. Units on MonthView will be days
  3. Units on WeekView will be days
  4. Units on DayView will be hours
  5. WeekView and DayView has equals all-day sections
  6. The position of the marked cells sent is taken from the center of the starting cell

* *DayView*
  1. Will show, initially, not just *today* but *tomorrow* as well
  2. Will create current event in days-grid view (only)

* *DaysGrid object*
  1. It's height is divided in 48 cells representing each
     half-hour of the day
  2. Will have to mark the actual time of day (hour)

* *EventWidget*
  1. Keeps its icalcomponent copy

* *Manager*
  1. `GcalEventData` contains:
     1. a weak `ESource` references
	 2. a owned (by the widget) `ECalComponent` representing the event
  2. `:objects-added` signal pass a a list of `GcalEventData` that will finally be passed onto widgets by `GcalWindow`
