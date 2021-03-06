# Full-stack code challenge

door2door, the operator of the 'allygator shuttle' service in Berlin, collects the live position of all vehicles in its fleet in real-time via a GPS sensor in each vehicle. To manage the service, door2door needs to be able to visualize the location and bearing of all vehicles in real-time, and has asked you to provide a solution for this.

To aid you in development of this solution, door2door has provided a tool which will simulate the registration and GPS emissions from their vehicles.

## Collecting vehicle locations

The vehicle software will expect to be able to send information to your solution using the following HTTP API endpoints.

### Vehicle registration

`POST /vehicles`

Request body:

```json
{ "id": "some-uuid-here" }
```

Response status code: 204

Response body should be empty.

The vehicle will contact this endpoint to register itself in your solution. It will pass in a universally unique ID to identify itself. After receiving this registration request, you should expect to start receiving vehicle location updates from the vehicle.

**Example:** the vehicle with the ID `abc123` wants to start sending location updates. It would contact `POST /vehicles` with ``{ "id": “abc123” }`` in the request body.

### Location update

`POST /vehicles/:id/locations`

Request body:

```json
{ "lat": 10.0, "lng": 20.0, "at": "2017-09-01T12:00:00Z" }
```

Response status code: 204

Response body should be empty.

The vehicle will contact this endpoint whenever a new location is detected, but no more than once every 3 seconds. It will contain the latitude and longitude of its position, and the timestamp that the position was recorded in ISO8601 format. The vehicle will never contact this endpoint without having registered first.

**Example:** the vehicle with the ID `abc123` has recorded a new position at 12pm CET on 2nd December 2017, at the position (51.2, 45.3). It would contact `POST /vehicles/abc123/locations` with ``{ "lat": 51.2, “lng”: 45.3, “at”: “2017-12-02T12:00:00+01:00” }`` in the request body.

### Vehicle de-registration

`DELETE /vehicles/:id`

Response status code: 204

Response body should be empty.

The vehicle will contact this endpoint when it will stop emitting location updates. After this endpoint has been called, the vehicle will never send further location updates unless it calls the vehicle registration endpoint again.

**Example:** the vehicle with the ID `abc123` has finished emitting location updates. It would contact `DELETE /vehicles/abc123` to indicate this.

## Visualizing vehicle location

You are free to choose how to visualize the current vehicle positions, but the most obvious choice would be to display them on a map in some way. You must also visualize the bearing/direction of the vehicle.

You can look at the Google Maps, Mapbox and Leaflet libraries as options to help you with building a map visualization. Try to consider how easy your visualization is to understand. Some concepts which can help to improve the visualization:

* Group clusters of vehicles together
* Modify your visualization depending on zoom level

We expect your visualization to be accessible via a web browser. It only needs to be tested against the most recent version of Chrome.

## Additional business requirements

If a vehicle has exited the "city boundaries", the system should disregard any GPS emissions from that vehicle. The city boundary is currently assumed to be a 3.5km radius around door2door's office which is located at latitude 52.53 and longitude 13.403.

All valid position data received must be stored somewhere for future analysis and consumption by the door2door data science team.

## Technical assumptions

* The API is expected to be implemented using one of the following languages: Ruby, Python or Javascript.
* You are free to make use of any frontend framework or library you please (if any), but you should justify your choice.

## Optional extras

* A containerized solution is not a requirement, but definitely preferred for us to easily deploy your solution and test. Therefore, adding a Dockerfile to your solution is optional, but recommended.

* We expect your solution to be able to handle the load from the provided simulation tool with ease. As an optional extra, you can demonstrate to us how your solution would scale to handle 1000+ vehicles. This could be through a screen capture, or some form of clearly readable performance benchmarking, or in another way if you choose.

## Delivery of your solution

Please deliver your solution to us as a publicly accessible git repository, or in a ZIP file. The repository should contain full instructions for us to run the solution on our own machines.

If you are able to publicly host the solution somewhere (e.g. Heroku, AWS), this would be a great bonus.

## Reviewing

The following description will give you an understanding of how we review the code challenge. What matters to us is to learn how you write code and what you consider as clean code. For us it is more important to have an understandable project than a complex algorithm.

The criteria that we are looking for are the following:

- Documentation: Is the project and the code properly documented?
- Correctness: Is the task solved? If there is anything missing, is the reason why it is missing documented?
- Technology: Which libraries or approaches are used? Do they make sense for the task? Justify why you've decided to use those technologies to solve the code challenge.
- Code quality: Is the code understandable and maintainable? What programming paradigm is being used? Is it implemented correctly? Is the project linted?
- Tests: How is the project tested? Does the project contain system and unit tests? Is the entire project tested or just parts of it?

## Using the simulation tool

Check the `driver-simulator/README.md` file in order to learn how to use the simulation tool.
