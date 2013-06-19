Flows
======

*Starting the application*

* Creating a gcal_window
  1. Connect the handlers to `GcalManager` signals
  2. Get the date the app will show either from settings or from the actual view
  3. Instantiate every view
  4. Show the last active view (got from the settings)

*Showing a view in a window*

* Clean previous states (**new event creation/search**)
* API: `gcal_main_view_changed`
* Update `GcalMainWindow.view_type`
* call `update_view`

*Update the app date*

* API: `update_view`: update only the active view
  1. drop every children
  2. queue resize/redraw if visible
  3. retrieve/set new time-range (always)
  4. update NavBar headers

*Create an event*

* Enter `new-event-mode`.
* disable toolbar
* First approach
  1. call `gcal_window_new_event` (from <Ctrl>N or [New Event] button)
  2. set `#GcalWindow:new-event-mode` to TRUE
  3. call `gcal_view_mark_current_unit`
  4. call `gcal_window_show_new_event_widget`
  5. handle outcome of widget
  6. unset `#GcalWindow:new-event-mode`
* Second approach:
  1. Receive `#GcalView:create-event` signal
  2. set `#GcalWindow:new-event-mode` to TRUE
  3. call `gcal_window_show_new_event_widget`
* handle outcome of the widget
* unset `#GcalWindow:new-event-mode`
* enable toolbar

*Search events*

* Init search
  1. set `#GcalWindow:search-mode`
  2. create `#GcalSearchView`
  3. update `priv->header_bar`
  4. add it to `GtkStack`, show it
* End search
  1. unset `#GcalWindow:search-mode`
  2. free `GcalSearchView`
  3. update `header_bar`
  4. reinstate last active view

*`#GcalManager:events-added*
*`#GcalManager:events-removed*
*`#GcalManager:events-modified*
*`#GcalManager:events-created*
