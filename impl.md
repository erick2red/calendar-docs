Implementation details
=======================

* Views requirements:
  1. API: `gcal_view_mark_current_unit` (event creation from [New Event] button)
  2. Mark a range of units, or a single one (2nd and 3rd ways of New Event flow)
  3. API: `gcal_view_clear_mark` (clear the marks made on the view)
  4. Signal about an event creation (2nd and 3rd ways of New Event flow)
  5. Handle enable/disable states (visible or not)
     Any changes to the views widgets when they're not visible, shouldn't cause
	 repaint, nor resize
  6. API: `gcal_view_get_initial_date`
  7. API: `gcal_view_get_final_date`
  8. API: `gcal_view_contains_date`
  9. API: `gcal_view_get_left_header`
  9. API: `gcal_view_get_right_header`

* Main window requirements:
  1. Keep a list of views, initialized views, not activated views.
  2. Keep a reference to a SearchResultsView
  3. Keep an initiated Edit dialog
  4. The usual widget family for the toolbar and the searchbar
  6. Keep states: *new-event creation/search/edit event*
  7. API: `gcal_window_show_view` (for settings the view)
  8. API: `gcal_window_new_event` (for creating an event)
  9. API: `gcal_window_set_search_mode`

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
  5. oa GtkListStore with calendar/sources info. (not in use now)
