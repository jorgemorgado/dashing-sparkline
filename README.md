# dashing-sparkline

## Preview

![Sparkline](https://raw.githubusercontent.com/wiki/jorgemorgado/dashing-sparkline/sparkline.png)

## Description

A [Dashing](http://shopify.github.com/dashing) widget (and associated job)
to display a sparkline graph. It also displays the current value and the
percentage difference (from the previous value), pretty much like the Number
widget.

A sparkline is a very small line chart, typically drawn without axes or
coordinates. It presents the general shape of the variation (typically over
time) in some measurement, such as temperature or stock market price, in a
simple and highly condensed way (from https://en.wikipedia.org/wiki/Sparkline).

The sparkline is drawn using the Rickshaw Graph framework just like the
default Graph widget.

## Installation

Create a folder in your widgets directory called `sparkline`. Insert the three
files (`sparkline.coffee`, `sparkline.html` and `sparkline.scss`).

Alternately, you can use the automated dashing installer by running
`dashing install 26068a72540619a4d4ec` from the root of your dashing project.

## Usage

Add the following code on the desired dashboard:

```erb
<li data-row="1" data-col="1" data-sizex="1" data-sizey="1">
  <div data-id="water_main_city" data-view="Sparkline" data-title="Water Consumed" data-moreinfo="Main City" data-graphtype="lineplot" data-suffix=" l/h"></div>
</li>
```

You can change the `data-graphtype`. Options include `lineplot`, `line`,
`area` and `bar`. Other options supported by `Rickshaw.Graph` should also work
but they might not look very nice for a sparkline.

Create your sparkline job `my_sparkline_job.rb`. Example:

```ruby
# Populate the graph with some random points
points = []
(1..10).each do |i|
  points << { x: i, y: rand(50000) }
end
last_x = points.last[:x]

SCHEDULER.every '60m' do

  points.shift
  last_x += 1
  points << { x: last_x, y: rand(50000) }

  send_event('water_main_city', points: points)
end
```

Alternatively you can also send data via HTTP post. Example using `curl`:

```sh
curl -d '{
  "auth_token": "YOUR_AUTH_TOKEN",
  "points":
    [
      { "x": "1",  "y": "10" },
      { "x": "2",  "y": "20" },
      { "x": "3",  "y": "70" },
      { "x": "4",  "y": "60" },
      { "x": "5",  "y": "10" },
      { "x": "6",  "y": "80" },
      { "x": "7",  "y": "90" },
      { "x": "8",  "y": "40" },
      { "x": "9",  "y": "30" },
      { "x": "10", "y": "10" }
    ]
  }' \
http://localhost:3030/widgets/water_main_city
```

## Contributors

- [Jorge Morgado](https://github.com/jorgemorgado)

## License

This widget is released under the [MIT License](http://www.opensource.org/licenses/MIT).
