---
layout: post
title:      "Adding a search bar to my rails event app"
date:       2020-11-19 19:27:14 +0000
permalink:  adding_a_search_bar_to_my_rails_event_app
---


As part of the coding challenge for the rails project review, I was asked to add a search bar for my events app. The app allows users to create, edit, and destroy their own events and reviews, as well as only view those created by others. Originally I had only added simple filtering and sorting options through scope methods. As mentioned before, it was brought to my attention during the project review that I should add a search functionality that would allow users to search for an event by date. This article will cover how I got started and what code I needed to write to make it work.
  
Since I have never done this before, the first thing I did was research online. I found really good information out there and tried to implement that in my code with a few changes. 

As the first step, I started with the events/index.html.erb view because I knew that I needed a search form that would be displayed to the user. As I was worked on this, I realized that there were other things that needed to come first in the event model and events controller if this was to work properly. However, as a beginner, it felt more easy to understand how everything was working by beginning with the view.

In the view I added the following code:

```
views/events/index.html.erb

<h4>Search by Date:</h4>
    <%= form_tag(events_path, method: :get) do %>
      <%= datetime_field_tag(:search, params[:search])%>
      <%= submit_tag ("Search") %>
    <% end %>
```

Since the goal here is to filter through our data and display in on our index, I had the form_tag take in the events_path and the GET request. The :search key in params is very important because it holds the value of whatever the user inputs in the search bar. However, if params[:search] is going to get passed anywhere, it needs to be whitelisted or permitted in the event_params method in the events controller. 

```
app/controllers/events_controller.rb

def event_params
        params.required(:event).permit(:name, :category, :location, :price, :date, :search)
    end
```

After that’s set up, the search method needs to be defined in the event model and it needs to know what to do when it finds and event instance matching the query and what to do when it doesn’t. 

```
app/models/event.rb

def self.search(search)
      if search
      @event = Event.find_by(date: search)
          if @event
             self.where(date: @event.date)
      else
        @events = Event.all
      end
    end
  end
```

Above we define search as a class method since the events controller will be calling out search on the Event class in the index action. The self.search method takes in an argument of search which is the params{:search] from the form.
The first if statement determines what input was submitted. In this case, since I had a date time field in the form, the value for search will always be either an empty string (if user only presses the search button without selecting a date) or an actual date time (that the user selects). In the case of a user only pressing the search button without selecting a date, @event will be nil and all events would be display. If the user actually inputs (or selects) a date and a match is found, self.where will return a collection of all event instances with that date that will be used by the controller.

```
app/controllers/events_controller.rb

def index
        if @events = Event.search(params[:search])
            if @events == Event.all
            flash.now[:alert] = "No events found with that date."
            end
        else
            filter_options
        end
    end
```

Finally in the controller, we invoke the search method defined in the event model  every time we want to search for an event with a certain date. I added conditional logic here, because I wanted to add a flash message every time the search query didn’t return any matching events.
