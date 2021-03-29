# Render for osm maps

Render all tiles from zoom 0 to 13
```console
nohup render_list -m default -a -z 0 -Z 13 --num-threads=6 &
```

Render tiles for specific territory (moscow)
x && y can get from leaflet
```console
nohup render_list -m default -a -z 18 -Z 18 -x 158313 -X 158632 -y 81750 -Y 82165 --num-threads=6 &
nohup render_list -m default -a -z 17 -Z 17 -x 79125 -X 79337 -y 40872 -Y 41112 --num-threads=6 &
nohup render_list -m default -a -z 16 -Z 16 -x 39561 -X 39665 -y 20425 -Y 20552 --num-threads=6 &
nohup render_list -m default -a -z 15 -Z 15 -x 19750 -X 19898 -y 10171 -Y 10315 --num-threads=6 &
```
