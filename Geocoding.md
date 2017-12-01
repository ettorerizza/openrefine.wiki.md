# Translate street addresses to lat/lng coordinates.

You can use Refine to translate street addresses to lat/lng coordinates to plot data on maps. While there is no native geocoding command in Refine, you can still accomplish this task calling HTTP-based geocoding services using the command Add Column by Fetching URLs and then parsing the returned HTTP responses to extract the lat/lng coordinates.

## Google Geocoding Service

One geocoding service to use is the [Google Geocoding API](http://code.google.com/apis/maps/documentation/geocoding/). For whatever API you use, please note its usage limits and its terms of service. For the Google Geocoding API, the limit is 2,500 calls per day, with the following restriction:

    the Geocoding API may only be used in conjunction with a Google map; geocoding results without displaying them on a map is prohibited. For complete details on allowed usage, consult the [Maps API Terms of Service License Restrictions](http://code.google.com/apis/maps/terms.html#section_10_12).

It is your responsibility to observe the terms of service, such as, in this case, plotting your data on a Google map eventually.

This document will use the Google Geocoding API as an example. Note that to geocode such an address as "77 Massachusetts Ave, Cambridge, MA", you call this URL:

  [http://maps.google.com/maps/api/geocode/json?sensor=false&address=77+Massachusetts+Ave,+Cambridge,+MA](http://maps.google.com/maps/api/geocode/json?sensor=false&address=77+Massachusetts+Ave,+Cambridge,+MA)

Then, parse the HTTP response body as JSON, and retrieve the lat/lng coordinates in `jsonBody.results[0].geometry.location`.

### Fetching

Say your project has a column named `address` that contains street addresses. Click on that column's dropdown menu and invoke Edit Column > Add Column by Fetching URLs... and enter this expression

  `"http://maps.google.com/maps/api/geocode/json?sensor=false&address=" + escape(value, "url")`

Give the new column to be created a name, such as `geocodingResponse`, adjust the throttle delay if you wish, and click OK. The process will take a while depending on the delay.

### Parsing

When the fetching process is done, you should see the new column `geocodingResponse` with a lot of JSON blobs. Invoke that column's dropdown menu Edit Column > Add Column based on This Column... and enter this expression

  `with(value.parseJson().results[0].geometry.location, pair, pair.lat +", " + pair.lng)`

Give the new column to be created a name, such as "latlng", and hit OK.

### Cleaning Up

You can delete the `geocodingResponse` column after you have already extracted the lat/lng coordinates.

### Batching

If you have more rows than the per-day limit, then you can work on one small batch of row per day. First, create a Custom Numeric Facet with an expression like this:

```
  floor(rowIndex / 2000)
```

Replace 2000 with some number below the per-day limit. Then use this facet to select a batch of rows to process.

At the end, you will have created several columns for the several batches. Say, the columns are named `response1`, `response2`, and `response3`. Merge all of them together by creating a new column using this expression

```
  forNonBlank(cells.response1.value, v1, v1, forNonBlank(cells.response2.value, v2, v2, forNonBlank(cells.response3.value, v3, v3, null)))
```

You can then delete the original 3 columns and use the merged one.

Additional References, Tests, & Software for GeoData & Coordinate Systems
----
  * http://earth-info.nga.mil/GandG/