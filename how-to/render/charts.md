(quickchart)[https://quickchart.io/] service permits to obtain a image file from a chart definition.

This definition may be embedded in an url, so it is super simple to get a chart viewable from any device without extra library.

In order to use Quickchart into Abricotine, you need two things :

1. To replace any sensitive character with its url encoded counterpart, ex : replace space with %20, single quote with %27...

You can easily get the encoded result of an url pasting it into Firefox' navigation bar. When you copy it from the navigation bar back, it will be encoded.

So the example from Quickchart front page : 

```
https://quickchart.io/chart?width=500&height=300&c={type:'bar',data:{labels:['January','February','March','April', 'May'], datasets:[{label:'Dogs',data:[50,60,70,180,190]},{label:'Cats',data:[100,200,300,400,500]}]}}
```
becomes 

```
https://quickchart.io/chart?width=500&height=300&c={type:%27bar%27,data:{labels:[%27January%27,%27February%27,%27March%27,%27April%27,%20%27May%27],%20datasets:[{label:%27Dogs%27,data:[50,60,70,180,190]},{label:%27Cats%27,data:[100,200,300,400,500]}]}}
```

2. To fool the software as Abricotine expects that image's url be terminated with one of the extensions listed in file : 

https://github.com/brrd/Abricotine/blob/develop/app/renderer/cm-extend-autopreview.js 

in config.image.regex section

So, you simply have to add an extra parameter in the url at the last position to view the chart, and the previous example url becomes:

```
https://quickchart.io/chart?width=500&height=300&c={type:%27bar%27,data:{labels:[%27January%27,%27February%27,%27March%27,%27April%27,%20%27May%27],%20datasets:[{label:%27Dogs%27,data:[50,60,70,180,190]},{label:%27Cats%27,data:[100,200,300,400,500]}]}}&dummy=fool.png
```

This works because quickchart does not raise any error on unknown parameter.

So using the latest url as an image in Abricotine will show you the expected chart.




