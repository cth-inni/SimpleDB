SimpleDB
========
SimpleDB is PHP class which creates a simple API for dealing with JSON document storage.

<h3>Folder Schema</h3>

<pre>
PROJECT ROOT
    models
        Simple_DB.php
    json
        FOLDERS HERE
            FILES HERE
</pre>
    
If you create the folder structure above and autoload or include this class, everything will work out of the box. 
If not, you will need to edit the constructor to tell the class where you want to store your files. The default 
location is a json folder in the project root. In other words, the storage is located at the same folder level as 
the folder which contains your classes.

If you are interested in protecting your data from prying eyes, use an .htaccess file in the json folder:

<pre>
Order deny,allow
Deny from all
</pre>

<h3>Basic Usage</h3>

Instantiate the class with the name of the type of object you are storing. If you are storing cars, the code would look 
like this:

<pre>
$simpleDB = new Simple_DB('car');
</pre>

This will create a folder for cars. You can think of that folder as a "table."

The class contains four basic methods which attempt to mimic HTTP methods:

<pre>
Simple_DB::get($id);
Simple_DB::post($content);
Simple_DB::put($id, $content);
Simple_DB::delete($id);
</pre>

<h3>Method Reference</h3>

<h4>get</h4>
If you have existing cars that you want to retrieve, you can do so like this:

<pre>
$simpleDB->get();
</pre>

Calling <code>get</code> without the ID parameter will return an array of objects containing all of that type of object. 
The code above will give you every car in storage. Calling <code>get</code> with an ID parameter returns a single object.

<h4>post</h4>

<pre>
$simpleDB->post($content);
</pre>

This method creates a new "row" or JSON document in storage. It creates an ID for the document and posts your object 
in plain text to that file. The file will be <code>YOURID.json</code>. You can pass post a string, array or object; 
but your data will get converted to a JSON object in the end.

<h4>put</h4>

<pre>
$simpleDB->put($id, $content);
</pre>

Similar to the <code>post</code> method, this is usually used to update an existing item in storage. This is not 
necessarily the case, however. You can use this method to write an item with a custom ID if you'd like.

<h4>delete</h4>
<pre>
$simpleDB->delete($id);
</pre>

The name says it all, and the ID is mandatory.

<h3>Advanced Usage</h3>
The class also contains a few more public methods that you may find useful.

<h4>query</h4>
<pre>
$simpleDB->query($queryString);
</pre>

This method is used to search for objects matching a certain criteria. The query string is formatted like a GET string. 
To continue with our car example, you might search for a car like so:

<pre>
$query = $simpleDB->query('color=blue&manufacturer=Honda');
</pre>

This will find all blue Hondas, returning an array of objects. Even though you may store complex multi-dimensional 
objects with this class, the query function will only search the top level. Make sure to include variables that need to 
be searched in the top level of your data schema to properly make use of this method.

<h4>returnSingleId</h4>
<pre>
$car = $simpleDB->query('model=Civic');
$carId = $simpleDB->returnSingleId($car);
</pre>

This method will give easy access to the ID of the object you retreived using <code>get</code>. Use it in a loop for 
multiple objects.

<h4>timestamp</h4>
<pre>
$car = $simpleDB->query('model=Civic');
$carId = $simpleDB->returnSingleId($car);
$time = $simpleDB->timestamp($carId);
</pre>

This is a simple method for retreiving the last modified date on the object. It makes use of PHP's 
<code>filemtime()</code>.

<h4>getJSON</h4>

<pre>
$car = $simpleDB->query('model=Civic');
$carId = $simpleDB->returnSingleId($car);
$json = $simpleDB->getJSON($carId);
</pre>

This function returns the literal JSON string without decoding it.
