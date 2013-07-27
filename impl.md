Implementation details
=======================

* Views requirements:
  1. Mark a range of units, or a single one (2nd and 3rd ways of New Event flow)
  2. Signal about an event creation (2nd and 3rd ways of New Event flow)
  3. Handle enable/disable states (visible or not)
     Any changes to the views widgets when they're not visible, shouldn't cause
	 repaint, nor resize
  4. API: `gcal_view_get_initial_date`
  5. API: `gcal_view_get_final_date`
  6. API: `gcal_view_draw_event`
  7. API: `gcal_view_get_left_header`
  8. API: `gcal_view_get_right_header`
  9. API: `gcal_view_mark_current_unit`
  9. API: `gcal_view_clear_marks`

* Main window requirements:
  1. Keep a list of views, initialized views, not activated views.
  2. Keep a reference to a SearchResultsView
  3. Keep an initiated Edit dialog
  4. The usual widget family for the toolbar and the searchbar
  6. Keep states: *new-event creation/search/edit event*
  7. API: `gcal_window_show_view` (for settings the view)
  8. API: `gcal_window_new_event` (for creating an event)
  9. API: `gcal_window_set_search_mode`
  10. Install a key-press listener over the entire application

* GcalNavBar requirements:
  1. Send back/forward signals
  2. Keep labels with headers

* GcalManager internals
  1. a list of GcalStoreUnit containing:
     1. Esource, ECalClient,ECalClientView,
	 2. `#GHashTable<uid, #ECalComponent>` (as *active events* of the view)
  2. a list of ESourceGroup
  3. `#icaltimetype *initial_date, *final_date`
  4. Emit signals for:
     1. objects-added   (wrapping ECalClientView)
     2. objects-removed (wrapping ECalClientView)
     3. events-changed (for time range changing ??)
  5. a GtkListStore with calendar/sources info. (not in use now)

* DaysGrid
  1. API: `gcal_days_grid_mark_hour`: will draw the blue mark of current unit
  2. `scale_width` internal attribute include left and right padding
