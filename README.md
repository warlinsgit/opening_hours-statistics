# opening_hours-statistics

Simple visualization of the [opening_hours][Key:opening_hours] values in [OpenStreetMap](https://openstreetmap.org).
The purpose of this statistic is to show the data quality and grow over time. Because of this, the [library][oh-lib] version which parses the opening_hours is pinned to [v3.0.0](https://github.com/opening-hours/opening_hours.js/releases/tag/v3.0.0).

See [announcement blog post in ypid’s OSM diary](https://www.openstreetmap.org/user/ypid/diary/23881).

See [blog post about how many amenities are open for a given time in ypid’s OSM diary](https://www.openstreetmap.org/user/ypid/diary/34881).

## Generating stats for your own region

Via Overpass it is possible to generate statistics for arbitrary boundaries. The thing is that the downloading from the Overpass API takes it‘s time. Currently the cronjobs are running from 21:00 till 11:00 because of the low load during that time on [overpass-api.de](https://overpass-api.de/munin/localdomain/localhost.localdomain/load.html).

Steps to add new boundaries:

    git clone https://github.com/opening-hours/opening_hours.js.git
    pushd opening_hours.js
    git checkout v3.0.0 -- opening_hours.js test.js test.log
    editor stats_for_boundaries.txt # Add your boundary. Checkout the Makefile for details. If you are generating stats on your own then you can comment out all other boundaries because they will be generated by ypid.
    # Run one of the Makefile targets like: osm-tag-data-gen-stats-cron-overpass. Be sure to read and understand the Makefile before you do.
    popd
    git clone https://github.com/ypid/opening_hours-statistics.git
    pushd opening_hours-statistics
    editor index.html # Change the location of stats_csv_source and add a mapping to boundary_to_name_mapping.
    popd
    # Serve it by your favorite web server.

Please note that the section to setup your own stats with your own boundaries is mainly to document it for the curious people. See a question about this in the [German OSM Forum](http://forum.openstreetmap.org/viewtopic.php?pid=498349#p498349).

Note that I currently only accept pull requests for country boundaries or state boundaries. I am not sure if it is worth it to add little regions yet.

## Internals

* The data is downloaded once a day from [taginfo][] and then parsed with [opening_hours.js][oh-lib] (see real_test.js which exports the csv files).

* April 2015 Overpass API support has been added which allows to import history data.

* Jobs for generating the latest statistics are scheduled every night on [gauss.openstreetmap.de](https://wiki.openstreetmap.org/wiki/FOSSGIS/Server/Projects/opening_hours.js). See the [Makefile](https://github.com/opening-hours/opening_hours.js/blob/master/Makefile) for details.

For visualization the [flot][flot-lib] and the [d3][d3-lib] library are used.

You are encouraged to generate your own stats. Here is the data:

http://openingh.openstreetmap.de/stats_data/real_test.opening_hours.stats.csv
http://openingh.openstreetmap.de/stats_data/

## Author
[Robin `ypid` Schneider](https://wiki.openstreetmap.org/wiki/User:Ypid)

<!-- Link definitions {{{ -->
[Key:opening_hours]: https://wiki.openstreetmap.org/wiki/Key:opening_hours
[flot-lib]: https://github.com/flot/flot
[d3-lib]: https://github.com/mbostock/d3
[oh-lib]: https://github.com/opening-hours/opening_hours.js
[taginfo]: https://taginfo.openstreetmap.org/
[real_test.js]: https://github.com/opening-hours/opening_hours.js/blob/master/real_test.js
[opening_hours_map]: https://github.com/ypid/opening_hours_map
<!-- }}} -->
