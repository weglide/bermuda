# bermuda

Proposal for Search and Rescue API and architecture

## Goal

Build open APIs and standards to automate and accelerate search and rescue efforts for glider pilots.

## Architecture

The proposed concept includes multiple components:

- DB API (after file upload)
- OGN API (live)
- Algorithms that run on top of live OGN data
- Frontend to interact with API and show data on map

### DB API

Different platforms could provide an API in this format, which would enable the frontend to collect data from multiple resources.

#### Search

GET /sar/search?lat=45&lon=10&radius=50&start_time=2021-02-07T19:00:00&end_time=2021-02-07T20:00:00

Return flight id / pilot id for flights that were closer than radius to the given position in the given time interval.

#### Last position

GET /sar/flarm?flarm_id=12345

Search uploaded files for flarm contacts with the given flarm id and returns the last known position of the aircraft.

#### Notify

POST /sar/notify?lat=45&lon=10&radius=50&start_time=2021-02-07T19:00:00&end_time=2021-02-07T20:00:00

Notify all pilots that were closer than radius to the given position in the given time interval by email. Notification could be call to upload flarm files which could contain more information.

#### Other endpoints

User could save data like [SPOT](https://www.findmespot.com/en-us/) key or emergency contacts which could also be given out by the API.

### OGN API

This API runs on top of live OGN data (saved for 24 hours) in redis or database.

#### Last live position

GET /sar/last_position/{flarm_id}

Get the last position of an aircraft from OGN data.

### Algorithms

These algorithms periodically run on top of live OGN data in redis or database. E.g. use [Viterbi](https://en.wikipedia.org/wiki/Viterbi_algorithm) algorithm to find maximum likelihood (hidden) path of states (crashed / not crashed) for sequence of observed events. Maybe we can abstract so that people can work on those algorithms without being familiar to the structure of live OGN data.

#### Data

It would be useful to have actual OGN data from glider crashes / outlandings to validate and tests these algorithms.

### Frontend

- UI for above API calls
- Map to show returned position and position history (from OGN API and)
- Authentication for special users to send emails and start other processes

## Time Schedule

We want to provide the DB API and frontend until late spring. This would be in time for the 2021 season. OGN API and algorithms are expected to take more time.

## Contribution

This repository should provide a place to discuss and share ideas. Please open pull requests or issues to exchange ideas.

## Other resources

- [OGN Search and Rescue](http://wiki.glidernet.org/sar)