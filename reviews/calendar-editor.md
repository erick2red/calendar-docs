## Things to fix

+ If you're keep using the hash-table:
    + [ ] use [e_source_hash()][1], and `e_source_equal()` instead of direct hash
    + [ ] ensure the hash-table get destroyed
+ [ ] rename `make_row_for_source()` to `make_row_from_source()`

+ [ ] on *gcal-source-dialog.c* remove whitespace lines 159,161
+ [ ] on *gcal-source-dialog.c:195,196* why unref()
+ [ ] on *gcal-source-dialog.c:195,196* what will happens if the previous default has been deleted?
+ [ ] on *gcal-source-dialog.c:205,274* why parenthesis
+ [ ] on *gcal-source-dialog.c:329,335* use `g_clear_object()`, and remove the parenthesis afterwards
+ [ ] on *gcal-source-dialog.c:392* move `g_ensure_type()` to `gcal_application_init()` or `gcal_source_dialog_init()`
+ [ ] on *gcal-source-dialog.c:445* you can place the details frame outside of the GtkNotebook like [A], and avoid the layout changing
+ [ ] on *gcal-source-dialog.c:481* remove `spinner_damaged` altogether and use `gtk_entry_progress_pulse()`
+ [ ] on *gcal-source-dialog.c:566* `gtk_dialog_set_default_response()` might not do anything, check it tho
+ [ ] on *gcal-source-dialog.c:577* what are you doing on `gcal_source_dialog_finalize()`. Why are you overriding it?
+ [ ] on *gcal-source-dialog.c* remove unused `static GParamSpec *gParamSpecs [LAST_PROP]`
+ [ ] on *gcal-source-dialog.c:585,500* Why are you overriding set/get_property pairs of functions?
+ [ ] on *gcal-source-dialog.c:666* use `self->priv = gcal_source_dialog_get_instance_private (self);` instead
+ [ ] on *gcal-source-dialog.c:722,736* avoid setting the size hardcoded by pixels. You never know how much width a translation will take, or the height of the default font the user has on is pc
+ [ ] on *gcal-source-dialog.c:724* `gtk_header_bar_set_subtitle()` allows NULL to unset, check the docs
+ [ ] on *gcal-source-dialog.c:755* I don't like the blocking/unblocking, the same is done now on GcalEditDialog, there's a better way of doing it. :-)
+ [ ] on *source-dialog.ui:210* use `GtkFileChooserButton` instead of simple button plus file-dialog
+ [ ] See the attached diff about gcal_source_dialog_set_mode()
```
	diff --git a/data/ui/source-dialog.ui b/data/ui/source-dialog.ui
	index 68504b0..f8a52db 100644
	--- a/data/ui/source-dialog.ui
	+++ b/data/ui/source-dialog.ui
	@@ -40,6 +40,7 @@
		     <property name="can_focus">False</property>
		     <property name="hexpand">True</property>
		     <property name="vexpand">True</property>
	+            <property name="vhomogeneous">False</property>
		     <child>
		       <object class="GtkGrid" id="edit_grid">
		         <property name="visible">True</property>
	@@ -259,7 +260,7 @@
		 <child>
		   <object class="GtkButton" id="cancel_button">
		     <property name="label" translatable="yes">Cancel</property>
	-            <property name="visible">True</property>
	+            <property name="visible" bind-source="headerbar" bind-property="show-close-button" bind-flags="default|sync-create|invert-boolean"/>
		     <property name="can_focus">True</property>
		     <property name="receives_default">True</property>
		     <signal name="clicked" handler="action_widget_activated" object="GcalSourceDialog" swapped="no"/>
	@@ -268,7 +269,7 @@
		 <child>
		   <object class="GtkButton" id="add_button">
		     <property name="label" translatable="yes">Add</property>
	-            <property name="visible">True</property>
	+            <property name="visible" bind-source="headerbar" bind-property="show-close-button" bind-flags="default|sync-create|invert-boolean"/>
		     <property name="can_focus">True</property>
		     <property name="sensitive">False</property>
		     <property name="receives_default">True</property>
	diff --git a/src/gcal-source-dialog.c b/src/gcal-source-dialog.c
	index a993753..75459d6 100644
	--- a/src/gcal-source-dialog.c
	+++ b/src/gcal-source-dialog.c
	@@ -714,10 +710,6 @@ gcal_source_dialog_set_mode (GcalSourceDialog    *dialog,
	   edit_mode = (mode == GCAL_SOURCE_DIALOG_MODE_EDIT);

	   gtk_header_bar_set_show_close_button (GTK_HEADER_BAR (priv->headerbar), edit_mode);
	-  gtk_widget_set_visible (priv->cancel_button, !edit_mode);
	-  gtk_widget_set_visible (priv->add_button, !edit_mode);
	-  gtk_widget_set_visible (priv->edit_grid, edit_mode);
	-  gtk_widget_set_visible (priv->notebook, !edit_mode);
	   gtk_stack_set_visible_child_name (GTK_STACK (priv->stack), edit_mode ? "edit" : "create");

	   if (!edit_mode)
```
+ [ ] on *gcal-manager.c:1145* use `e_source_registry_commit_source()` async before sync
+ [ ] on *gcal-manager.c:994* `g_object_get()` usually "transfer full" and doc says "transfer none", check out and free the string returned in gcal-source-dialog.c:459

+ [ ] on *gcal-window.c:250* we need to come up with a better method to undo generic operations, it may not be undo in first place
+ [ ] on *gcal-window.c:428* I rather keep the default calendar on-top, and maybe the rest of them organized by name as you did?
+ [ ] on *gcal-window.c:428* This method is run every pair of row in the listbox, and for every time the methods is run, you're creating a list with the hash-table keys and releasing it. This is slow.
+ [ ] on *gcal-window.c:461* use `e_source_compare_by_display_name()`
+ [ ] on *gcal-window.c:119* Read note [B]
+ [ ] on *gcal-window.c:873,914* If you're going to hide the dialog every time it has responded, `connect_after` `gtk_widget_hide()` to `GtkDialog:reponse` and be done with it
+ [ ] on *gcal-window.c:1533* call `gcal_source_dialog_set_manager()` on line 1628, when setting the manager property.

## Notes:

+ **A**
        +-----------------------+
        |+----------+----------++
        || remote   | local    ||
        |+---------------------+|  <-- notebook
        ||                     ||
        ||                     ||
        ||                     ||
        ||                     ||
        |+---------------------+|
        |+---------------------+|
        || details frame       ||
        ||                     ||
        ++---------------------+|
        +-----------------------+
+ **B**:
  + Why are you using a hash-table instead of a list, or attached data. The idea behind using a hash is to be able to lookup by its keys very fast, but the only time you are finding something in this hash-table you are iterating through its value because what you got its a row, not an ESource.
  + Second, where the destroying on the hash-table? Call `g_hash_table_destroy()` where appropriate
+ What's `GcalWindow::removed_source` for?

[1]: https://developer.gnome.org/libedataserver/stable/ESource.html#e-source-hash


