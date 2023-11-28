import datetime
import json

class Event:
    def _init_(self, name, address, category, time, description):
        self.name = name
        self.address = address
        self.category = category
        self.time = time
        self.description = description
        self.attendees = []

class EventManager:
    def _init_(self):
        self.events = []

    def add_event(self, event):
        self.events.append(event)

    def view_events(self):
        for event in self.events:
            print(f"{event.name} - {event.time}")

    def join_event(self, event, user):
        event.attendees.append(user)

    def cancel_event(self, event, user):
        if user in event.attendees:
            event.attendees.remove(user)
            print(f"You have successfully canceled your participation in {event.name}")
        else:
            print(f"You are not registered for {event.name}")

    def nearest_event(self):
        now = datetime.datetime.now()
        nearest = min(self.events, key=lambda x: abs(x.time - now))
        print(f"The nearest event is {nearest.name} at {nearest.time}")

    def past_events(self):
        now = datetime.datetime.now()
        past_events = [event for event in self.events if event.time < now]
        print("Past events:")
        for event in past_events:
            print(f"{event.name} - {event.time}")

class User:
    def _init_(self, username, email, password):
        self.username = username
        self.email = email
        self.password = password

def save_events(events):
    with open("events.data", "w") as file:
        json.dump([vars(event) for event in events], file)

def load_events():
    try:
        with open("events.data", "r") as file:
            data = json.load(file)
            events = [Event(**event) for event in data]
            return events
    except FileNotFoundError:
        return []

if _name_ == "_main_":
    event_manager = EventManager()
    event_manager.events = load_events()

    while True:
        print("\n1. View Events\n2. Join Event\n3. Cancel Event\n4. Nearest Event\n5. Past Events\n6. Exit")
        choice = input("Enter your choice: ")

        if choice == "1":
            event_manager.view_events()
        elif choice == "2":
            event_manager.join_event()
        elif choice == "3":
            event_manager.cancel_event()
        elif choice == "4":
            event_manager.nearest_event()
        elif choice == "5":
            event_manager.past_events()
        elif choice == "6":
            save_events(event_manager.events)
            break
        else:
            print("Invalid choice. Please try again.")
