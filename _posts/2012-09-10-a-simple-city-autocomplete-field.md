---
layout: post
title:  "A Simple City Autocomplete Field"
date:   2012-09-10 19:00:00
categories: Programming
tags: [javascript, tutorials, jquery, programming, web]
---

I was playing around with some code recently, and came across a very easy way to create a city lookup field using jQuery’s UI components and a very useful webservice provided by Geonames.

How the autocomplete functionality in jQuery works, is by wrapping the functionality around a standard html text input field, populating a dropdown list of values from an ajax call to a webservice, in this case, the Geonames webservice.

When supplied with a partial city name, the GeoNames webservice returns back a list of information for cities matching the partial city name supplied, allowing you to display, and keep track of, the country, province, full city name, and a host of other info. Full documentation is provided on the GeoNames site.

{% highlight javascript %}
$( "#city" ).autocomplete({
  source: function( request, response ) {
    $.ajax({
      url: "http://ws.geonames.org/searchJSON",
      dataType: "jsonp",
      data: {
        featureClass: "P",
        style: "full",
        maxRows: 12,
        name_startsWith: request.term
      },
      success: function( data ) {
        //Display city name, state name, country name
        response( $.map( data.geonames, function( item ) {
          return {
            label: item.name + (item.adminName1 ? ", " + item.adminName1 : "") + ", " + item.countryName,
            value: item.name + (item.adminName1 ? ", " + item.adminName1 : "") + ", " + item.countryName
          }
        }));
      }
    });
  },
  minLength: 2,
  select: function( event, ui ) {
    $('#city').val(ui.item.value);
    return false;
  }
});
{% endhighlight %}
