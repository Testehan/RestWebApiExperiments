## Introduction
* Of course, Twitter, Facebook, and Google are big companies that compete with each
  other. They don’t want to make it easy for you to learn their competitors’ APIs. But small
  companies and nonprofits do the same thing. They design their APIs as though nobody
  else had ever had a similar idea. This interferes with their goal of getting people to
  actually use their APIs.
* When navigating the forest of standards, it’s useful to keep in mind that not all standards
  have equal force. Some are extremely well established, used by everyone, and if you go
  against them you’re causing a lot of trouble for yoursef. Other standards are just one
  person’s opinion, and that opinion might be no better than yours.
  I find it helpful to divide standards into four categories: fiat standards, personal stand‐
  ards, corporate standards, and open standards. I’ll be using these terms throughout the
  book, so let me explain each one in a bit more depth before we move on.
* Designing a new API today means reinventing a long series of wheels. Once your API
  is finished, your client developers have to reinvent corresponding wheels on the client side.
  Even under ideal circumstances, your API will be a fiat standard, since your business
  requirements will be slightly different from everyone else’s. But ideally a fiat standard
  would be just a light gloss over a number of other standards.

## 1. Surfing the web
* Resources = The URL http://www.youtypeitwepostit.com/ identifies a resource—probably the home
  page of the website advertised on the billboard
* Representations = When a web browser sends an HTTP request for a resource, the server sends a document
  in response (usually an HTML document, but sometimes a binary image or something
  else). Whatever document the server sends, we call that document a representation of the resource.
* So each URL identifies a resource. When a client makes an HTTP request to a URL, it
  gets a representation of the underlying resource. The client never sees a resource directly
* This principle is sometimes called statelessness. I think this is a confusing term because
  the client and the server in this system both keep state; they just keep different kinds of
  state. The term “statelessness” is getting at the fact that the server doesn’t care what state
  the client is in.
* Application state = Every state in this diagram corresponds to a particular page (or to no page at all) being
  open in Alice’s browser window. In REST terms, we call this bit of information—which
  page are you on—the application state.
*  Resource State=When the story begins, there are two messages in the message list: “Hello” and “Later.”
   Sending a GET to the home page doesn’t change resource state, since the home page is
   a static document that never changes. Sending a GET to the message list won’t change the state either.
   But when Alice sends a POST to the message list, it puts the server in a new state. Now
   the message list contains three messages: “Hello,” “Later,” and “Test.”
* Connectedness = The strands of the web are the HTML \<a\> tags and \<form\>
  tags, each describing a GET or POST HTTP request Alice might decide to make. I call
  this the principle of connectedness: each web page tells you how to get to the adjoining pages.
* The Web as a whole works on the principle of connectedness (see above), which is better known as
  “hypermedia as the engine of application state,” sometimes abbreviated HATEOAS. I
  prefer “connectedness” or “the hypermedia constraint,” because “hypermedia as the en‐
  gine of application state” sounds intimidating. But at this point, you should have no
  reason to find it intimidating. You know what application state is—it’s which web page
  a client is on. Hypermedia is the general term for things like HTML links and forms:
  the techniques a server uses to explain to a client what it can do next.
  To say that hypermedia is the engine of application state is to say that we all navigate
  the Web by filling out forms and following links.
* Web APIs Lag Behind the Web
* * Web APIs frequently have human-readable documentation that explains how to
  construct URLs for all the different resources. This is like writing English prose
  explaining how to find a particular file on an FTP server. If websites did this, no
  one would bother to use the Web.
  Instead of telling you what URLs to type in, websites embed URLs in \<a\> tags and
  \<form\> tags—hypermedia controls that you can activate by clicking a link or a button.
  In REST terms, putting information about URL construction in separate human readable documents violates the
  principles of connectedness and self-descriptive messages.
* * Lots of websites have help docs, but when was the last time you used them? Unless
    there’s a serious problem (you bought something and it was never delivered), it’s
    easier to click around and figure out how the site works by exploring the connected,
    self-descriptive HTML documents it sends you.
    Today’s APIs present their resources in a big menu of options instead of an inter‐
    connected web. This makes it difficult to see what one resource has to do with another.
* * Integrating with a new API inevitably requires writing custom software, or instal‐
    ling a one-off library written by someone else. But you don’t need to write custom
    software to use a new website. You see a URL on a billboard and plug it into your
    web browser—the same client you use for every other website in the world.
* * When APIs change, custom API clients break and have to be fixed. But when a
    website undergoes a redesign, the site’s users grumble about the redesign and then
    they adapt. Their browsers don’t stop working.
    In REST terms, the website redesign is entirely encapsulated in the self-descriptive
    HTML documents served by the website. A client that could understand the old
    HTML documents can understand the new ones

## 2. A Simple API
* The ideal API would have the same characteristics that make the World Wide Web easy
  to use. As a developer, you would be able to figure out how to use it, starting with nothing
  but a URL you saw on a billboard.
* How to Read an HTTP Response ? Well, every HTTP response can be split into three parts:
* * The status code, sometimes called the response code
    This is a three-digit number that summarizes how the request went. The response
    code is the first thing an API client sees, and it sets the tone for the rest of the
    response. Here, the status code was 200 (OK). This is the status code a client hopes
    for—it means that everything went fine.
* * The entity-body, sometimes called just the body
    This is a document written in some data format, which the client is expected to
    understand. If you think of a GET request as a request for a representation, you can
    think of the entity-body as the representation (technically, the entire HTTP response is the
    ‘representation’, but the important information is usually in the entitybody).
* * The response headers 
    These are a series of key-value pairs describing the entity-body and the HTTP re‐
    sponse in general. Response headers are sent between the status code and the entitybody
* The most important HTTP header is Content-Type, which tells the HTTP client
  how to understand the entity-body. It’s so important that its value has a special
  name. We say the value of the Content-Type header is the entity-body’s media
  type. (It’s also called the MIME type or the content type. Sometimes “media type” is
  hyphenated: media-type.)
  (example : the most common media types are text/html (for HTML) and image types like image/jpeg)
* Collection+JSON is a way of serving lists—not lists of data structures, which you can
  do with normal JSON, but lists that describe HTTP resources
* A document that doesn’t follow these rules isn’t a Collection+JSON document: it’s just
  some JSON. By allowing yourself to be bound by Collection+JSON’s constraints, you
  gain the ability to talk about concepts like resources and URLs. These concepts are not
  defined in JSON, which can only talk about simple things like strings and lists.
* If the publishers of microblogging APIs got together and agreed to use a common set
  of application semantics, the semantic gap for microblogging would disappear almost
  entirely. (This would be a profile, and I’ll cover this idea in Chapter 8.) The more con‐
  straints we share and the more compatible our designs, the smaller the semantic gap
  and the more our users benefit.

## 3. Resources and representations
* REST is not a protocol, a file format, or a development framework. It’s a set of design
  constraints: statelessness, hypermedia as the engine of application state, and so on. Col‐
  lectively, we call these the Fielding constraints, because they were first identified in Roy
  T. Fielding’s 2000 dissertation on software architecture, which gathered them together
  under the name “REST.
* From the client’s perspective, it doesn’t matter what a resource is, because the client
  never sees a resource. All it ever sees are URLs and representations.
* A representation can be any machine-readable document containing any information about a resource.
* We think of representations as something the server sends to the client. That’s because
  when we surf the Web, most of our requests are GET requests. We’re asking for repre‐
  sentations. But in a POST, PUT, or PATCH request, the client sends a representation to
  the server. The server’s job is then to change the resource state so it reflects the incoming
  representation
* The server sends a representation describing the state of a resource. The client sends a
  representation describing the state it would like the resource to have. That’s represen‐
  tational state transfer (REST).
* The Protocol Semantics of HTTP
* * GET - Get a representation of this resource.
* * DELETE - Destroy this resource.
* * POST - Create a new resource underneath this one, based on the given representation.
* * PUT - Replace this state of this resource with the one described in the given representation.
* * HEAD - Get the headers that would be sent along with a representation of this resource, but
    not the representation itself.
* * OPTIONS - Discover which HTTP methods this resource responds to.
* Sending a GET request to the server should have the same effect on re‐
  source state as not sending a GET request—that is, no effect at all. Incidental side effects
  like logging and rate limiting are OK, but a client should never make a GET request
  hoping that it will change the resource state.
* idempotence = Sending a request twice has the same effect on resource state as sending it once.
  The notion of idempotence comes from math. Multiplying a number by zero is an
  idempotent operation. 5 × 0 is zero, but 5 × 0 × 0 is also zero. 
  Multiplying by 1 is a safe operation, the way HTTP GET is supposed to be safe.
* OPTIONS is a good idea, but almost nobody uses it. Well-designed APIs advertise a
  resource’s capabilities by serving hypermedia documents (see Chapter 4) in response to
  GET requests. The links and forms in those documents explain what HTTP requests a
  client can make next. Poorly designed APIs use human-readable documentation to ex‐
  plain which HTTP requests a client can make.
* Now it’s time to reveal the skeleton in the HTTP closet. The HTTP POST method has
  a dirty secret, one you’ve certainly encountered if you’ve ever worked in web develop‐
  ment. POST is not solely used to create new resources. On the Web we surf with our
  browsers, HTTP POST is used to convey any kind of change. It’s PUT, DELETE, PATCH,
  LINK, and UNLINK all rolled into one.
* In terms of protocol semantics, this operation—“edit this blog post”—sounds like a PUT
  request. But an HTML form can’t trigger a PUT request. The HTML data format doesn’t
  allow it. So we use POST instead.
* The definition is so vague that a POST request really has no protocol semantics at all. POST doesn’t
  really mean “create a new resource”; it means “whatever!”
  I call this “whatever!” usage of POST overloaded POST. Because an overloaded POST
  request has no protocol semantics, you can only understand it in terms of its application
  semantics.
* The methods I recommend for use in most web APIs are GET, POST, PUT, DELETE,
  and PATCH(?..Before 2008, the PATCH method didn’t exist.)

## 4. Hypermedia
* The missing piece of the puzzle is hypermedia. Hypermedia connects resources to each
  other, and describes their capabilities in machine-readable ways. Properly used, hyper‐
  media can solve—or at least mitigate—the usability and stability problems found in
  today’s web APIs.
* Like REST, hypermedia isn’t a single technology described by a standards document
  somewhere. Hypermedia is a strategy, implemented in different ways by dozens of technologies
* The hypermedia strategy always has the same goal. Hypermedia is a way for the server
  to tell the client what HTTP requests the client might want to make in the future. It’s a
  menu, provided by the server, from which the client is free to choose. The server knows
  what might happen, but the client decides what actually happens.
* To sum up, the familiar HTML controls allow the server to describe four kinds of HTTP requests.
* * The /<a/> tag describes a GET request for one specific URL, which is made only if
  the user triggers the control.
* * The /<img/> tag describes a GET request for one specific URL, which happens auto‐
  matically, in the background.
* * The /<form/> tag with method="POST" describes a POST request to one specific URL,
  with a custom entity-body constructed by the client. The request is only made if
  the user triggers the control.
* * The /<form/> tag with method="GET" describes a GET request to a custom URL con‐
  structed by the client. The request is only made if the user triggers the control.
* Fielding (guy who first wrote about REST) dissertation:
  Hypermedia is defined by the presence of application control information embedded
  within, or as a layer above, the presentation of information.
* URI Versus URL ..What is the difference ?  As far as this book is concerned, the difference is this: there’s no
  guarantee that a URI has a representation. A URI is nothing but an identifier. A URL is
  an identifier that can be dereferenced. That is, a computer can somehow take a URL and
  get a representation of the underlying resource.  Here’s a URI that’s not a URL: urn:isbn:9781449358063. It designates a resource: the
  print edition of this book.
* Without a URL, you can’t get a representation. Without representations, there can be
  no representational state transfer. In general, when your web API refers to a resource, it should use a URL with the http
  or https scheme, and that URL should work: it should serve a useful representation in
  response to a GET request.
* That’s why API designers shouldn’t design APIs that serve plain JSON. You should use
  a media type that has real support for hypermedia. Your users will thank you. They’ll
  be able to use preexisting libraries written against the media type, rather than writing
  new ones specifically for your API.
* Providing the links is a step in the right direction. Out of the infinite set of legal HTTP
  requests, a hypermedia document explains which requests might be useful right now,
  on this particular site. The client doesn’t have to guess.
* A hypermedia format doesn’t have to be generic like HTML. It can be defined in enough
  detail to convey the application semantics of a wiki or a store. In the next chapter, I’ll
  talk about hypermedia formats that are designed to represent one specific type of prob‐
  lem. Outside that problem space, they’re practically useless. But within their limits, they
  meet the semantic challenge very well.

## 5. Domain specific designs
* Although this is a frivolous example, the maze is a good metaphor for hypermedia
  applications in general. Any complex problem can be represented as a hypermedia maze
  that the client must navigate. If you’ve ever been trapped in a phone tree, or searched
  for products on an online store and then bought something from the search results,
  you’ve navigated a hypermedia maze.
* A link relation is a magical string associated with a hypermedia control like Maze+XML’s
  \<link\> tag. It explains the change in application state (for safe requests) or resource
  state (for unsafe requests) that will happen if the client triggers the control
* One of the most important web pages for a RESTful API developer is the registry of link
  relations managed by the Internet Assigned Numbers Authority (IANA).
  http://www.iana.org/assignments/link-relations/link-relations.xhtml
  I’ll be coming back to this registry throughout the book. It contains about 60 link relations that have
  been deemed to be generally useful and not tied to a particular data format. The simplest
  examples are the "next" and "previous" relations, for navigating a list.
* Chapter 9 includes a guide explaining when it’s OK to use the shorter names of registered
  relations. Here’s a summary:
* * You can use extension relations wherever you want.
* * You can use IANA-registered link relations whenever you want.
* * If a document’s media type defines some registered relations, you can use them
  within the document.
* * If a document includes a profile that defines some link relations (see Chapter 8),
  you can treat them as registered relations within that document.
* * Don’t give your link relations names that conflict with the names in the IANA
  registry
  (this refer to the string that is put in the "rel" attribute of the "link" html element)
  https://www.w3schools.com/tags/tag_link.asp
*  The URL to the collection of mazes is the proverbial “URL advertised on the billboard.”
   Starting with no information but this URL, you can do everything it’s possible to do
   with a Maze+XML API:
   1. Start off by GETting a representation of the collection of mazes. You know how to
   parse the representation, because you read the Maze+XML specification and pro‐
   grammed this knowledge into your client.
   2. Your client also knows that the link relation maze indicates an individual maze. This
   gives it a URL it can use in a second GET request. Sending that GET request gives
   you the representation of an individual maze.
   3. Your client knows how to parse the representation of an individual maze (because
   you programmed that knowledge into it), and it knows that the link relation start
   indicates an entrance into the maze. You can make a third GET request to enter the
   maze.
   4. Your client knows how to parse the representation of a maze cell. It knows what
   east, west, north, and south mean, so it can translate movement through an ab‐
   stract maze into a series of HTTP GET requests.
   5. Your client knows what exit means, so it knows when it’s completed a maze.
* Is Maze+XML an API?
  If you’ve got experience in this field, you may be wondering: where’s the API? A maze
  game isn’t a complex application, but even so, you may have expected more than a few
  XML tag names and link relations. The Maze+XML specification lacks the things you
  may be accustomed to. It doesn’t define any API calls or give any rules for constructing
  URLs. In fact, it barely mentions HTTP at all! I’ve shown some URLs in the example
  representations, but I deliberately made the URL formats internally inconsistent (com‐
  pare /beginner to /expert-maze/start) so you wouldn’t think URL formats were defined
  by the standard.
* But this book focuses on web APIs, which is to say, web-scale APIs (i.e., APIs where any
  member of the public can use a client, or write a client, or, in some cases, write a server).
  When you allow someone outside your organization to make API calls, you make that
  person a silent partner in the implementation of your server. It becomes very difficult
  to change anything on the server side without hurting this unknown customer
* Designs based on hypermedia have more flexibility. Every time the client makes an
  HTTP request, the server sends a response explaining which HTTP requests make the
  most sense as a next step. If the server-side options change, that document changes
  along with it. This doesn’t solve all of our API problems—the semantic gap is a huge
  problem!—but it solves the one we know how to solve.
* Imagine starting up a web browser that’s only ever been tested against one particular
  website. As soon as you send that browser to a site it wasn’t tested on, it’s going to crash.
  That’s the situation here. A standard like Maze+XML may have multiple server imple‐
  mentations. Client implementations need to be designed to work against all server im‐
  plementations, not just one.
* The similarity is no coincidence. As I said at the beginning of this chapter, the maze is
  a metaphor for hypermedia applications in general. Some “mazes” are tidy and wellbehaved. Others are chaotic
  and infinitely large. Thinking of a state diagram as a maze
  to be navigated will get you in the right frame of mind to understand hypermedia APIs.
* But just because a data format doesn’t include hypermedia controls doesn’t mean it’s
  useless. In Chapter 8, I’ll show you how JSON-LD can add basic hypermedia capabilities
  to any JSON format. In Chapter 10, I’ll show how XForms and XLink can do the same
  for XML. These technologies let you graft hypermedia controls onto an existing API
  that doesn’t include them.
* I showed in Chapter 4 how you can use the HTTP header Link to add simple hypermedia
  links and forms to documents that have no hypermedia controls of their own. Using
  these headers, you could conceivably design an API that served nothing but JPEG im‐
  ages, but I don’t recommend this.
* This is another reason why it’s important to look for domain-specific data formats before
  you set off to design your API. A standard like vCard represents a lot of time and money
  spent identifying the application-level semantics for a problem domain. You don’t need
  to start over just because vCard doesn’t have hypermedia controls
*  A hypermedia-aware script is less likely to break when something trivial happens, like
   the URL of a resource changing or new data being added to a representation. This means
   a hypermedia API has some room to change without breaking the scripts that depend
   on it. But a script is a playback of a human being’s thought process. If it encounters a
   situation the human didn’t originally consider, the script won’t be able to fill in the blanks.

## 6. The collection pattern
* Collection+JSON is one of several standards designed not to represent one specific
  problem domain (the way Maze+XML does), but to fit a pattern—the collection—that
  shows up over and over again, in all sorts of domains. This standard makes a good
  example, because it’s a formalized version of the JSON-based APIs that first-time de‐
  signers tend to come up with. Collection+JSON lets you follow your natural design
  inclinations without running afoul of the Fielding constraints.
* If there’s no domain-specific standard for your problem domain (and there probably
  isn’t), you may be able to use a collection-based standard instead. Instead of starting
  from nothing, you’ll be able to focus on adapting your application semantics to the
  collection pattern. Not only will you save time, you’ll get access to a preexisting base of
  client programs and server-side tools.
* A collection resource is a little more specific than that. It exists mainly to group other
  resources together. Its representation focuses on links to other resources, though it may
  also include snippets from the representations of those other resources. (Or even the
  full representations!)
* (See book notes for a nice description of Collection+JSON)
* There are a number of generic link relations for navigating paginated lists. These include
  "next", "previous", "first", "last", and "prev" (which is a synonym for “previous”).
  These link relations were originally defined for HTML, but now they’re registered with
  the IANA, so you can use them with any media type
* Google, the biggest corporate adopter of AtomPub, uses Atom
  documents to represent videos, calendar events, cells in a spreadsheet, places on a map,
  and more.
* But the AtomPub story also shows that “nothing wrong with the standard” isn’t good
  enough. People won’t go through the trouble of learning a standard unless it’s directly
  relevant to their needs. It’s easier to reinvent the “collection” pattern using a fiat standard
  based on JSON, so that’s what thousands of developers did—and continue to do
* But an "item" still isn’t anything in particular. It’s almost as vague a term as “resource.”
  In a microblogging API, an "item" will be a bit of text with a timestamp. In a payment
  processor, an "item" will include a creditor, a debitor, a method of payment, and an
  amount of money. There’s still an enormous gap between the application semantics of
  the collection pattern and the application semantics of your individual API.

## 7. Pure hypermedia designs
* Nothing says you have to use the collection pattern, but it is the most popular design
  pattern for APIs. If you want to implement some other pattern, or if your API design
  doesn’t fit any particular pattern, you can describe an API’s semantics using pure hy‐
  permedia. You don’t have to create an entirely new standard like Maze+XML, with its
  own media type. You can represent the state of your resources using a generic hypermedia language.
* HTML has distinct advantages even for an API designed to be consumed entirely by
  machines. HTML imposes more structure on a document than XML or JSON does, but
  not so much structure as to solve only one specific problem, the way Maze+XML does.
  HTML sits somewhere in the middle, like Collection+JSON.
* More important, HTML has built-in hypermedia controls. I mentioned these controls
  back in Chapter 4, but just to recap, here are the most important ones:
* * The /< link /> tag and /< a /> tag are simple outbound links, like the /< link /> tag in Maze
  +XML. They tell the client to make a GET request to a specific URL in order to get
  a representation. That representation becomes the current view.
* * The /< img /> tag and /< script /> tags are embedding links. They tell the client to au‐
  tomatically make a GET request to another resource, and to embed the represen‐
  tation of that resource in the current view.
* * When the /< form /> tag has the string "GET" as its method attribute (i.e. /< form meth
  od="GET">), it acts as a templated outbound link. This works like a URI Template,
  or the queries slot in Collection+JSON. The server provides the client with a base
  URL and some input fields (HTML /< input /> tags). The client plugs in values for
  those fields, combines them with the base URL to form a one-of-a-kind destination
  URL, and makes a GET request to that URL.
* * When the /<form/> tag has "POST" as its method attribute, it describes an HTTP POST
  request that can do anything at all. The /< input /> tags are still present, but instead
  of being used to create the request URL, they’re used to create an entity-body with
  the media type application/x-www-form-urlencoded. The request URL is hard‐
  coded in the action attribute of the <form> tag
* hCard is a microformat: a lightweight industry standard defined through informal collaboration on a
  wiki, rather than through the formal IETF process that results in RFCs.
* Microdata is a refinement of the microformat concept for HTML 5. You see, microfor‐
  mats are kind of a hack. HTML’s class attribute was designed to convey information
  about visual display (via CSS), not to convey bits of application semantics
* The main source of microdata items is schema.org, a project of four big search engines
  (Bing, Google, Yahoo!, and Yandex) to define application semantics for different prob‐
  lem domains. Search engines have an interest in understanding the high-level applica‐
  tion semantics of a web page—that is, whatever real-world thing the web page is talking
  about.
* you’ll start seeing examples that refer to schema.org microdata items. To a
  human being, it should be pretty obvious what they mean. The URL http://schema.org/Person
  points to the schema.org microdata item corresponding to our everyday notion
  of a “person.”
* What exactly should the HTML documents look like? I didn’t define this because I
  don’t care. Your hMaze implementation can serve full human-readable documents
  with lots of flavor text, or it can serve very compact documents optimized for au‐
  tomated clients. As long as you use the hMaze CSS classes and link relations cor‐
  rectly, your choice will have no effect on the application semantics of the maze.
* If you find yourself writing up (or generating) documentation like this example, you’re
  using human-readable documentation as a substitute for hypermedia. That’s unacceptable.
  You’re creating useless work for yourself and your users.
*  I don’t have to provide a template for constructing the action URL, or force the client
   to reckon with my internal concept of a “switch ID,” because the <form> tag for flipping
   a switch includes the actual URL the client should use. I don’t have to make caveats like
   “You can only flip a switch if you’re in the same cell as the switch,” because hypermedia
   controls are presented only when they can be used. If the submit button isn’t there, the
   state transition isn’t available
* I used to think that you should design APIs by identifying the resources and tying them
  together with hypermedia. This resource-oriented approach is good advice when you’re
  trying to move away from publishing all your internal methods as a huge list of API
  calls. Thinking in terms of resources will at least group the API calls together in sensible ways.
* But in a hypermedia-based design, resources don’t matter as much. The designer’s job
  is to identify all the state transitions. A resource-oriented design would focus heavily
  on the mysterious switch as a resource, as a thing in itself. But the switch itself isn’t all
  that important. My design focuses on the state transition, on what you can do with the
  switch.
* HTML includes a lot of hypermedia controls, but the controls can’t describe all of
  HTTP’s protocol semantics. There’s no way to tell an HTML client to make a PUT
  or DELETE request without using JavaScript.
* This is why I think HAL strips too much away from HTML. How are you supposed to
  know which of the infinite possibilities are present in a given link? The <link> tag with
  rel="east" should trigger a GET request that gives you a representation of the cell to
  the east. The /< link /> tag with rel="flip" should trigger a POST request that flips the
  switch. One of them is a safe operation that modifies application state; the other is an
  unsafe, non-idempotent operation that modifies resource state. In HAL, those two links
  look almost identical. The only real difference is the link relation.
* Siren - I’ll close out this chapter by taking a brief look at another general hypermedia format: Siren.
  Siren is a newer format than HAL, and although it’s based on JSON, it takes a
  more HTML-like approach to hypermedia than HAL’s minimalism.
* Siren is designed to represent abstract groupings of data it calls entities. A Siren “entity”
  is conceptually similar to HTML’s < di v> tag. It’s a convenient way of splitting up your
  data. An entity may be an HTTP resource with its own URL, but it doesn’t have to be.
* The Semantic Challenge: How Are We Doing?
* * We have a client-server Internet protocol, HTTP, which assigns very general meanings to different kinds of
requests: GET, POST, PUT, and so on.
* * We have the idea of hypermedia, which allows the server to tell the client which HTTP
requests it might want to make next. This frees the client from having to know the shape
of the API ahead of time. 
* * And we have a whole lot of standards for building APIs

## 8. Profiles
## 9. The design procedure
## 10. The Hypermedia zoo
## 11. HTTP for APIs
## 11. Resource description and linked data
## 12. CoAP: REST for embedded systems
