# The "podcast" Namespace

A wholistic rss namespace for podcasting that is meant to synthesize the fragmented world of podcast namespaces.  The broad goal is to create a single, compact, efficient
namespace that is easily extensible, community controlled/authored and addresses the needs of the independent podcast industry now and in the future.
Our hope is that this namespace
will become the framework that the independent podcast community needs to deliver new functionality to apps and aggregators.


<br><br>


## Current Roadmap

**Phase 1** - Comment period closes on `11/15/20` and tags that have good consensus will be adopted.  Any tags with questions, concerns or no discernable use case will be either
              removed or booted to Phase 2.

**Phase 2** - This phase is currently open for tag proposals and ideas.


<br><br>


## Tag Adoption Process

To be adopted as an official part of the namespace, there must be consensus around a tag's usefulness and either commitment to adoption by at least 1 host and 1 app, or a recognition
that the tag is already being used in the wild.

It is ALWAYS ok to delay a tag to a future Phase if there is any concern about it.  That is to be expected and encouraged.

When a Phase comes to a close, there will be a full review of any tags currently open for comment and questions will be asked to gather consensus before final adoption.  No tags
will be adopted by fiat, or if there are unresolved questions.  They will just be moved to the next Phase for further comment and refinement.

Tags that are proposals or rough ideas should be expected to have syntax problems or typos.  Those should be refined away as they are worked on.  If they are not, that is a good idea
that the tag in question isn't being seen as useful and should be considered for dropping.

We are not a "standards body".  It is a community driven project where all stake holders are encouraged to participate, so that many voices are heard.  This is an open-source
project to be built fully in the open.  Discussions also take place on [podcastindex.social](https://podcastindex.social) where anyone is free to register and participate.


<br><br>


## Goal #1 - Eliminate Redundancy

There is significant overlap amongst the many existing podcast namespaces.  Each platform and publisher has created their own namespace to give their respective
system and audience the metadata they need in the way they want it delivered.


## Goal #2 - Keep "required" tags and attributes minimal

The only required tags should be those that solve an overwhelming need in the industry.  Requiring tags is a roadblock to adoption and should be avoided.  Attributes
should also only be required when they are key to the functionality of the tag.


## Goal #3 - Keep Exisiting Conventions

Reinventing the wheel helps nobody.  When at all possible, existing conventions should be maintained.  For example, it would make sense to turn **\<podcast:explicit>** into
a unary element, where it's existence is taken as a "yes" and it's absence as a "no".  But, that has never been the standard.  And, given as how this namespace will probably
sit alongside at least one other namespace, it makes sense to keep existing conventions in place.


## Goal #4 - Be General... to a point

There is no way to address every possible metadata point that each platform would want.  That is not the aim.  Instead we focus on defining the elements that would be useful
to the broadest set of apps, publishers, platforms and aggregators.  Individual parties can keep their respective supplemental namespaces small and targeted as an adjunct to
this larger namespace.  But, we don't want to be so general that the spec becomes overly complicated.  A beautiful, "perfect" spec is not important.  Solving real problems is.


## The Guiding Principles

Our guiding principles for development of this namespace are the "[Rules for Standards Makers](http://scripting.com/2017/05/09/rulesForStandardsmakers.html)" by Dave Winer.
Please read it before contributing if you aren't familiar with it.


## Official XMLNS Definition

To see the formalized tags, the official definition file is [here](docs/1.0.md).


## Supporting Platforms and Apps

To see a list of platforms and apps that currently implement some or all of these tags, see the list [here](docs/element-support.md).


## Example Feed

There is an example feed [example.xml](example.xml) in this repository showing the podcastindex namespace side by side with the Apple itunes namespace.  There is also
a "sandbox" feed where we experiment with tags while they are being hashed out.

The url for that feed is:  [https://noagendaassets.com/enc/pc20sandbox.xml](https://noagendaassets.com/enc/pc20sandbox.xml).

<br><br>

## Element List

### <u>Phase 1 (Open)</u>

<br>

- **\<podcast:locked owner="[podcast owner email address]">**[yes or no]**\</podcast:locked>** (formalized)

   Channel

   (required | single)

   This tells other podcast platforms whether they are allowed to import this feed.  A value of "yes" means that any attempt to import
   this feed into a new platform should be rejected.  It is expected that podcast hosting providers will enable a toggle in their GUI to allow their users to turn
   feed transfer lock on or off.

   The owner attribute is an email address that can be used to verify ownership of this feed during move and import operations.  This could be a public email or a
   virtual email address at the hosting provider that redirects to the owner's true email address.  This is a critical element, and it's expected that podcast
   hosting providers (if not providing virtual addresses) will allow setting this element's value in their GUI with an emphasis to their users of how important
   it is to have this be a valid, working email address.

   When importing a feed, if the hosting provider has already verified the owner="" email address on their own system, and the email matches what is listed in this
   tag, it is safe to import the feed.  This could be useful in a situation where the owner's account at the previous hosting company is locked out or for some other
   reason inaccessible to the owner for disabling the lock.

   Attributes:

   - `owner` (required) The email address of the podcast owner.

<br>

- **\<podcast:transcript url="[url to a file or website]" type="[mime type]" rel="captions" language="[language code]" />** (formalized)

   Item

   (optional | multiple)

   Links to an external file containing a transcript or closed captions file, which may actually be the same file.  The mime type of the file should be given - such
   as `text/plain`, `text/html`, `application/srt`, `application/json`.  If there is no language attribute given, the linked file is assumed to be the same language that is
   specified by the RSS \<language\> element.  If the rel="captions" attribute is present, the linked file is considered to be a closed captions file, regardless of what the
   mime type is.  In that scenario, time codes are assumed to be present in the file in some capacity.

   Detailed file format information and example files are [here](transcripts/transcripts.md).

<br>

- **\<podcast:funding url="[url for the show at the platform]">**[user provided content to link]**\</podcast:funding>** (formalized)

   Channel or Item

   (optional | multiple)

   This element lists multiple possible donation/funding links for the podcast.

   Attributes:

   - `url` (required) Full url to the specific show on the funding platform.
   - `node value` (required) Used as free form string from the podcast owner to show to the listeners.  Ex. "Support us on Patreon!"

   The maximum recommended string character length of the node value is 128 characters.


<br>

- **\<podcast:chapters url="[url to chapter data file]" type="[mime type]" />**

   Item

   (optional)

   Links to an external file (see example file) containing chapter data for the episode. The mime type of the file should be given - JSON prefered, `application/json`.  See
   the [jsonChapters.md](chapters/jsonChapters.md) file for a description of the chapter file syntax.  And, see the [example.json](chapters/example.json) example file for
   a real world example.

<br>

- **\<podcast:person role="[host or guest]" img="[(uri of content)]" href="[(uri to website/wiki/blog)]">**[name of person]**</podcast:person>**

   Channel or Item (optional | multiple)

   This element specifies a person of interest to the podcast.

   - `name` (required) This is the full name or alias of the person.
   - `role` (optional) Used to identify what role the person has for the show or episode. Currently there are two defined roles: "host" or "guest". If role is missing then "host" is assumed.
   - `img` (optional) This is the url of a picture or avatar of the person.
   - `href` (optional) Link to a relevant resource of information about the person. (eg. website, blog or wiki entry).

   The maximum recommended string length of the node value is 128 characters.

<br>

- **\<podcast:soundbite startTime="[123]" duration="[30]">[Title of Soundbite]\</podcast:soundbite>**

   Item

   (optional | multiple)

   See "ID's" in this document for an explanation.

   - `startTime` (required) The time where the soundbite begins
   - `duration` (required) How long is the soundbite (recommended between 15 and 120 seconds)
   - `node value` (optional) Used as free form string from the podcast creator to specify a title for the soundbite (otherwise default to episode title)

<br><br>


### <u>Phase 2 (proposals)</u>

- **\<podcast:location country="[Country Code]" locality="[Locality]" latlon="[latitude,longitude]" (osmid="[OSM type][OSM id]") />**

   Channel (optional | single)

   Item (optional | multiple)

   This element must contain, at minimum, a latlon point and a country code.  Although, an OSM specification is highly recommended.  The purpose of this tag is to allow specifying
   locations relevant to this podcast or episode.  It can be used as simply as a single tag in the channel to show the "home base" of where a podcast is produced.  Or, more thoroughly
   for a travel podcast to specify locations that were visited during each episode.  Also, a history podcast could reference points of interest discussed.  There are many possible uses.

    - latlon: (required) A latitude/longitude point reflecting the location associated with this show or episode. This could be where it is made, or alternatively a location which
                         features in the podcast.
    - osmid: (recommended) From the OpenStreetMap API. If a value is given for osmid it must contain both 'type' and 'id'.
        - osm type: A one-character description of the type of OSM point. Valid is "N" (node); "W" (way); "R" (relation).
        - osm id: The ID of the OpenStreetMap feature that is described. This may be a city or a building. While OSM IDs are not considered permanent, cities rarely disappear.
    - CountryCode: (required) The ISO 3166-1 alpha-2 country code, eg 'US'. (Note that the United Kingdom is 'GB', not 'UK'.)
    - Locality: (recommended) With a pipe separator from the countrycode, this is a humanly-readable place name as preferred by the podcast publisher.

   The maximum recommended string character length of the node value is 128 characters.

<br>

- **\<podcast:social platform="[service slug]" url="[link to social media account]">**[social media handle]**\</podcast:social>**

   Channel or Item

   (optional | multiple)

   This element lists social media accounts for this podcast.  The service slugs should be community written into the accompanying serviceslugs.txt file.

   The maximum recommended string length of the node value is 128 characters.

<br>

- **\<podcast:category>**[category Name]**\</podcast:category>**

   Channel

   (optional | multiple)

   See "Categories" in this document for an explanation.  There can be up to a total of 9 categories defined.

   There can be a maximum of 9 category elements defined in a feed.  Any number greater than that should be discarded.

   Category names are defined in the accompanying "categories.json" file in this repository.  They should be referenced in the element by their textual name.
   The characters can be in any case.  This list of categories aims to replicate the current standard but also eliminate as much as possible compound, heirarchical
   naming and the use of ampersands.  Thus, "Health & Fitness" becomes "Health" and "Fitness" as two distinct categories.  And, "Religion & Spirituality" becomes
   two separate categories.  Again, they are different things that don't always go together.  Splitting them allows for more flexible combinations.  And, avoiding
   ampersands makes xml encoding errors less likely.

<br>

- **\<podcast:contentRating>**[rating letter]**\</podcast:contentRating>**

  Channel or Item

  (optional | single)

  Specifies the generally accepted rating letter of G, PG, PG-13, R or X.  Or, perhaps an age rating system like all, 14, 19, adult.  Needs discussion.

<br>

- **\<podcast:previousUrl>**[url this feed was imported from]**\</podcast:previousUrl>**

   Channel

   (optional | multiple)

   Lists the previous url of this feed before it was imported.  Any time a feed is moved, an additional **\<podcast:previousUrl>** element
   should be added to the channel, to create a paper trail of all the previous urls this feed has lived at.  This way, aggregators can easily deduplicate their feed lists.

<br>

- **\<podcast:alternateEnclosure url="[url of media asset]" type="[mime type]" length="[(int)]" bitrate="[(float)]" title="[(string)]" rel="[(string)]" />**

   Channel (optional | single)

   Item (optional | multiple)

   This element is meant to provide alternate versions of an enclosure, such as low or high bitrate, or alternate formats or alternate uri schemes, like IPFS or live streaming.
   There may be multiple alternateEnclosure elements in an item, but there must be no more than one in a channel.  The presence of this element at the channel level would be
   useful for adding a video/audio trailer or intro media that introduces the listener to the podcast.  For instance, in a podcast of an audiobook, this could be the book's
   introduction or preface.  The alternateEnclosure element always refers to an "alternate" media version.  The standard RSS enclosure element is always the default media to be played.

   An `<enclosure>` tag must be present along with this tag within the item.

   - `url` (required) This is the url to the media asset.
   - `type` (required) Mime type of the media asset.
   - `length` (required) Length of the file in bytes.
   - `bitrate` (optional) Encoding bitrate of media asset.
   - `title` (required) Alternate assets need a title since main title will apply to primary asset.
   - `rel` (optional) Indicates what the purpose of this enclosure is. Like "lowbitrate" for a small file to use over cellular.

<br>

- **\<podcast:indexers>** + **\<podcast:block>[domain, bot or service slug]\</podcast:block>**

   Channel (optional | single)

   The "indexers" element is meant as a container for one or more `<podcast:block>` elements which send a signal to podcast aggregators whether they are allowed to pull and parse
   this feed.  If the aggregator is listed as blocked, it should take that as a signal by the feed owner to not index/aggregate this feed.

   *Note: this element needs a lot more discussion and work.  This is just a placeholder for discussion.

<br>

- **\<podcast:images srcset="[url to image] [pixelwidth(int)]w,
                             [url to image] [pixelwidth(int)]w,
                             [url to image] [pixelwidth(int)]w,
                             [url to image] [pixelwidth(int)]w" />**

   Channel or Item

   (optional | single)

   This points to a group of images, separated by commas - each with a pixel width declared after them.  It is highly recommended that the images referenced
   be square (1:1 ratio), as this is the industry standard for podcast album art, and what podcast apps expect to work with.  The srcset attribute is designed
   to work like the ```srcset``` attribute in the HTML5 specification.  Suggested widths are 1500px, 600px, 300px and 150px.  See the example feed in this
   repo for an example of how this looks in practice.

   All attributes are required.

<br>

- **\<podcast:id platform="[service slug]" id="[platform id]" url="[link to the podcast page on the service]" />**

   Channel

   (optional | multiple)

   See "ID's" in this document for an explanation.

   - `platform` (required) This is the service slug of the platform.
   - `id` (required) This is the unique identifier for this podcast on the respective platform.
   - `url` (optional) A url to the page for this podcast on the respective platform.

<br>

- **\<podcast:contact type="[feedback or advertising or abuse]">**[email address or url]**\</podcast:contact>**

   Channel

   (optional | multiple)

   This element allows for listing different contact methods for the podcast owner.  This could be for general feedback, advertising inquiries, abuse reports, etc.  Only one element of each "type"
   is allowed.

   All attributes are required.

<br><br>



## Verification for Importing and Transferring

If the "locked" element is present and set to "yes", podcasting hosts should not allow importing of this feed until the email listed in the element's owner="" attribute is
contacted and subsequently changes the value of the element to "no".


<br><br>


## ID's

Their can be multiple **\<podcast:id>** elements to indicate a listing on multiple platforms, directories, hosts, apps and services.  The "platform" attribute shall be a slug
representing the platform, directory, host, app or service. The slugs will look like this:

- blubrry
- captivate
- podcastindex
- fireside
- transistor
- libsyn
- buzzsprout
- apple
- google
- spotify
- anchor
- overcast

More should be added by the community as needed.  This is just a starter list.  The full list is [here](serviceslugs.txt).
