---
layout: post
title:      "Rails Event Project App - Associations"
date:       2020-11-18 08:04:54 +0000
permalink:  rails_event_project_app_-_associations
---


For my rails project I decided to do a simple event app that would allow users to create, edit, and delete their own events and reviews, while also allowing them to view the events and reviews by other users. One of the things that I thought made it difficult for me to start my project was understanding the associations of my three models: Review, User, and Event. In the case of reviews it is pretty simple to understand that reviews belongs to a user and belongs to an event. Like so:

```
class Review < ApplicationRecord
  
  belongs_to :user
  belongs_to :event


end

```

However, things get a little bit tricky with the User and Event models. In the case of the User model, you can say that a user has_many events and has_many reviews because they should be able to create, view, edit, and delete their own events and reviews. But, a user's events also has reviews written by other users and that user has reviews for events created by other users. With this in mind, a simple has_many association is not enough and so thats where through  and source comes in. Because a user' events will have reviews written by others we can have a reviewed_events association to reviews, however rails will look for an association reviewed_events in the Reviews class, which is why we need to tell it to use the event association instead. Like so:

```
class User < ApplicationRecord

  has_many :events
	has_many :reviews
	has_many :reviewed_events, through: :reviews, source: :event
	
	end
```

Lastly, its pretty clear that an event has_many reviews. But an event also belongs_to a user that we want to distinguish it from, say a user who maybe wrote a review for some event. We can call the user who created the event, the host. But if we say that an event belongs_to a host, Rails is not going to know what a host is so we can use the :class_name => "User" to clarify that we are actually referring to the User class. In contrast, we can call the user who wrote a review for some event, a reviewer. Again, Rails is not going to know who the reviewer is so we can tell it that an event has many reviewers, which are actually users, through the reviews association.

```
class Event < ApplicationRecord
  
  has_many :reviews
	belongs_to :host, :class_name => 'User'
	has_many :reviewers, through: :reviews, source: :user
	
	end
```

