---
title: "Place"
---

# Place

* TOC
{:toc}

Place objects represent physical locations which can be given a name and associated with a latitude and longitude somewhere on Earth. For example, the Caltrain station in San Francisco, CA at 700 4th St., which has a location of latitude 37.776905, longitude -122.395012, can be considered a Place. In order to provide accurate and thorough Place data, we use a database from [factual.com](http://factual.com/). As such, all of our Places use the same UUIDs as those retrieved from factual.com.

~~~ js
{
    "factual_id": "19931850-dc2f-012e-561d-003048cad9da",
    "name": "Caltrain",
    "longitude": -122.395012,
    "latitude": 37.776905,
    "address": "700 4th St",
    "address_extended": "Ste 4",
    "locality": "San Francisco",
    "region": "CA",
    "postcode": "94107",
    "country_code": "us",
    "is_open": true,
    "website": "http://www.caltrain.com",
    "telephone": "(800) 660-4287",
    "categories": [
        {
            "id":"429",
            "labels": [
                "Transportation",
                "Transport Hubs",
                "Rail Stations"
            ]
        }
    ]
}
~~~

## Place Fields

<table>
    <tr>
        <th>Field</th>
        <th>Type</th>
        <th>Description</th>
    </tr>
    <tr>
        <td><code>factual_id</code></td>
        <td>string</td>
        <td>Primary identifier for a place. Uses factual.com's Place UUID.</td>
    </tr>
    <tr>
        <td><code>name</code></td>
        <td>string</td>
        <td>Human-friendly name.</td>
    </tr>
    <tr>
        <td><code>address</code></td>
        <td>string</td>
        <td>Street address.</td>
    </tr>
    <tr>
        <td><code>address_extended</code></td>
        <td>string</td>
        <td>Apartment or Suite number in street address.</td>
    </tr>
    <tr>
        <td><code>locality</code></td>
        <td>string</td>
        <td>City or town.</td>
    </tr>
    <tr>
        <td><code>region</code></td>
        <td>string</td>
        <td>State, region, province etc.</td>
    </tr>
    <tr>
        <td><code>admin_region</code></td>
        <td>string</td>
        <td>Additional sub-division (e.g. Wales).</td>
    </tr>
    <tr>
        <td><code>post_town</code></td>
        <td>string</td>
        <td>Town used in postal addressing.</td>
    </tr>
    <tr>
        <td><code>po_box</code></td>
        <td>string</td>
        <td>PO Box.</td>
    </tr>
    <tr>
        <td><code>postcode</code></td>
        <td>string</td>
        <td>Postcode / zipcode.</td>
    </tr>
    <tr>
        <td><code>country_code</code></td>
        <td>string</td>
        <td>A two-letter country code in <a href="http://en.wikipedia.org/wiki/ISO_3166-1_alpha-2">ISO 3166-1 alpha-2</a> format.</td>
    </tr>
    <tr>
        <td><code>latitude</code></td>
        <td>decimal</td>
        <td>Latitude in decimal degrees.</td>
    </tr>
    <tr>
        <td><code>longitude</code></td>
        <td>decimal</td>
        <td>Longitude in decimal degrees.</td>
    </tr>
    <tr>
        <td><code>is_open</code></td>
        <td>boolean</td>
        <td>Whether the establishment is still "in business" and/or open to the public and does not refer to business hours or whether it may be serving customers at any particular moment in time.</td>
    </tr>
    <tr>
        <td><code>telephone</code></td>
        <td>string</td>
        <td>Telephone number in local formatting.</td>
    </tr>
    <tr>
        <td><code>fax</code></td>
        <td>string</td>
        <td>Fax number in local formatting.</td>
    </tr>
    <tr>
        <td><code>website</code></td>
        <td>string</td>
        <td>Official URL of the establishment.</td>
    </tr>
    <tr>
        <td><code>categories</code></td>
        <td>list of objects</td>
        <td>
            <br>
            <table>
                <tr>
                    <th>Field</th>
                    <th>Type</th>
                    <th>Description</th>
                </tr>
                <tr>
                    <td><code>labels</code></td>
                    <td>list</td>
                    <td>Human-friendly descriptive labels for this category. Ordered from general to specific.</td>
                </tr>
                <tr>
                    <td><code>id</code></td>
                    <td>string</td>
                    <td>Category ID. Corresponds to most specific category label.</td>
                </tr>
            </table>
        </td>
    </tr>
</table>

In the future, Places may eventually sit at multiple categorical "leaf nodes" and as a result, we provide lists of category objects (though for now, no Place will be associated with more than one category ID).

## Attaching Places to other resources

Places can be attached to any resource that allows annotations. You can insert Place data into any annotation using the [`+net.app.core.place` replacement value](https://github.com/appdotnet/object-metadata/blob/master/annotation-replacement-values/+net.app.core.place.md) and providing a `factual_id`. For more information, see the [annotation replacement values](/docs/meta/annotations/#annotation-replacement-values) documentation.

We provide a [Checkin annotation](https://github.com/appdotnet/object-metadata/blob/master/annotations/net.app.core.checkin.md) as a [core annotation](/docs/meta/annotations/#core-annotations) for the common use case of using Place data to give users a way to say "I was here when I posted this", though developers are free to use Place data in other contexts.

## Retrieve a Place

Returns info about a Place object with a given `factual_id`.

### Deduplication effects

Because it is possible for duplicate entries to exist for the same Place, Factual provides a method to deduplicate one Place object by "replacing" it with another. As a result, you may notice sometimes that when requesting a Place with one `factual_id` you will get back an entry with a different `factual_id`. When we return a different Place than the one you requested, we are claiming, to the best of our knowledge, that it is equivalent to the one requested.

<%= migration_warning ['response_envelope'] %>

<%= endpoint "GET", "places/[factual_id]", "Any" %>

### Parameters

None.

### Example

> GET https://alpha-api.app.net/stream/0/places/19931850-dc2f-012e-561d-003048cad9da

~~~ js
{
    "data": {
        "website": "http://www.caltrain.com",
        "name": "Caltrain",
        "locality": "San Francisco",
        "region": "CA",
        "telephone": "(800) 660-4287",
        "longitude": -122.395012,
        "latitude": 37.776905,
        "is_open": true,
        "postcode": "94107",
        "factual_id": "19931850-dc2f-012e-561d-003048cad9da",
        "address": "700 4th St",
        "address_extended": "Ste 4",
        "country_code": "us",
        "categories": [
            {
                "labels": [
                    "Transportation",
                    "Transport Hubs",
                    "Rail Stations"
                ],
                "id": "429"
            }
        ]
    },
    "meta": {
        "code":200
    }
}
~~~

## Search for a Place

Performs a search for nearest places from given latitude and longitude. Optionally takes a `q` string to restrict matches on name (like autocomplete for Place names).

Returns a list of Places sorted by distance or distance/string match if `q` is provided. These are the same Place objects as returned by the previous endpoint but will also include a `distance` property which gives, in meters, the distance from the search centroid to the Place.

<%= migration_warning ['response_envelope'] %>

{::options parse_block_html="true" /}
<div class="alert alert-error alert-block">
**When using this endpoint, it is a requirement that _all_ requests originate from user actions.** As an example, acceptable use cases include when a user presses a button to search for local Places or when a user types a character to specify part of a Place name. Unacceptable use cases include automated access (e.g. "bots", "scrapers"), periodic scans and attempts to create comprehensive local caches or copies of the Place data. **We will be monitoring search usage and will take necessary actions to terminate unacceptable use.**
</div>

<%= endpoint "GET", "places/search", "User" %>

### Parameters

<table>
    <thead>
        <tr>
            <th>Name</th>
            <th>Required?</th>
            <th>Type</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><code>latitude</code></td>
            <td>Required</td>
            <td>decimal</td>
            <td>Latitude of search location. Combined with longitude to create central point of search results.</td>
        </tr>
        <tr>
            <td><code>longitude</code></td>
            <td>Required</td>
            <td>decimal</td>
            <td>Longitude of search location. Combined with latitude to create central point of search results.</td>
        </tr>
        <tr>
            <td><code>q</code></td>
            <td>Optional</td>
            <td>string</td>
            <td>The name-based search query. Can be a partial string — for example, 'cre' will find any local 'creameries' and 'ice cream' locations.</td>
        </tr>
        <tr>
            <td><code>radius</code></td>
            <td>Optional</td>
            <td>decimal</td>
            <td>Approximate radius (in meters) of bounding circle on results. For example, supplying <code>radius=100</code> will limit all locations to be within 100 meters. Defaults to 100, ranges between 0.001 and 50,000.</td>
        </tr>
        <tr>
            <td><code>count</code></td>
            <td>Optional</td>
            <td>int</td>
            <td>Number of results to return. Defaults to 20, ranges between 1 and 100.</td>
        </tr>
        <tr>
            <td><code>altitude</code></td>
            <td>Optional</td>
            <td>decimal</td>
            <td>Altitude of search location (in meters). Not presently used to generate search results but may be later.</td>
        </tr>
        <tr>
            <td><code>horizontal_accuracy</code></td>
            <td>Optional</td>
            <td>decimal</td>
            <td>Accuracy of <code>latitude</code>/<code>longitude</code> parameters (in meters). Not presently used to generate search results but may be later.</td>
        </tr>
        <tr>
            <td><code>vertical_accuracy</code></td>
            <td>Optional</td>
            <td>decimal</td>
            <td>Accuracy of <code>altitude</code> parameter (in meters). Not presently used to generate search results but may be later.</td>
        </tr>
    </tbody>
</table>

### Example (no search string)
> GET https://alpha-api.app.net/stream/0/places/search?latitude=37.761334&longitude=-122.426276

~~~ js
{
    "data": [
        {
            "distance": 17.9039689207,
            "name": "Dolores Park Tennis Courts",
            "locality": "San Francisco",
            "region": "CA",
            "longitude": -122.426165,
            "is_open": true,
            "factual_id": "4c469fdf-8f28-43c7-a9f6-c1cb5e95ff9f",
            "latitude": 37.761469,
            "country_code": "us",
            "categories": [
                {
                    "labels": [
                        "Sports and Recreation",
                        "Racquet Sports",
                        "Tennis"
                    ],
                    "id": "399"
                }
            ]
        },
        {
            "distance": 17.9039689207,
            "name": "Dolores Park",
            "locality": "San Francisco",
            "region": "CA",
            "telephone": "(415) 831-5520",
            "longitude": -122.426165,
            "is_open": true,
            "factual_id": "b7a7f1c8-c68f-4f24-92cb-668cf05658e2",
            "latitude": 37.761469,
            "country_code": "us",
            "categories": [
                {
                    "labels": [
                        "Landmarks",
                        "Parks"
                    ],
                    "id": "118"
                }
            ]
        },
        ...
    ],
    "meta": {
        "code": 200
    }
}
~~~

### Example (search string, radius and count)
> GET https://alpha-api.app.net/stream/0/places/search?latitude=37.78592&longitude=-122.400751&q=bi-rite&count=1&radius=5000

~~~ js
{
    "data": [
        {
            "website": "http://www.biritecreamery.com",
            "distance": 2771.26823449,
            "name": "Bi-Rite Creamery",
            "locality": "San Francisco",
            "region": "CA",
            "telephone": "(415) 626-5600",
            "longitude": -122.409012,
            "is_open": true,
            "postcode": "94110",
            "factual_id": "96304850-ea37-012e-3020-00259004449e",
            "address": "3692 18th St",
            "latitude": 37.761868,
            "country_code": "us",
            "categories": [
                {
                    "labels": [
                        "Social",
                        "Food and Dining",
                        "Restaurants"
                    ],
                    "id": "347"
                }
            ]
        }
    ],
    "meta": {
        "code": 200
    }
}
~~~
