## Design tinder

### Functional Requirements
- Users can create a profile with preferences(ex: age, interests, distance)
- Users can view stack of potential matches
- Users can swipe left/right on those profiles
- Users can get a match notification if it happens

### Non-Functional Requirements
- The system should have strong consistency for swiping. If a user swipes yes on the user who already swiped yes on them, they should get a match
- The system should scale to 5M daily users, with approx >100M daily swipes
- System should load the profile stack with min latency
- System should not show profiles that user has already swiped on


