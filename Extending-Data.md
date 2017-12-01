_Extending data_

## Calling Web Services

- **NOTE:** For a walkthrough tutorial of this, see [Fetching URLs From Web Services](Fetching+URLs+From+Web+Services)

You can construct a new column by calling a web service, passing it data from existing columns. For example, if you have a column containing street addresses, you can create a new column containing their corresponding latitude/longitude pairs by calling a geocoding service, such as Google's [http:_code.google.com/apis/maps/documentation/geocoding/_](Geocoding+service). Invoke the column's drop-down menu and pick <tt>Edit column &gt; Add column by fetching URLs...</tt> The expression you enter formulates the URL to fetch for each row. For example, this expression should work on Google's Geocoding service:

```
"http://maps.googleapis.com/maps/api/geocode/json?sensor=false&address=" + escape(value,'url')
```

Consider a cell containing

```
77 Massachusetts Ave., Cambridge, MA
```

Evaluating the expression above on that cell yields the URL

```
http://maps.googleapis.com/maps/api/geocode/json?sensor=false&address=77%20Massachusetts%20Ave.%2C%20Cambridge%2C%20MA
```

The result returned from that URL will be stored into the cell in the new column on the same row as the original cell. In this case, the result will look something like this:

```
{
    "status": "OK",
    "results": [ {
      "types": ["street_address"],
      "formatted_address": "77 Massachusetts Ave, Cambridge, MA 02139, USA",
      "address_components": [ {
        "long_name": "77",
        "short_name": "77",
        "types": ["street_number"]
      }, ...
      ],
      "geometry": {
        "location": {
          "lat": 42.3590509,
          "lng": -71.0936229
        },
        "location_type": "ROOFTOP",
        "viewport": {
          "southwest": {
            "lat": 42.3559033,
            "lng": -71.0967705
          },
          "northeast": {
            "lat": 42.3621985,
            "lng": -71.0904753
          }
        }
      }
    }, ...
    ]
  }
```

Since it's just JSON, we can parse it out using the <tt>parseJSON</tt> expression, and then extract the lat/lng pair inside.

## Querying Databases

You can also extend your data using queryable web databases such as [http:_www.freebase.com/_](Freebase).

- See [Extending Data From Freebase](Extending+Data+-+Using+Freebase) for more details.
