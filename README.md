hypermedia-design
=================

# The design of Hypermedia APIs

## Context

### The rest of REST
#### The current plateau
Representational State Transfer (REST) is a software architectural style that describes how to construct network-based software applications such that they have the best characteristics of the Web. It's very easy to get *most* of these characteristics nowadays. With tools and frameworks available to pretty much any current computing platform (tessel.io) we can build a client-server, stateless, non-shared caching, layered system that relies on location-based identifiers (URL), that can be included into the data itself (HTML) and that can be retrieved by means of a general protocol (HTTP).

#### The learning curve
Yes, we went thru a learning curve (picture RMM - http://martinfowler.com/articles/richardsonMaturityModel.html) but we have reached a plateau of sensibility and practicability. On the RMM: POST serves many useful purposes in HTTP, including the general purpose of “this action isn’t worth standardising (RMM 1, 2). ING banking app is at RMM 1.5, but we're hitting RMM 2

Still, we are missing out on the one crucial aspect of REST... 

#### Long-term evolution
"REST is software design on the scale of decades: every detail is intended to promote software longevity and independent evolution"

If you don't care what happens to your product years after it is deployed, or at least you expect to be around to rewrite it when such change occurs... this talk is not for you.

The WWW is an example; HTML was specified in 1990 (HTML4 in 1997), HTTP in 1991 (0.9) and 1996 (1.1).

#### It's hard (but not difficult)
"Many of the (REST) constraints are directly opposed to short-term efficiency. Unfortunately, people are fairly good at short-term design, and usually awful at long-term design"

An hypermedia API requires design effort; just like good UX or city planning. NY Central park: designed to have "separate circulation" systems for pedestrians, horseback riders, and pleasure vehicles. The "crosstown" commercial traffic was entirely concealed in sunken roadways, (today called "transverses"), screened with densely planted shrub belts so as to maintain a rustic ambiance. When cars were becoming commonplace, bringing with them their burden of pollution, and people's attitudes were beginning to change. No longer were parks to be used only for walks and picnics in an idyllic environment, but now also for sports, and similar recreation.

It also requires a "do not break pledge"

#### Coupling is the enemy
Or actually, implicit assumptions are the enemy. Assumptions about the objects implementing the API, about the URL (and URL patterns) that can be identified with, and the methods that be lavished on them. 

Instead, we work on a serialisation of the state of the *overall application*, not of its individual objects, work an URLs that are handed over by the application or that we construct using a (form) mechanism handed by the application. 

if a client relies on the resource being a certain type, then it certainly isn’t a RESTful API. If a client is receiving instructions on how to interact with a resource just before acting only on those instructions, then the client doesn’t care to know how the resource is implemented. How many Web browsers are aware of the distinction between an online-banking resource and a Wiki resource? None of them. As a result: no application logic in clients; we'd rather prefer fat APIs over fat clients

## Some examples
- Amazon HAL
- Github
- Stormpath

Demonstrated with a console (cURL or Spring Data console) as well as a Traverson (JS) application

Key takeaway: 'hypertext' i the simultaneous presentation of information and controls such that the information becomes the affordance through which the user (or client application) obtains choices and selects actions.

## Design
### Job-to-be-Done
The Job-to-be-Done doesD not change over time all that much (but the way they are used, presented and consumed does)(example: barbershop, portable music). JtbD-thinking is a great way to think about Hypermedia APIs. A great API knows its audience, after all. Also, Hypermedia affordances only come into play if the domain has clear *intent*.

### Customer Journey
Driven by a customer's need to get a job done. Service interaction at discrete touch points. A touch point represents a point of interaction involving a specific need in a specific time and place; a channel is medium of interaction with customers or users; n:m relationship

Touch point supports many channels, many of which did not exist at the time the touch point came into being. At the same time, that support offered by the touch point needs to be relevant for the channel.

### Approach
Need good example.

1. Identify the business proces (ie. journey)
2. Build the state machine matching that process
3. Identify an existing media type to cover for states and state transitions
4. Failing that, define a new media type (based on existing ones)
5. Implement

Viz. #3: The media type identifies a specification that defines how a representation is to be processed. That is out-of-band information (all communication is dependent on some prior knowledge).

A REST API should spend almost all of its descriptive effort in defining the media type(s) used for representing resources and driving application state. Any effort spent describing what methods to use on what URIs of interest should be entirely defined within the scope of the processing rules for a media type (and, in most cases, already defined by existing media types). 

Smell: when a protocol calls for the use of a generic media type (like application/xml or application/json) and then requires that it be processed in a way that is special to the protocol/API.

## Implementation

### Component-connector
Connector does the HTTP part, component the remainder. Do no mix!

### Resources
Resources are not storage items (or, at least, they aren’t always equivalent to some storage item on the back-end). The same resource state can be overlayed by multiple resources, just as an XML document can be represented as a sequence of bytes or a tree of individually addressable nodes.

A resource is a conceptual mapping to a set of entities. Requests and responses have the appearance of a remote invocation style, but REST messages are targeted at a conceptual resource rather than an implementation identifier.

REST works well because it does not limit the implementation of resources to certain predefined models, allowing each application to choose an implementation that best matches its own needs and enabling the *replacement* of implementations without impacting the user. Which brings us to...

### Micro-services
One of the major goals of REST is to support the gradual and fragmented deployment of changes within an already deployed architecture.

Vert.x example?

## Conclusion
