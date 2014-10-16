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
  6. API: `gcal_view_draw_event`: Whether the event within these boundaries will be drawn by the view
  7. API: `gcal_view_get_left_header`
  8. API: `gcal_view_get_right_header`
  9. API: `gcal_view_mark_current_unit`
  9. API: `gcal_view_clear_marks`
  10. Implement `ECalDataModelSubscriber`
  11. Updates its subscribed range on every date change
  12. Handle events children widgets on its vfuncs and,
      there's no need of calling `gcal_view_clear` nor anything
	  like that on showing, if only just queue a redraw on show
  13. API: `gcal_view_set_manager`: So the view could update its range internally.
      Defined to keep an internal week reference to the app `GcalManager` instance.
  14. Keep a property for a date
  15. Keep a property for a manager.

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
  1. Send today signal
  2. Keep labels with headers

* GcalManager internals
  1. a hash of `GcalManagerUnit` containing:
     1. Esource as keys
	 2. a strut containing ECalClient,enabled,color as values
  2. `#icaltimetype *initial_date, *final_date`
  3. A `ECalDataModel` instance
  3. Creation flow:
    1. Read sources and schedule connecting of each client
	2. Create `ECalDataModel` in `_init` vfunc
	3. Delayed: once the client is connected add them to the `e_cal_data_model` instance

* Edit dialog details
  1. It receives an `ECalComponent` and use it for rendering values
     and record changes. If the user changed the summary of an event
	 the proper flow would be to update the internal `ECalComponent`
	 using `e_cal_component_set_summary`.
  2. Use three responses:
     1. `GCAL_RESPONSE_DELETE_EVENT`: for the action of delete the event
     2. `GCAL_RESPONSE_SAVE_EVENT`: for saving he changes made to the event
     3. `GTK_RESPONSE_CANCEL`: for canceling any changes
  3. Use a GtkBuilder file for loading the ui, as much as possible
