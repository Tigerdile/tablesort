# tablesort
Basic table-sorting program for jQuery, designed to work with dynamically reloaded ajax

Okay ... there's a million table sort plugins for jQuery, some with some very rich features, others very basic.  Why did I make one?

Because even the basic ones were too complex for my needs!  Most sort plugins tout that they can handle thousands of rows.  This one does not.  This plugin is made to work with, on average, 20-ish rows though it should work okay for a few hundred rows.

Specifically, I use this plugin to refresh a list of online channels; I needed a table sort that would allow whichever column the user wants to sort by to remain the sorting column each time the table refreshes.

No fancypants features were necessary; just click the header to toggle table directions and that's it.

Out of the box, the sorter understands text sorting and numeric sorting; however it is very easy to add custom sort handlers.

# Usage
Download the td-tablesort.js file, and include it thusly (after jQuery's include) :

```
<script src="td-tablesort.js" type="text/javascript"></script>
```

Then, you attach it to your tables thusly :

jQuery('#tableSelector').tablesort();

Your tables must be structured with a ```<thead>``` and ```<th>``` elements, thusly:

```
<table id="tableSelector">
  <thead>
    <tr>
       <th>Column 1</th>
       <th>Column 2</th>
    </tr>
  </thead>
  <tbody></tbody>
</table>
```

This will inject ```<a>``` elements around your column names in the ```<th>```, which will capture the sorting clicks.  Whichever ```<a>``` element has the class 'active' will be the default sort, so you could do thusly:

```
jQuery('#tableSelector').tablesort();
jQuery('th#someColumnId a').addClass('active');
```

to make the column with header ID someColumnId the default sort column.  The class 'desc' on the a record will make it sort descending instead of the default ascending.

When you want to sort the table, you call the 'resort' method on the table object.  This part is kind of clunky, but works for me.  I call resort after ajax has populated it.

For instance:

```
jQuery.get('/some/url', function(data) {
  // use data to populate the table
  var myTable = jQuery('#tableSelector');
  myTable.get(0).resort(myTable);
});
```

'resort' will sort whatever column's th a is 'active'.  The sorted 'td' elements will have the class 'sorted' if you care to style that at all.

Yes, I know this is crude and clunky.  And it doesn't handle "colspans" at all.  But it works for what I need, and maybe it'll work for you, too.

# Custom Sorting
If "parseInt" understands your column of data to be entirely numeric, your data will be sorted numerically; otherwise it will use the string sort.

If this very basic sorting is not acceptable, you can attach a custom sort method to your th elements.  The method should be called 'sortFunction' and it must take two parameters (the items to sort).  It works thusly :

```
jQuery('#tableSelector th#someColumnHeader').each(function() {
  this['sortFunction'] = function(a, b) {
    // return positive (i.e. 1) if a > b
    // return 0 if a == b
    // return negative (i.e. -1) if a < b
  };
});
```

I use a .each because in my case, I use a CSS class on my th element and I have multiple tables that use the same sort functions.  You can override the sort function at any time.

# Gotchyas?
There are NO gotchyas!  There's no cache, there's no weird processing, there's no performance tuning.  This is made for small, dynamic tables.

Wouldn't recommend it for huge tables, but huge tables + ajax would have their own problems!


Hope this is useful to you.


# License
This is released in the Public Domain.  You are free to do with it whatever you please with no warranty or promise of functionality.  It would be very courteous of you to retain Tigerdile, LLC's information at the top of the JS file but it is not specifically required.


