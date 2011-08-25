---
title: Bandwidth Detection
tags: ModestMaps
layout: control
---

This control adapts maps to low-bandwidth conditions. By default, it requests
a test tile to determine speed, but accepts options that allow you to subsitute
your own detection method. This method only works on **composited tiles** - that is,
combinations of tilesets with the characteristic `,` between their tileset
names.


{% highlight html %}
<div id='modestmaps-bw' class='map dark'></div>
<a id='trigger-low'>low quality</a>
<a id='trigger-high'>high quality</a>
<script>
var tilejson = {
  tilejson: '1.0.0',
  scheme: 'tms',
  tiles: ['http://a.tiles.mapbox.com/mapbox/1.0.0/blue-marble-topo-jul' +
    ',world-bank-borders-ar/{z}/{x}/{y}.png']
};
var mm = com.modestmaps;
var m = new mm.Map('modestmaps-bw',
  new wax.mm.connector(tilejson),
  new mm.Point(240,120),
  [new mm.MouseHandler(), new mm.TouchHandler()]);
var bw = wax.mm.bwdetect(m, {
  png: '.png32'
});
document.getElementById('trigger-low').onclick = function() {
  bw.bw(0); return false;
};
document.getElementById('trigger-high').onclick = function() {
  bw.bw(1); return false;
};
m.setCenterZoom(new mm.Location(39, -98), 2);
</script>
{% endhighlight %}

## API

<dl>
  <dt>{% highlight js %}bwdetect = wax.mm.bwdetect(map, options{% endhighlight %}</dt>
  <dd>Creates a new bandwidth detection object. Takes an options argument with the options:
    <dl>
      <dt>auto</dt><dd>default true, if false the control will not test bandwidth</dd>
      <dt>jpg</dt> <dd>the low-quality version of JPEG files. Default `.jpg70`.</dd>
      <dt>png</dt> <dd>the low-quality version of PNG files. Default `.png128`.</dd>
    </dl>
  </dd>
  <dt>{% highlight js %}bwdetect.bw(0 or 1){% endhighlight %}</dt>
  <dd>Manually set the bandwidth level: 1 is for high-bandwidth / original quality,
  0 is for low-bandwidth / degraded quality.</dd>
</dl>