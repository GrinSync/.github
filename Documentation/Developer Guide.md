# Developer Guide
This is a guide to the developers of GrinSync. 

## Table of Contents
- ### Flutter - Creating Pages for the App
- ### Software Architecture
- ### Database Structure
- ### Languages and Frameworks
- ### API Documentation
- ### Coding Guidelines

## Flutter - Creating Pages for the App
Imagine a page in the app as one big widget. We will create this widget and add functionalities to it. 

This is the basic scheme for creating a new page. 

```dart
// Define a new class that extends the StatefulWidget class. This means that the page is a widget
// that can have mutable state.
class NEWPage extends StatefulWidget {
  const NEWPage({super.key}); // Define a constructor
  // Specify that when a new instance of the page is created, it should create a corresponding state
  // object of a new type that is defined below
  // (which will be how we manage the state of the widget of the page)
  @override 
  State<NEWPage> createState() => _NEWPage();
}

// Define a state class to manage the state of the widget 
class _NEWPage extends State<NEWPage> {
  // These are variables that need to be stored for this page
  // late indicates that the variables are initialized later on
  late final TextEditingController ...;
  late final String ...;
  late List<String> ...;

  /// Initialize variables defined above here
  @override
  void initState() {
    ... 
    super.initState();
  }

  /// Dispose of the variable defined above when the page closes.
  @override
  void dispose() {
    ...
    super.dispose();
  }

  /// Here is where we build the page interface
  @override
  Widget build(BuildContext context) {
    return Scaffold(...);
  }
```

If your page doesn't contain any variables or the value of your variables will never change, it is recommended to use a StatelessWidget. The basic scheme looks like this. 

```dart
class NEWPage extends StatelessWidget {
  final String text;
  ... // Other variables here
  NEWPage({Key? key, required text}) : super(key: key); // constructor

  @override
  Widget build(BuildContext context) {
    return Scaffold(...);
  }
}

```

The widgets that comprise the page UI/interface go within the Scaffold widget. The Scaffold widget provides you areas to place an appBar, a body. You don't have to use Scaffold widget if an appBar is not needed. You can return the widget that goes into the 'body' of Scaffold widget directly. We suggest going through this tutorial to learn how to add UI elements: https://codelabs.developers.google.com/codelabs/flutter-codelab-first?hl=en#0 

## Languages and Frameworks
### Frontend
- Framework: [Flutter](https://flutter.dev/)
- Language: [Dart](https://dart.dev/overview)
### Backend
- Framework: [Django](https://www.djangoproject.com/)
- Language: [Python](https://docs.python.org/3/)

## API Documentation
Currently, these are the available API functions to interact with the backend: 

```python
path('api/search', apiViews.search, name = 'search'),
path('api/getUser', apiViews.getUser, name = 'getUser'),
path('api/getEvent', apiViews.getEvent, name = 'getEvent'),
path('api/getAll', apiViews.getAll, name = 'getAll'),
path('api/getCreatedEvents', apiViews.getAllCreated, name = 'getCreatedEvents'),
path('api/getLikedEvents', apiViews.getLikedEvents, name = 'getLikedEvents'),
path('api/likeEvent', apiViews.likeEvent, name = 'likeEvent'),
path('api/unlikeEvent', apiViews.unlikeEvent, name = 'unlikeEvent'),
path('api/toggleLikedEvent', apiViews.toggleLikedEvent, name = 'toggleLikedEvent'),
path('api/editEvent', apiViews.editEvent, name = 'editEvent'),
path('api/deleteEvent', apiViews.deleteEvent, name = 'deleteEvent'),
path('api/upcoming', apiViews.getUpcoming, name = 'getUpcomming'),
path('api/getAllTags', apiViews.getTags, name = 'getAllTags'),
path('api/getUserTags', apiViews.getUserTags, name = 'getUsersTags'),
path('api/auth', tokenViews.obtain_auth_token),
path('api/validate/login', apiViews.validateLogin, name = 'vallog'),
path('api/validate', apiViews.validate, name = 'val'),
path('api/create/user', apiViews.createUser, name = 'newUser'),
path('api/verifyUser', apiViews.verifyUser, name = 'newUser'),
path('api/create/event', apiViews.createEvent, name = 'newEvent'),
path('api/create/tag', apiViews.createTag, name = 'newTag'),
path('api/updateInterestedTags', apiViews.updateInterestedTags, name = 'updateTags'),
path('api/claimEvent', apiViews.claimEvent, name = 'claimEvent'),
path('api/reassignEvent', apiViews.reassignEvent, name = 'reassEvent'),
path('api/create/org', apiViews.createOrg, name = 'newOrg'),
path('api/confirmOrg', apiViews.confirmOrg, name = 'addCoLead'),
path('api/claimOrg', apiViews.claimOrg, name = 'claimOrg'),
path('api/confirmOrgClaim', apiViews.confirmOrgClaim, name = 'addCoLead'),
path('api/getOrg', apiViews.getOrg, name = 'getOrg'),
path('api/getUserOrgs', apiViews.getUserOrgs, name = 'usersOrgs'),
path('api/getAllOrgs', apiViews.getAllOrgs, name = 'allOrgs'),
path('api/getOrgEvents', apiViews.getOrgEvents, name = 'getOrgsEvents'),
path('api/getFollowedOrgs', apiViews.getFollowedOrgs, name = 'getFollowedOrgs'),
path('api/followOrg', apiViews.followOrg, name = 'followOrg'),
path('api/unfollowOrg', apiViews.unfollowOrg, name = 'unfollowOrg'),
path('api/toggleFollowedOrg', apiViews.toggleFollowedOrg, name = 'toggleFollowedOrg'),
```
For the complete list, please refer to: https://github.com/GrinSync/GrinSyncBackend/blob/main/GrinSync/urls.py

## Coding Guidelines
### Frontend

#### Widgets
- Know what each widget does in a widget tree, remove the ones that don't do anything. Here is an examples: 
  - If you have a scaffold, but all the child widgets are only in 'body' of the scaffold, and you are not calling anything only scaffold has (a snackbar, for example), you should probably remove the scaffold and pass the widget in scaffold's body directly to scaffold's parent widget.

#### Page Refresh
This is a mechanism implemented to avoid refresh issues where a child widget modifies an object, but the parent widget still shows the old version before modification. An example of the issue (it actually happens twice in this example): 
1. A user opens events I created page and see a list of events they created.
2. They click on an event card to go to the event details page of the event.
3. There they selects 'Edit Event' to go to the event edit page.
4. At event edit page, they change the start time of the event.
5. After saving the changes, they go back to the event detials page, but the start time shown on the page is before their changes are made (because the event details page is not refreshed).
6. They go back to the events I created page, the event card also shows the old start time (because the events I created page is not refreshed). 

##### Description of The Mechanism
- A page should have a refresh function (where the page updates its field(s) by getting the information from the backend again) if its child widgets can modify the page's content shown. When calling the child widget, pass the refresh function to the child widget. 
  - This requires the page to be a StatefulWidget
  - Homepage (upcoming events) is the only exception because we don't want to refresh the page which interrupts the user's event exploration.
- A child widget should always take in a VoidCallback function called refreshParent (its parent widget's refresh function) if the child widget is able to modify what the parent widget pass to it. Whenever the child widget modifies the object (e.g. an Event) the parent passed to it, it should call refreshParent().
- If the child widget has a refresh function too, the refreshParent function should be wrapped in it as well, so that when the child widget is refreshed by the grandchild widget, the grandparent widget can be refreshed as well. 
- At times when the parent widget doesn't have a refresh function/doesn't require the child widget to refresh it, but the child requires a VoidCallback, an empty anonymous function ('() {}') can be passed to the child widget.


##### Implementation of the Mechanism
Starting from EventsICreatedPage.dart where we implemented the events I created page, here is the refresh function decalred in the `_EventsICreatedPageState` class. The SetState function triggers a rebuild of the page with the new _loadEventsFuture value. 

```dart
  /// Function to refresh the page by loading the events again
  refresh() {
    setState(() {
      _loadEventsFuture = loadEvents();
    });
  }
```

`_loadEventsFuture` is a local variable of type `Future` which the `FutureBuilder` depends on. `FutureBuilder` waits for the Future to complete and builds the list of event cards: 

```dart
// Use a FutureBuilder to wait for the events to load
      body: FutureBuilder(
          future: _loadEventsFuture,
          builder: (context, snapshot) {
            // if the connection is waiting, show a loading indicator
            if (snapshot.connectionState == ConnectionState.waiting) {
```
In the refresh function, by assigning the function `loadEvents()` to `_loadEventsFuture`, FutureBuilder will wait for the the function `loadEvents()` to run, and rebuild the list once the function finishes. With this, whenever a child widget calls refresh(), this page will refresh itself. 

Clicking on an event card will lead us to the event details page, we pass the event to the event card and the refresh function: 
```dart
child: EventCardPlain(
  event: events[index],
  refreshParent: refresh,
),// EventCardPlain is an event card Widget in get_events.dart
```
The event card uses the Event object to display the event information, but doesn't use the refresh function itself. Rather, it passes it to the event details page so that the refresh function can be called inside the event details page: 

```dart
// Navigates to the Event Details page if the user taps on the event card
onTap: () {
  Navigator.push(
    context,
    CupertinoPageRoute(
      builder: (context) => EventDetailsPage(
          event: event, refreshParent: refreshParent),
    ),
  );
},
```

In event details page, whenever a change to the event is made, for example, when an event is deleted, this function will be called: 

```dart
onPressed: () async {
  String deleteMsg =
      await deleteEvent(event.id); // Call deleteEvent to delete the event from the backend
  // Show a toast message to confirm deletion
  Fluttertoast.showToast(
      msg: deleteMsg,
      toastLength: Toast.LENGTH_SHORT,
      gravity: ToastGravity.CENTER,
      timeInSecForIosWeb: 1,
      backgroundColor: Colors.grey[800],
      textColor: Colors.white,
      fontSize: 16.0);

  Navigator.of(context).pop(); // Dismiss the dialog
  Navigator.of(context).pop(); // Dismiss the page

  widget.refreshParent(); // Refresh the parent page
},
```

At the same time, event details page also has its own refresh function, which gets the event object from the backend directly by the event id. This function is passed to child widgets such as the event edit page: 
```dart
/// Function to refresh the event details page by getting the event by its ID again
refresh() async {
  var newEvent = await getEventByID(event.id);
  setState(() {
    if (newEvent != null) {
      event = newEvent;
    }
  });
  widget.refreshParent(); // if this refreshes, the parent page should refresh too
}
```
Notice that the refreshParent function is included in event details page's refresh function too. This because when the child widget calls the function to refresh the event details page, it indicates that some changes have been made to the event. Hence it's probably a good idea to refresh event details page's parent widget too. 

