`gut` is a way to use publicly hosted web services to convert data between different formats

A gut web service has the following attributes:

  * accepts POST data containing a file or other data (such as CSV)
  * turns the incoming data into some other data format (e.g. an array of JSON objects)
  * sends the converted data back to the URL specified by the incoming requests HTTP "X-callback" header
  
That's it!

An example:

I'm writing a web app concerning public/open data and I want to allow people to upload CSV spreadsheets. Instead of writing a CSV parser from scratch i'm just going to use a gut-compatible CSV-to-JSON server that some nice person wrote.

Given I have the following file, `cats.csv`:
    
    name,appearance
    chewbacca,hairy
    bill,nonplussed
    bubbles,relaxed
    
I can convert my file to JSON using curl:

    curl -X POST someniceperson.com/csvconverter5000 -H "X-callback: http://cat-information.com" --data-binary @cats.csv
    
My server (cat-information.com) will receive the following HTTP POST:

    POST / HTTP/1.1
    host: cat-information.com
    content-type: application/json
    content-length: 186
    Connection: close

    {"headers":[{"name":"name"},{"name":"appearance"}],"rows":[{"name":"chewbacca","appearance":"hairy"},{"name":"bill","appearance":"nonplussed"},{"name":"bubbles","appearance":"relaxed"}]}

currently only a CSV gut service is implemented, but you are encouraged to fork and add your own simple gut servers for common open data transformation formats. some useful ones might include:

MSOffice (.xls, .xlxs, .doc, .docx etc), ESRI Shapefiles and File GeoDatabases (.shp), SQL dumps (.sql), Address geocoder to lat/lng ("123 fake street")