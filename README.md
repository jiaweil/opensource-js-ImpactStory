ImpactStory.js
=========================

An awesome, easy to use, flexible and extensible JavaScript API for ImpactStory (http://impactstory.org). 

It has two major features:

1. A general JavaScript API for working with ImpactStory data. This is essentially a javaScript wrapper around the REST api that abstracts away a lot of the boilerplate to make it easier to use.

2. A robust "embed" feature-set for embedding ALM data (reports and badges) into a website. It's powered using mustache.js, so advanced users can swap out the templates. It's also built as a jQuery plugin, so that it should work well with webpages loaded by AJAX and also gives developers flexibility on when / how the ALM report / badges are embedded. Despite all these features, the "default" implementation looks nearly identical to and as easy to use as the one on impactstory.org.

Overall I'm pretty happy with it.  Left to do:

1. Getting the badges mustache template working. I just ran out of time and prioritized the report template.

2. Adding synchronous wrappers for all the async API methods. 

3. Documentation (both inline and readme) as well as lots of little tweaks and bug fixes.

I'm currently working on the documentation. In the meantime, check out the `example.html` file, which is well documented and provides a lot of useful exmaples. 

```html
<!DOCTYPE html>
<html>
  <head>
    <script src="//ajax.googleapis.com/ajax/libs/jquery/1.9.1/jquery.min.js"></script>
    <script src="//raw.github.com/janl/mustache.js/master/mustache.js"></script>
    <script src="./ImpactStory.js"></script>

    <script>
       // Optionally set some global variables
       ImpactStory.key = 'YOURKEY';
       ImpactStory.templates = './templates'; // You can set your own template path if you are hosting the library locally, otherwise it will use a CDN
       
       // Overriding jQuery for the sake of brevity in this example file (don't do this in real life)
       var $ = jQuery;
    </script>
  </head>

  <body>
    <!-- It's easy to auto-embed an ALM report in your website like so. -->
    <!-- This is all you really need, everything else (even the global variables above) is optional -->
    <div class="impactstory-embed-report" data-id="19210768" data-id-type="pmid" data-api-key="YOURKEY"></div>

    <!-- You can provide data-doi or data-pmid directly instead of data-id and data-id-type -->
    <div class="impactstory-embed-report" data-pmid="19210768" data-api-key="YOURKEY"></div>

    <!-- You can provide data-preloaded="yes" to make things faster if you are sure that impactStory has your item indexed -->
    <div class="impactstory-embed-report" data-preloaded="yes" data-pmid="19210768" data-api-key="YOURKEY"></div>

    <!-- You can also trigger your own embed. This is useful if you need to embed to a page that is loaded by AJAX -->
    <!-- Note that we don't need to set a api-key since we set it above in the global options -->
    <div class="my-own-class" data-pmid="19210768"></div>
    <script>
      $('.my-own-class').ImpactStoryEmbed('report');
    </script>

    <!-- Instead of relying on data elements, you can pass in the requisite information -->
    <!-- Anything you can set in the data elements you can set as options and vice-versa -->
    <div class="my-own-class-2"></div>
    <div class="my-own-class-3"></div>
    <div class="my-own-class-4"></div>
    <script>
      $('.my-own-class-2').ImpactStoryEmbed('report', {doi: '10.1186/1471-2148-9-37'});

      $('.my-own-class-3').ImpactStoryEmbed('report', {preloaded: true,  'id-type': 'pmid',  id: 19210768});

      // Or we can pass an "item" in a format like {pmid: 12345} or ['pmid','12345']
      $('.my-own-class-4').ImpactStoryEmbed('report', {item: ['pmid','12345']});
    </script>

    <!-- There is also a robust API for working entirely in JavaScript land -->
    <script>
      // It's easy to get JSON metric data for a single item
      ImpactStory.getItem({doi: '10.1186/1471-2148-9-37'}, function(data) {
        // Log all the metrics data in JSON format!
        console.log(data);
      });

      // It's also easy to create and then get a collection of ALM data in JSON format
      ImpactStory.createAndGetCollection(
          [["pmid", "10197686"], ["pmid", "11074575"]],
          "test collection",
          function(collectionData) {
            console.log(collectionData);
          }
      );
      
      // You can instatiate different impactStory objects that use different keys, urls etc.
      var myIS = new impactStory();
      myIS.key = 'SomeOtherDifferentKey';
      myIS.getItem({doi: '10.1186/1471-2148-9-37'}, function(data) { console.log(data) });
      
      var myOtherIS = new impactStory();
      myOtherIS.url = 'http://api.impactstory.org/v2/'; // Use a theoretical v2 url
      myOtherIS.getItem({doi: '10.1186/1471-2148-9-37'}, function(data) { console.log(data) });

      // **
      // There's a lot more you can do if you read the source of ImpactStory.js
      // MORE DOCUMENTATION COMMING SOON!
      // **
    </script>

  </body>
</html>
```
