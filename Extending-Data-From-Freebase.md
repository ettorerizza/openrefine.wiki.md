 **NOTE !! - The FREEBASE API is now deprecated.**

## Fetching Text Blurbs (descriptions) and Photos

NOTE: The simplest way to get this information now is the Topic API, but this documentation needs to be updated.

The Freebase graph stores many things, but 2 examples of useful data that are NOT stored in the graph are - text blurbs and photos.

You can get the text blurbs (topic descriptions) by fetching them from the Blob store using "Add column by fetching URL" but first you'll need the article ID.

1. Starting from your reconciled column, use "Add a column by fetching a URL" and construct the URL as follows: <tt>"https://www.googleapis.com/freebase/v1/text" + cell.recon.match.id</tt>
2. Now the new column contains results in JSON format. Choose "Add Column based on this column". 
3. Type the following expression to extract the desired description:

<tt>parseJson(value).result</tt>

Note: for images you can use directly " [https://usercontent.googleapis.com/freebase/v1/image/](https://usercontent.googleapis.com/freebase/v1/image/)" + cell.recon.match.id

You could do something similar using the property /common/topic/image to get the images associated with a topic, except that Refine doesn't store handle images as a native datatype, so the best you can do is construct the URLs that you'd use to fetch them

The Topic API is documented at [https://developers.google.com/freebase/v1/topic-overview](https://developers.google.com/freebase/v1/topic-overview) if you want more information.

Note that both text blurbs and images typically have different licenses than the rest of Freebase data since they are often provided by Wikipedia (or other contributors). Using the Topic API is a good way to deal with this since it includes the necessary attribution text that you can use to credit the licensor.

If you need additional help with pulling in other Freebase properties or schema, then do not hesitate to ask us on the [http:_groups.google.com/group/openrefine/_](Refine+mailing+list).

## Fetching Properties against a reconciled Freebase Type

(TODO)

## Fetching Properties from Freebase co-Types

(TODO)

