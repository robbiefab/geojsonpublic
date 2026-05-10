Power BI Maps 2026-05-10

(All clipped) Regional Council 2025, Stat Area 2 2025, Stat Area 3 2025

https://www.stats.govt.nz/methods/geographic-hierarchy/

https://datafinder.stats.govt.nz/mapviewer/?mv.basemap=Streets&mv.centre=177.6025534540155%2C-38.24390216957039&mv.content=layer.120966.color%3A009e00%2Clayer.120969.color%3Aff0000%2Clayer.120945.color%3A003399&mv.panes=pane.0.id%3Af66a14bf-2655-4cf3-a4d1-6c8a73d9287d%3Bpane.0.centre%3A%5B177.6025534540155%2C-38.24390216957039%5D%3Bpane.0.zoom%3A4%3Bpane.0.pitch%3A0%3Bpane.0.bearing%3A0%3Bpane.0.resolution%3A6983.556358432649%3Bpane.0.extent%3A%7B%22minx%22%3A148.3385445173388%2C%22miny%22%3A-49.20055805802893%2C%22maxx%22%3A206.86656239069225%2C%22maxy%22%3A-25.350978666422858%7D%3B&mv.panesViewOption=map-pane-single&mv.zoom=4

need to delete comments out to run all commands at once

(regional council layer)
-each "if (REGC2025_V >= 12 && REGC2025_V <= 20) TerritoryCodev2 = 5; else if (REGC2025_V >= 6 && REGC2025_V <= 9) TerritoryCodev2 = 4; else if (REGC2025_V >= 3 && REGC2025_V <= 5) TerritoryCodev2 = 3;  else if (REGC2025_V >= 2 && REGC2025_V <= 2) TerritoryCodev2 = 2; else if (REGC2025_V >= 1 && REGC2025_V <= 1) TerritoryCodev2 = 1; else TerritoryCodev2 = null"



(SA2 layer)
-join regional-council-2025-clipped fields=REGC2025_V,REGC2025_1,TerritoryCodev2
-join statistical-area-3-2025-clipped fields=SA32025_V1,SA32025__1
-each "if (SA32025_V1 >= 50390 && SA32025_V1 <= 51130) TerritoryCodev2 = 1" --upper Auckland into 1
-each "if (SA32025_V1 >= 51290 && SA32025_V1 <= 51370) TerritoryCodev2 = 1"  --upper Auckland into 1
-each "if (TerritoryCodev2 === 2 && SA32025_V1 >= 51960) TerritoryCodev2 = 3" --south Auckland into 3
-each "if (['52000', '50850'].includes(SA32025_V1 )) TerritoryCodev2 = 2"  --botany junction and ? into 2
-each "if (['51440', '51460', '51700', '51870', '51860', '51950'].includes(SA32025_V1 )) TerritoryCodev2 = 1" --moves specific suburbs into 1, beachlands, st lukes (mt albert), Epsom, ellerslie
-each "if (['51610','51880','51840'].includes(SA32025_V1 )) TerritoryCodev2 = 3" --manukau and ? and ? into 3
-dissolve TerritoryCodev2
-each "fill = (TerritoryCodev2 == 1) ? '#BBD2DB' : (TerritoryCodev2 == 2) ? '#7F7F7F' : (TerritoryCodev2 == 3) ? '#BFCDC3' : (TerritoryCodev2 == 4) ? '#F9E0A4' : (TerritoryCodev2 == 5) ? '#EDBFAF' : '#FFFFFF'"
-rename-fields 'Territory Code=TerritoryCodev2'

simplify to 10%? file size max 25mb. can do in UI 

-proj wgs84
-o output.json
