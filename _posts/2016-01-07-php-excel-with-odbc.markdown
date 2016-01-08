---
layout: post
title:  "PHP & Excel with ODBC"
date:   2016-01-07 17:25:14 -0600
categories: posts php
---

> Note: This is an old post from before my blog died that I rescued via the Wayback Machine. It was originally found at: http://code.goingasplannedby.us/2011/php-excel-with-odbc/

I’ve been working on a project that uses a variety of charts to compare against to crunch some numbers.
Rather than add each of these into our existing database as its own table, we have opted to leave them as excel sheets and communicate with them directly that way. We decided to use PHP and ODBC for this.

Info on how to achieve this connection with code snippets can be found after the break.

Issues: There are a couple issues that come up regularly. One, discussed below, is that any columns meant to act as field names need to be carefully managed. The other is that it will not work if the excel sheet is actively open. There may be a way to specify a read only call to get around this but I haven’t encountered it yet.

There are additional links at the end of the post that may also prove helpful.

On to the actual guts of the process!

Opening an excel sheet with ODBC is very similar to any other ODBC connection:

{% highlight php %}
// Basic ODBC connection to Excel
$excelFile = realpath('C:/ExcelData.xls');
$excelDir = dirname($excelFile);
$connection = odbc_connect("Driver={Microsoft Excel Driver (*.xls)};DriverId=790;Dbq=$excelFile;DefaultDir=$excelDir" , '', '');
{% endhighlight %}
Once you’re connected you can treat Excel like a database and run select statements off of it.
Each sheet in a book is called like a table.

{% highlight php %}
// Basic selection from a sheet
$result = odbc_exec ($connection, "select * from [Sheet1$]");
{% endhighlight %}
You can use column titles as field names. Be careful, though, as field names are pretty strict.
I’d suggest removing spaces and symbols from anything that might be a field otherwise you’ll get either some gibberish in your field names or generic field names like [F1].

{% highlight php %}
// Selection from an Excel sheet using a field name
$result = odbc_exec($connection, "select * from [employees] where [first_name] = 'Joe'");
{% endhighlight %}
Data can be output or examined with most of the normal ODBC methods.

{% highlight php %}
// Quick output to confirm data
odbc_result_all($result, "bgcolor='DDDDDD' cellpadding = '1'");

// Loop to handle all results 
while( $row = odbc_fetch_array($result) )
{
    // row data is accessible by field name
   $row['first_name'];
   
   // unnamed fields follow a F# pattern
   $row['F4'];

   // each key and value can be examined individually as well
   foreach($row as $key => $value)
   {
      print "<br>Key: " . $key . " Value: " . $value;
   }
}
{% endhighlight %}

That’s pretty much all there is to it. It’s mostly like any other ODBC call with a few hangups.

There are several places I found helpful in figuring this out and to model my code.  
All of them that I have still have links for are listed below:  
**PHP Manual**: [http://php.net/manual/en/function.odbc-connect.php](http://php.net/manual/en/function.odbc-connect.php)  
**O’Reilly Programming PHP**: [http://docstore.mik.ua/orelly/webprog/php/ch15_04.htm](http://docstore.mik.ua/orelly/webprog/php/ch15_04.htm)  
**Excel Forum**: [http://www.excelforum.com/excel-general/477932-excel-odbc-driver-get-column-names.html](http://www.excelforum.com/excel-general/477932-excel-odbc-driver-get-column-names.html)  
