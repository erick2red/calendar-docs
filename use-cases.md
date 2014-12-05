## Edit dialog

### Use cases

1. User wants to add his drum lessons, which are on Thursday and Friday, every week
2. User wants to add his SCRUM meeting which are on every month the 3rd day
3. User wants to add his next GNOME keynote at GUADEC, which will happens on another country on another timezone
   + The user might wants to see which is the time of keynote presentation in the local timezone and in the timezone of the event.
   + User wants to submit this event to its GNOMErs friends.
   + User wants to attach the keynote pdf file.
4. User wants to open Gnome Maps to check the location of his next GUADEC talk
5. User wants to delete one of the drum lessons, just the one.
6. User wants to delete every drum lesson from the schedule (A)
7. User wants to confirm his going into a party his friends are scheduling for next week. (B)
8. User wants to mark his touring with his band an all-day event
9. User wants to set an alarm as a reminder of his upcoming visa application
10. User wants to set an email as a reminder to his wife of her upcoming visa application (C)
11. User wants to set one of the participants on an event as the **Organizer**
12. User might want to copy a shared event to his calendar. (D)

### Notes

+ (A): Here the API provides four possibilities:
  + Remove one instances
  + Remove every instance
  + Remove from this instance forward
  + Remove from this instance into the past
+ (B): assistance: Yes, No, Maybe, dunno right now, what the API provides, but design for everything we will see.
+ (C): Notice that again I have no idea of how to handle this. It's just that I've seen calendar clients doing it. Also, there's other types of reminders: the API from eds provides also a sound playing.
  + The alarms can be set 5, 10, or whatever time the user estimates before the event, after the event, or at an absolute time.
+ (D): After all, this is a bonus. Google does show link for this on his online calendar.
