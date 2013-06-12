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

* *DaysGrid object*
  1. It's height is divided in 48 cells representing each
     half-hour of the day
