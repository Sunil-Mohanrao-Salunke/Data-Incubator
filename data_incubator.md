---
title: 'Data Incubator: R Notebook'
output:
  html_document:
    keep_md: true
    df_print: paged
editor_options:
  chunk_output_type: inline
---




```r
#question: What proportion of FDNY responses in this dataset correspond to the most common type of incident?
library(tidyverse)
```

```
## ── Attaching packages ─────────────────────────────────────────────────────────────────────────────────────────── tidyverse 1.2.1 ──
```

```
## ✔ ggplot2 3.1.0     ✔ purrr   0.2.5
## ✔ tibble  1.4.2     ✔ dplyr   0.7.7
## ✔ tidyr   0.8.1     ✔ stringr 1.3.1
## ✔ readr   1.1.1     ✔ forcats 0.3.0
```

```
## ── Conflicts ────────────────────────────────────────────────────────────────────────────────────────────── tidyverse_conflicts() ──
## ✖ dplyr::filter() masks stats::filter()
## ✖ dplyr::lag()    masks stats::lag()
```

```r
#Read data
incidents <- read_csv("Incidents_Responded_to_by_Fire_Companies.csv")
```

```
## Parsed with column specification:
## cols(
##   .default = col_character(),
##   IM_INCIDENT_KEY = col_integer(),
##   UNITS_ONSCENE = col_integer(),
##   TOTAL_INCIDENT_DURATION = col_integer(),
##   ZIP_CODE = col_integer(),
##   FIRE_ORIGIN_BELOW_GRADE_FLAG = col_integer(),
##   STORY_FIRE_ORIGIN_COUNT = col_integer(),
##   STANDPIPE_SYS_PRESENT_FLAG = col_integer()
## )
```

```
## See spec(...) for full column specifications.
```

```
## Warning in rbind(names(probs), probs_f): number of columns of result is not
## a multiple of vector length (arg 2)
```

```
## Warning: 1 parsing failure.
## row # A tibble: 1 x 5 col      row col     expected           actual file                            expected    <int> <chr>   <chr>              <chr>  <chr>                           actual 1 226666 ZIP_CO… no trailing chara… -0000  'Incidents_Responded_to_by_Fir… file # A tibble: 1 x 5
```

```r
#Check if data is loaded properly
head(incidents)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["IM_INCIDENT_KEY"],"name":[1],"type":["int"],"align":["right"]},{"label":["FIRE_BOX"],"name":[2],"type":["chr"],"align":["left"]},{"label":["INCIDENT_TYPE_DESC"],"name":[3],"type":["chr"],"align":["left"]},{"label":["INCIDENT_DATE_TIME"],"name":[4],"type":["chr"],"align":["left"]},{"label":["ARRIVAL_DATE_TIME"],"name":[5],"type":["chr"],"align":["left"]},{"label":["UNITS_ONSCENE"],"name":[6],"type":["int"],"align":["right"]},{"label":["LAST_UNIT_CLEARED_DATE_TIME"],"name":[7],"type":["chr"],"align":["left"]},{"label":["HIGHEST_LEVEL_DESC"],"name":[8],"type":["chr"],"align":["left"]},{"label":["TOTAL_INCIDENT_DURATION"],"name":[9],"type":["int"],"align":["right"]},{"label":["ACTION_TAKEN1_DESC"],"name":[10],"type":["chr"],"align":["left"]},{"label":["ACTION_TAKEN2_DESC"],"name":[11],"type":["chr"],"align":["left"]},{"label":["ACTION_TAKEN3_DESC"],"name":[12],"type":["chr"],"align":["left"]},{"label":["PROPERTY_USE_DESC"],"name":[13],"type":["chr"],"align":["left"]},{"label":["STREET_HIGHWAY"],"name":[14],"type":["chr"],"align":["left"]},{"label":["ZIP_CODE"],"name":[15],"type":["int"],"align":["right"]},{"label":["BOROUGH_DESC"],"name":[16],"type":["chr"],"align":["left"]},{"label":["FLOOR"],"name":[17],"type":["chr"],"align":["left"]},{"label":["CO_DETECTOR_PRESENT_DESC"],"name":[18],"type":["chr"],"align":["left"]},{"label":["FIRE_ORIGIN_BELOW_GRADE_FLAG"],"name":[19],"type":["int"],"align":["right"]},{"label":["STORY_FIRE_ORIGIN_COUNT"],"name":[20],"type":["int"],"align":["right"]},{"label":["FIRE_SPREAD_DESC"],"name":[21],"type":["chr"],"align":["left"]},{"label":["DETECTOR_PRESENCE_DESC"],"name":[22],"type":["chr"],"align":["left"]},{"label":["AES_PRESENCE_DESC"],"name":[23],"type":["chr"],"align":["left"]},{"label":["STANDPIPE_SYS_PRESENT_FLAG"],"name":[24],"type":["int"],"align":["right"]}],"data":[{"1":"55672688","2":"2147","3":"300 - Rescue, EMS incident, other","4":"01/01/2013 12:00:20 AM","5":"01/01/2013 12:14:23 AM","6":"1","7":"01/01/2013 12:20:06 AM","8":"1 - More than initial alarm, less than Signal 7-5","9":"1186","10":"00 - Action taken, other","11":"NA","12":"NA","13":"UUU - Undetermined","14":"E 138 ST","15":"10454","16":"2 - Bronx","17":"NA","18":"NA","19":"NA","20":"NA","21":"NA","22":"NA","23":"NA","24":"NA"},{"1":"55672692","2":"0818","3":"735A - Unwarranted alarm/defective condition of alarm system","4":"01/01/2013 12:00:37 AM","5":"01/01/2013 12:09:03 AM","6":"3","7":"01/01/2013 12:30:06 AM","8":"1 - More than initial alarm, less than Signal 7-5","9":"1769","10":"86 - Investigate","11":"NA","12":"NA","13":"UUU - Undetermined","14":"W 46 ST","15":"10036","16":"1 - Manhattan","17":"NA","18":"NA","19":"NA","20":"NA","21":"NA","22":"NA","23":"NA","24":"NA"},{"1":"55672693","2":"9656","3":"300 - Rescue, EMS incident, other","4":"01/01/2013 12:01:17 AM","5":"01/01/2013 12:04:55 AM","6":"1","7":"01/01/2013 12:15:18 AM","8":"1 - More than initial alarm, less than Signal 7-5","9":"841","10":"00 - Action taken, other","11":"NA","12":"NA","13":"UUU - Undetermined","14":"116 ST","15":"11418","16":"5 - Queens","17":"NA","18":"NA","19":"NA","20":"NA","21":"NA","22":"NA","23":"NA","24":"NA"},{"1":"55672695","2":"7412","3":"412 - Gas leak (natural gas or LPG)","4":"01/01/2013 12:02:32 AM","5":"01/01/2013 12:07:48 AM","6":"4","7":"01/01/2013 12:40:11 AM","8":"1 - More than initial alarm, less than Signal 7-5","9":"2259","10":"44 - Hazardous materials leak control & containment","11":"64 - Shut down system","12":"82 - Notify other agencies.","13":"429 - Multifamily dwelling","14":"43 ST","15":"11103","16":"5 - Queens","17":"1","18":"NA","19":"NA","20":"NA","21":"NA","22":"NA","23":"NA","24":"NA"},{"1":"55672697","2":"4019","3":"735A - Unwarranted alarm/defective condition of alarm system","4":"01/01/2013 12:01:49 AM","5":"01/01/2013 12:06:27 AM","6":"6","7":"01/01/2013 12:24:56 AM","8":"1 - More than initial alarm, less than Signal 7-5","9":"1387","10":"86 - Investigate","11":"NA","12":"NA","13":"UUU - Undetermined","14":"WYCKOFF AVE","15":"11385","16":"5 - Queens","17":"NA","18":"NA","19":"NA","20":"NA","21":"NA","22":"NA","23":"NA","24":"NA"},{"1":"55672698","2":"1328","3":"735A - Unwarranted alarm/defective condition of alarm system","4":"01/01/2013 12:02:45 AM","5":"01/01/2013 12:07:55 AM","6":"3","7":"01/01/2013 12:18:20 AM","8":"1 - More than initial alarm, less than Signal 7-5","9":"935","10":"86 - Investigate","11":"NA","12":"NA","13":"UUU - Undetermined","14":"HAMILTON AVE","15":"11215","16":"4 - Brooklyn","17":"NA","18":"NA","19":"NA","20":"NA","21":"NA","22":"NA","23":"NA","24":"NA"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

```r
#Calculate highest Frequency
incidents %>%
  select(INCIDENT_TYPE_DESC) %>% 
  group_by(INCIDENT_TYPE_DESC) %>% 
  summarise(count.incident = n()) %>% 
  mutate(relative.freq = format(round((count.incident/sum(count.incident)), 10), nsmall = 10)) %>% 
  arrange(desc(relative.freq))
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["INCIDENT_TYPE_DESC"],"name":[1],"type":["chr"],"align":["left"]},{"label":["count.incident"],"name":[2],"type":["int"],"align":["right"]},{"label":["relative.freq"],"name":[3],"type":["chr"],"align":["left"]}],"data":[{"1":"300 - Rescue, EMS incident, other","2":"823378","3":"0.3614828304"},{"1":"651 - Smoke scare, odor of smoke","2":"148924","3":"0.0653812332"},{"1":"353 - Removal of victim(s) from stalled elevator","2":"118264","3":"0.0519207526"},{"1":"710 - Malicious, mischievous false call, other","2":"117864","3":"0.0517451430"},{"1":"522 - Water or steam leak","2":"108893","3":"0.0478066573"},{"1":"412 - Gas leak (natural gas or LPG)","2":"108362","3":"0.0475735354"},{"1":"735A - Unwarranted alarm/defective condition of alarm system","2":"100500","3":"0.0441219275"},{"1":"113 - Cooking fire, confined to container","2":"87039","3":"0.0382122234"},{"1":"555 - Defective elevator, no occupants","2":"48147","3":"0.0211376960"},{"1":"736 - CO detector activation due to malfunction","2":"45982","3":"0.0201872087"},{"1":"322 - Motor vehicle accident with injuries","2":"45057","3":"0.0197811113"},{"1":"445 - Arcing, shorted electrical equipment","2":"37839","3":"0.0166122350"},{"1":"740A - Unnecessary alarm/construction activities","2":"31373","3":"0.0137735048"},{"1":"151 - Outside rubbish, trash or waste fire","2":"28130","3":"0.0123497495"},{"1":"323 - Motor vehicle/pedestrian accident (MV Ped)","2":"27737","3":"0.0121772130"},{"1":"5001 - Cable / Telephone Wires Down","2":"24997","3":"0.0109742868"},{"1":"311 - Medical assist, assist EMS crew","2":"24530","3":"0.0107692625"},{"1":"611 - Dispatched & canceled en route","2":"23107","3":"0.0101445311"},{"1":"331 - Lock-in (if lock out , use 511 )","2":"22519","3":"0.0098863849"},{"1":"745A - Unnecessary alarm/ordinary household activities","2":"21496","3":"0.0094372632"},{"1":"600 - Good intent call, other","2":"19742","3":"0.0086672149"},{"1":"511 - Lock-out","2":"18456","3":"0.0081026298"},{"1":"735B - Unnecessary alarm/alarm system testing or servicing","2":"18426","3":"0.0080894591"},{"1":"111 - Building fire","2":"13089","3":"0.0057463872"},{"1":"500 - Service Call, other","2":"13020","3":"0.0057160945"},{"1":"200 - Overpressure rupture, explosion, overheat other","2":"12816","3":"0.0056265336"},{"1":"118 - Trash or rubbish fire, contained","2":"10451","3":"0.0045882414"},{"1":"424 - Carbon monoxide incident","2":"10203","3":"0.0044793635"},{"1":"116 - Fuel burner/boiler malfunction, fire confined","2":"9672","3":"0.0042462416"},{"1":"131 - Passenger vehicle fire","2":"8985","3":"0.0039446320"},{"1":"746 - Carbon monoxide detector activation, no CO","2":"8807","3":"0.0038664857"},{"1":"622 - No incident found on arrival at dispatch address","2":"8779","3":"0.0038541931"},{"1":"4001 - Tree/branch removal or cutting","2":"8680","3":"0.0038107297"},{"1":"100 - Fire, other","2":"8196","3":"0.0035982420"},{"1":"550 - Public service assistance, other","2":"7867","3":"0.0034538030"},{"1":"444 - Power line down","2":"6985","3":"0.0030665837"},{"1":"112 - Fires in structure other than in a building","2":"6092","3":"0.0026745352"},{"1":"731 - Sprinkler activation due to malfunction","2":"6090","3":"0.0026736571"},{"1":"732 - Extinguishing system activation due to malfunction","2":"5422","3":"0.0023803890"},{"1":"740B - Unnecessary alarm/other known cause","2":"5221","3":"0.0022921451"},{"1":"463 - Vehicle accident, general cleanup","2":"5097","3":"0.0022377061"},{"1":"520 - Water problem, other","2":"4921","3":"0.0021604379"},{"1":"400 - Hazardous condition, other","2":"4731","3":"0.0020770233"},{"1":"142 - Brush or brush-and-grass mixture fire","2":"4516","3":"0.0019826331"},{"1":"324 - Motor vehicle accident with no injuries.","2":"4022","3":"0.0017657552"},{"1":"411 - Gasoline or other flammable liquid spill","2":"3908","3":"0.0017157064"},{"1":"553 - Public service","2":"3696","3":"0.0016226333"},{"1":"440 - Electrical  wiring/equipment problem, other","2":"3353","3":"0.0014720480"},{"1":"117 - Commercial Compactor fire, confined to rubbish","2":"2485","3":"0.0010909750"},{"1":"510 - Person in distress, other","2":"2376","3":"0.0010431214"},{"1":"352 - Extrication of victim(s) from vehicle","2":"2028","3":"0.0008903410"},{"1":"321 - EMS call, excluding vehicle accident with injury","2":"1858","3":"0.0008157069"},{"1":"413 - Oil or other combustible liquid spill","2":"1583","3":"0.0006949752"},{"1":"551 - Assist police or other governmental agency","2":"1555","3":"0.0006826826"},{"1":"700 - False alarm or false call, other","2":"1515","3":"0.0006651216"},{"1":"554 - Assist invalid","2":"1511","3":"0.0006633655"},{"1":"461 - Building or structure weakened or collapsed","2":"1503","3":"0.0006598533"},{"1":"4002 - Explosive Escort","2":"1444","3":"0.0006339509"},{"1":"552 - Police matter","2":"1335","3":"0.0005860972"},{"1":"621 - Wrong location","2":"1303","3":"0.0005720485"},{"1":"150 - Outside rubbish fire, other","2":"1237","3":"0.0005430729"},{"1":"442 - Overheated motor","2":"1066","3":"0.0004679997"},{"1":"365 - Watercraft rescue","2":"1036","3":"0.0004548290"},{"1":"650 - Steam, other gas mistaken for smoke, other","2":"971","3":"0.0004262925"},{"1":"132 - Road freight or transport vehicle fire","2":"949","3":"0.0004166339"},{"1":"531 - Smoke or odor removal","2":"885","3":"0.0003885364"},{"1":"652 - Steam, vapor, fog or dust thought to be smoke","2":"807","3":"0.0003542925"},{"1":"4003 - No Detector Activation Carbon Monoxide Incident or Emergency","2":"802","3":"0.0003520974"},{"1":"460 - Accident, potential accident, other","2":"712","3":"0.0003125852"},{"1":"730 - System malfunction, other","2":"612","3":"0.0002686828"},{"1":"350 - Extrication, rescue, other","2":"600","3":"0.0002634145"},{"1":"742 - Extinguishing system activation","2":"587","3":"0.0002577072"},{"1":"342 - Search for person in water","2":"586","3":"0.0002572682"},{"1":"410 - Combustible/flammable gas/liquid condition, other","2":"586","3":"0.0002572682"},{"1":"4004 - Scaffold Incident","2":"502","3":"0.0002203901"},{"1":"130 - Mobile property (vehicle) fire, other","2":"398","3":"0.0001747316"},{"1":"441 - Heat from short circuit (wiring), defective/worn","2":"358","3":"0.0001571706"},{"1":"671 - HazMat release investigation w/no HazMat","2":"350","3":"0.0001536585"},{"1":"370 - Electrical rescue, other","2":"326","3":"0.0001431219"},{"1":"160 - Special outside fire, other","2":"316","3":"0.0001387316"},{"1":"661 - EMS call, party transported by non-fire agency","2":"313","3":"0.0001374146"},{"1":"721 - Bomb scare - no bomb","2":"311","3":"0.0001365365"},{"1":"733 - Smoke detector activation due to malfunction","2":"290","3":"0.0001273170"},{"1":"422 - Chemical spill or leak","2":"288","3":"0.0001264390"},{"1":"744 - Detector activation, no fire - unintentional","2":"278","3":"0.0001220487"},{"1":"542 - Animal rescue","2":"261","3":"0.0001145853"},{"1":"351 - Extrication of victim(s) from building/structure","2":"252","3":"0.0001106341"},{"1":"512 - Ring or jewelry removal","2":"251","3":"0.0001101951"},{"1":"360 - Water & ice-related rescue, other","2":"238","3":"0.0001044877"},{"1":"114 - Chimney or flue fire, confined to chimney or flue","2":"226","3":"0.0000992195"},{"1":"743 - Smoke detector activation, no fire - unintentional","2":"224","3":"0.0000983414"},{"1":"115 - Incinerator overload or malfunction, fire confined","2":"207","3":"0.0000908780"},{"1":"420 - Toxic condition, other","2":"190","3":"0.0000834146"},{"1":"162 - Outside equipment fire","2":"181","3":"0.0000794634"},{"1":"421 - Chemical hazard (no spill or leak)","2":"180","3":"0.0000790243"},{"1":"423 - Refrigeration leak","2":"172","3":"0.0000755122"},{"1":"381 - Rescue or EMS standby","2":"171","3":"0.0000750731"},{"1":"641 - Vicinity alarm (incident in other location)","2":"169","3":"0.0000741951"},{"1":"653 - Smoke from barbecue, tar kettle","2":"159","3":"0.0000698048"},{"1":"713 - Telephone, malicious false alarm","2":"146","3":"0.0000640975"},{"1":"371 - Electrocution or potential electrocution","2":"144","3":"0.0000632195"},{"1":"4401 - Compact Fluorescent Bulb","2":"144","3":"0.0000632195"},{"1":"243 - Fireworks explosion (no fire)","2":"142","3":"0.0000623414"},{"1":"240 - Explosion (no fire), other","2":"136","3":"0.0000597073"},{"1":"210 - Overpressure rupture from steam, other","2":"133","3":"0.0000583902"},{"1":"133 - Rail vehicle fire","2":"128","3":"0.0000561951"},{"1":"812 - Flood assessment","2":"109","3":"0.0000478536"},{"1":"741 - Sprinkler activation, no fire - unintentional","2":"101","3":"0.0000443414"},{"1":"715 - Local alarm system, malicious false alarm","2":"95","3":"0.0000417073"},{"1":"154 - Dumpster or other outside trash receptacle fire","2":"94","3":"0.0000412683"},{"1":"251 - Excessive heat, scorch burns with no ignition","2":"92","3":"0.0000403902"},{"1":"462 - Aircraft standby","2":"91","3":"0.0000399512"},{"1":"364 - Surf rescue","2":"83","3":"0.0000364390"},{"1":"340 - Search for lost person, other","2":"82","3":"0.0000360000"},{"1":"220 - Overpressure rupture from air or gas, other","2":"78","3":"0.0000342439"},{"1":"361 - Swimming/recreational water areas rescue","2":"78","3":"0.0000342439"},{"1":"541 - Animal problem","2":"78","3":"0.0000342439"},{"1":"711 - Municipal alarm system, malicious false alarm","2":"73","3":"0.0000320488"},{"1":"140 - Natural vegetation fire, other","2":"72","3":"0.0000316097"},{"1":"571A - Relocation","2":"72","3":"0.0000316097"},{"1":"540 - Animal problem, other","2":"71","3":"0.0000311707"},{"1":"714 - Central station, malicious false alarm","2":"69","3":"0.0000302927"},{"1":"443 - Breakdown of light ballast","2":"64","3":"0.0000280975"},{"1":"571 - Cover assignment, standby, moveup","2":"64","3":"0.0000280975"},{"1":"211 - Overpressure rupture of steam pipe or pipeline","2":"60","3":"0.0000263414"},{"1":"481 - Attempt to burn","2":"59","3":"0.0000259024"},{"1":"134 - Water vehicle fire","2":"58","3":"0.0000254634"},{"1":"480 - Attempted burning, illegal action, other","2":"58","3":"0.0000254634"},{"1":"355 - Confined space rescue","2":"55","3":"0.0000241463"},{"1":"357 - Extrication of victim(s) from machinery","2":"52","3":"0.0000228293"},{"1":"712 - Direct tie to FD, malicious false alarm","2":"50","3":"0.0000219512"},{"1":"341 - Search for person on land","2":"49","3":"0.0000215122"},{"1":"521 - Water evacuation","2":"49","3":"0.0000215122"},{"1":"354 - Trench/below-grade rescue","2":"47","3":"0.0000206341"},{"1":"141 - Forest, woods or wildland fire","2":"46","3":"0.0000201951"},{"1":"800 - Severe weather or natural disaster, other","2":"46","3":"0.0000201951"},{"1":"672 - Biological hazard investigation, none found","2":"45","3":"0.0000197561"},{"1":"482 - Threat to burn","2":"42","3":"0.0000184390"},{"1":"223 - Air or gas rupture of pressure or process vessel","2":"38","3":"0.0000166829"},{"1":"143 - Grass fire","2":"36","3":"0.0000158049"},{"1":"356 - High-angle rescue","2":"35","3":"0.0000153658"},{"1":"813 - Wind storm, tornado/hurricane assessment","2":"35","3":"0.0000153658"},{"1":"451 - Biological hazard, confirmed or suspected","2":"34","3":"0.0000149268"},{"1":"161 - Outside storage fire","2":"33","3":"0.0000144878"},{"1":"213 - Steam rupture of pressure or process vessel","2":"31","3":"0.0000136097"},{"1":"734 - Heat detector activation due to malfunction","2":"30","3":"0.0000131707"},{"1":"221 - Overpressure rupture of air or gas pipe/pipeline","2":"29","3":"0.0000127317"},{"1":"363 - Swift water rescue","2":"28","3":"0.0000122927"},{"1":"163 - Outside gas or vapor combustion explosion","2":"25","3":"0.0000109756"},{"1":"343 - Search for person underground","2":"25","3":"0.0000109756"},{"1":"631 - Authorized controlled burning","2":"25","3":"0.0000109756"},{"1":"212 - Overpressure rupture of steam boiler","2":"24","3":"0.0000105366"},{"1":"164 - Outside mailbox fire","2":"23","3":"0.0000100976"},{"1":"138 - Off-road vehicle or heavy equipment fire","2":"22","3":"0.0000096585"},{"1":"471 - Explosive, bomb removal (for bomb scare, use 721)","2":"20","3":"0.0000087805"},{"1":"561 - Unauthorized burning","2":"18","3":"0.0000079024"},{"1":"120 - Fire in mobile prop. used as a fixed struc., other","2":"17","3":"0.0000074634"},{"1":"244 - Dust explosion (no fire)","2":"15","3":"0.0000065854"},{"1":"123 - Fire in portable building, fixed location","2":"14","3":"0.0000061463"},{"1":"222 - Overpressure rupture of boiler from air or gas","2":"13","3":"0.0000057073"},{"1":"362 - Ice rescue","2":"13","3":"0.0000057073"},{"1":"153 - Construction or demolition landfill fire","2":"12","3":"0.0000052683"},{"1":"814 - Lightning strike (no fire)","2":"12","3":"0.0000052683"},{"1":"430 - Radioactive condition, other","2":"10","3":"0.0000043902"},{"1":"241 - Munitions or bomb explosion (no fire)","2":"8","3":"0.0000035122"},{"1":"815 - Severe weather or natural disaster standby","2":"8","3":"0.0000035122"},{"1":"152 - Garbage dump or sanitary landfill fire","2":"7","3":"0.0000030732"},{"1":"372 - Trapped by power lines","2":"7","3":"0.0000030732"},{"1":"811 - Earthquake assessment","2":"7","3":"0.0000030732"},{"1":"231 - Chemical reaction rupture of process vessel","2":"6","3":"0.0000026341"},{"1":"242 - Blasting agent explosion (no fire)","2":"6","3":"0.0000026341"},{"1":"431 - Radiation leak, radioactive material","2":"6","3":"0.0000026341"},{"1":"122 - Fire in motor home, camper, recreational vehicle","2":"5","3":"0.0000021951"},{"1":"155 - Outside stationary compactor/compacted trash fire","2":"4","3":"0.0000017561"},{"1":"751 - Biological hazard, malicious false report","2":"4","3":"0.0000017561"},{"1":"121 - Fire in mobile home used as fixed residence","2":"3","3":"0.0000013171"},{"1":"135 - Aircraft fire","2":"3","3":"0.0000013171"},{"1":"136 - Self-propelled motor home or recreational vehicle","2":"2","3":"0.0000008780"},{"1":"170 - Cultivated vegetation, crop fire, other","2":"2","3":"0.0000008780"},{"1":"173 - Cultivated trees or nursery stock fire","2":"2","3":"0.0000008780"},{"1":"632 - Prescribed fire","2":"2","3":"0.0000008780"},{"1":"172 - Cultivated orchard or vineyard fire","2":"1","3":"0.0000004390"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>


```r
#question: How many times more likely is an incident in Staten Island a false call compared to in Manhattan? 
false_calls_island <- incidents %>%
  select(BOROUGH_DESC,INCIDENT_TYPE_DESC) %>% 
  filter(str_detect(BOROUGH_DESC,"Staten Island")) %>% 
  filter(INCIDENT_TYPE_DESC == "710 - Malicious, mischievous false call, other") %>%
  count()

total_calls_island <- incidents %>% 
  select(BOROUGH_DESC, INCIDENT_TYPE_DESC) %>% 
  filter(str_detect(BOROUGH_DESC, "Staten Island")) %>% 
  count()

false_rate_island = format(round(false_calls_island / total_calls_island, 10), nsmall =10)
false_rate_island <- as.double(false_rate_island)

false_calls_Manhattan <- incidents %>%
  select(BOROUGH_DESC,INCIDENT_TYPE_DESC) %>% 
  filter(str_detect(BOROUGH_DESC,"Manhattan")) %>% 
  filter(INCIDENT_TYPE_DESC == "710 - Malicious, mischievous false call, other") %>%
  count()

total_calls_Manhattan <- incidents %>% 
  select(BOROUGH_DESC, INCIDENT_TYPE_DESC) %>% 
  filter(str_detect(BOROUGH_DESC, "Manhattan")) %>% 
  count()

false_rate_Manhattan = format(round(false_calls_Manhattan / total_calls_Manhattan, 10), nsmall=10)
false_rate_Manhattan <- as.double(false_rate_Manhattan)

(ratio = format(round(false_rate_island/false_rate_Manhattan,10), nsmall=9))
```

```
## [1] "1.624381949"
```


```r
#question: Compute what proportion of all incidents are cooking fires for every hour of the day by normalizing the number of cooking fires in a given hour by the total number of incidents that occured in that hour. Find the hour of the day that has the highest proportion of cooking fires and submit that proportion of cooking fires.
library(lubridate)
```

```
## 
## Attaching package: 'lubridate'
```

```
## The following object is masked from 'package:base':
## 
##     date
```

```r
incidents %>% 
  select(INCIDENT_DATE_TIME, INCIDENT_TYPE_DESC) %>%
  filter(INCIDENT_TYPE_DESC == "113 - Cooking fire, confined to container") %>% 
  mutate(date_time = mdy_hms(INCIDENT_DATE_TIME)) %>% 
  mutate(hour_incident = hour(date_time)) %>% 
  group_by(hour_incident) %>% 
  summarise(cooking_fire_incidents = n()) %>% 
  inner_join(incidents %>%
               select(INCIDENT_DATE_TIME, INCIDENT_TYPE_DESC) %>%
               mutate(date_time = mdy_hms(INCIDENT_DATE_TIME)) %>% 
               mutate(hour_incident = hour(date_time)) %>% 
               group_by(hour_incident) %>% 
               summarise(total_incidents = n()), by = "hour_incident") %>% 
  mutate(cooking_fire_incident_ratio = format(round(cooking_fire_incidents/total_incidents, 10), nsmall = 10)) %>% 
  arrange(desc(cooking_fire_incident_ratio))
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["hour_incident"],"name":[1],"type":["int"],"align":["right"]},{"label":["cooking_fire_incidents"],"name":[2],"type":["int"],"align":["right"]},{"label":["total_incidents"],"name":[3],"type":["int"],"align":["right"]},{"label":["cooking_fire_incident_ratio"],"name":[4],"type":["chr"],"align":["left"]}],"data":[{"1":"18","2":"6806","3":"133853","4":"0.0508468245"},{"1":"19","2":"6456","3":"127787","4":"0.0505215711"},{"1":"17","2":"6541","3":"136283","4":"0.0479957148"},{"1":"20","2":"5451","3":"120731","4":"0.0451499615"},{"1":"16","2":"5680","3":"128304","4":"0.0442698591"},{"1":"12","2":"5000","3":"117203","4":"0.0426610240"},{"1":"15","2":"5150","3":"126410","4":"0.0407404477"},{"1":"13","2":"4963","3":"121908","4":"0.0407110280"},{"1":"14","2":"4979","3":"126016","4":"0.0395108558"},{"1":"11","2":"4464","3":"113931","4":"0.0391816099"},{"1":"21","2":"4284","3":"110342","4":"0.0388247449"},{"1":"10","2":"3983","3":"109463","4":"0.0363867243"},{"1":"9","2":"3503","3":"102894","4":"0.0340447451"},{"1":"22","2":"3140","3":"99051","4":"0.0317008410"},{"1":"8","2":"2818","3":"91402","4":"0.0308308352"},{"1":"23","2":"2530","3":"88474","4":"0.0285959717"},{"1":"0","2":"2047","3":"71631","4":"0.0285770127"},{"1":"6","2":"1455","3":"52133","4":"0.0279093856"},{"1":"7","2":"2007","3":"72671","4":"0.0276176191"},{"1":"1","2":"1571","3":"57394","4":"0.0273721992"},{"1":"3","2":"1059","3":"41313","4":"0.0256335778"},{"1":"2","2":"1203","3":"47079","4":"0.0255527942"},{"1":"4","2":"951","3":"39658","4":"0.0239800293"},{"1":"5","2":"998","3":"41848","4":"0.0238482126"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>


```r
#question: only consider incidents that have information about whether a CO detector was present or not. For events with CO detector and for those without one, compute the proportion of incidents that lasted 20-30, 30-40, 40-50, 50-60, and 60-70 minutes (both interval boundary values included) by dividing the number of incidents in each time interval with the total number of incidents. For each bin, compute the ratio of the 'CO detector absent' frequency to the 'CO detector present' frequency. Perform a linear regression of this ratio to the mid-point of the bins. From this, what is the predicted ratio for events lasting 39 minutes?

#co detector present
co_detector_present_freq <- incidents %>%
  filter(!is.na(CO_DETECTOR_PRESENT_DESC)) %>%
  filter(CO_DETECTOR_PRESENT_DESC == "Yes") %>%
  mutate(total_incident_duration_min = TOTAL_INCIDENT_DURATION/60) %>% 
  group_by(groups_duration = cut(total_incident_duration_min,breaks = c(20,30,40,50,60,70), include.lowest = TRUE)) %>%
  filter(!is.na(groups_duration)) %>% 
  summarise(co_detector_present_count = n()) %>% 
  mutate(co_detector_present_freq = co_detector_present_count / sum(co_detector_present_count))

#Frequency of co detector present in each group
co_detector_present_freq
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["groups_duration"],"name":[1],"type":["fctr"],"align":["left"]},{"label":["co_detector_present_count"],"name":[2],"type":["int"],"align":["right"]},{"label":["co_detector_present_freq"],"name":[3],"type":["dbl"],"align":["right"]}],"data":[{"1":"[20,30]","2":"6501","3":"0.63208556"},{"1":"(30,40]","2":"2145","3":"0.20855615"},{"1":"(40,50]","2":"923","3":"0.08974234"},{"1":"(50,60]","2":"481","3":"0.04676714"},{"1":"(60,70]","2":"235","3":"0.02284881"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

```r
#co detector absent
co_detector_absent_freq <- incidents %>%
  filter(!is.na(CO_DETECTOR_PRESENT_DESC)) %>%
  filter(CO_DETECTOR_PRESENT_DESC == "No") %>%
  mutate(total_incident_duration_min = TOTAL_INCIDENT_DURATION/60) %>% 
  group_by(groups_duration = cut(total_incident_duration_min,breaks = c(20,30,40,50,60,70), include.lowest = TRUE)) %>%
  filter(!is.na(groups_duration)) %>% 
  summarise(co_detector_absent_count = n()) %>%
  mutate(co_detector_absent_freq = co_detector_absent_count / sum(co_detector_absent_count))

#Frequency of co detector absent in each group
co_detector_absent_freq
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["groups_duration"],"name":[1],"type":["fctr"],"align":["left"]},{"label":["co_detector_absent_count"],"name":[2],"type":["int"],"align":["right"]},{"label":["co_detector_absent_freq"],"name":[3],"type":["dbl"],"align":["right"]}],"data":[{"1":"[20,30]","2":"1181","3":"0.46827914"},{"1":"(30,40]","2":"586","3":"0.23235527"},{"1":"(40,50]","2":"348","3":"0.13798573"},{"1":"(50,60]","2":"249","3":"0.09873117"},{"1":"(60,70]","2":"158","3":"0.06264869"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

```r
proportions_co <- co_detector_present_freq %>%
  left_join(co_detector_absent_freq, by = "groups_duration") %>% 
  mutate(proportion_incidents = co_detector_absent_freq/co_detector_present_freq) %>%
  select(proportion_incidents,groups_duration)

#Proportion incidents along with groups mid points
proportions_co$group.mid <- nrow(proportions_co)
proportions_co$group.mid <- c(25,35,45,55,65)

#let's replace groups with midpoints before building linear regression model
lm_model <- lm(proportion_incidents~group.mid, data = proportions_co)
summary(lm_model)
```

```
## 
## Call:
## lm(formula = proportion_incidents ~ group.mid, data = proportions_co)
## 
## Residuals:
##        1        2        3        4        5 
##  0.09155 -0.03509 -0.11153 -0.03789  0.09296 
## 
## Coefficients:
##              Estimate Std. Error t value Pr(>|t|)    
## (Intercept) -0.600475   0.154368   -3.89 0.030123 *  
## group.mid    0.049991   0.003273   15.28 0.000609 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 0.1035 on 3 degrees of freedom
## Multiple R-squared:  0.9873,	Adjusted R-squared:  0.9831 
## F-statistic: 233.3 on 1 and 3 DF,  p-value: 0.0006093
```

```r
#Let's predicted ratio for events lasting 39 minutes
prediction <- predict(lm_model, newdata = data.frame(group.mid = 39))
format(round(prediction,10), nsmall = 9)
```

```
##             1 
## "1.349163741"
```


```r
#question: What is the ratio of the average number of units that arrive to a scene of an incident classified as '111 - Building fire' to the number that arrive for '651 - Smoke scare, odor of smoke'?
#average number of units that arrive to a scene of an incident classified as '111 - Building fire'
avg_units_111 <- incidents %>% 
  filter(INCIDENT_TYPE_DESC == "111 - Building fire") %>% 
  summarise(avg.units = mean(UNITS_ONSCENE, na.rm = TRUE))
avg_units_111
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["avg.units"],"name":[1],"type":["dbl"],"align":["right"]}],"data":[{"1":"11.08464"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

```r
#average number of units that arrive for '651 - Smoke scare, odor of smoke'
avg_units_651 <- incidents %>% 
  filter(INCIDENT_TYPE_DESC == "651 - Smoke scare, odor of smoke") %>% 
  summarise(avg.units = mean(UNITS_ONSCENE, na.rm = TRUE))
avg_units_651
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["avg.units"],"name":[1],"type":["dbl"],"align":["right"]}],"data":[{"1":"4.016524"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

```r
#Ratio
(ratio_avg_units <- format(round(avg_units_111/avg_units_651,10), nsmall =9))
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":[""],"name":["_rn_"],"type":[""],"align":["left"]},{"label":["avg.units"],"name":[1],"type":["S3: AsIs"],"align":["right"]}],"data":[{"1":"2.759759514","_rn_":"1"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>


```r
# question: Check the distribution of the number of min. it takes between the time a '111 - Building fire' incident has been logged into system and the time at which the first unit arrives on scene. What is the third quartile of distribution.
incident.time <- incidents %>% 
  filter(INCIDENT_TYPE_DESC == "111 - Building fire") %>% 
  mutate(incident_dt_tm = mdy_hms(INCIDENT_DATE_TIME)) %>% 
  select(incident_dt_tm)

arrival.time <- incidents %>% 
  filter(INCIDENT_TYPE_DESC == "111 - Building fire") %>% 
  mutate(arrival_dt_tm = mdy_hms(ARRIVAL_DATE_TIME)) %>% 
  select(arrival_dt_tm)

#let's create interval object
elapsed_time <- incident.time$incident_dt_tm %--% arrival.time$arrival_dt_tm

#Duration in minutes from interval object
duration_object <- as.duration(elapsed_time)/dminutes(1)

format(round(quantile(duration_object, na.rm = TRUE)[4], 10), nsmall = 9)
```

```
##           75% 
## "4.150000000"
```


```r
#question : What is the coefficient of determination (R squared) between the number of residents at each zip code and the number of inicidents whose type is classified as '111 - Building fire' at each of those zip codes.
census_data <- read_csv("2010+Census+Population+By+Zipcode+(ZCTA).csv")
```

```
## Parsed with column specification:
## cols(
##   `Zip Code ZCTA` = col_character(),
##   `2010 Census Population` = col_integer()
## )
```

```r
colnames(census_data) <- c("Zip_Code_ZCTA", "2010_Census_population")

#let's convert zip_code type from integer to character in FDNY dataset because zip_code values are stored as character in census dataset.
incidents$ZIP_CODE <- as.character(incidents$ZIP_CODE)

#Join census data and FDNY dataset
residents_fire_data <- census_data %>% 
  left_join(incidents %>% 
              select(INCIDENT_TYPE_DESC, ZIP_CODE) %>% 
              filter(INCIDENT_TYPE_DESC == "111 - Building fire" ) %>% 
              group_by(ZIP_CODE) %>% 
              summarise(incident_111_building_fire_count = n()), by = c("Zip_Code_ZCTA" = "ZIP_CODE")) %>% 
  filter(!is.na(incident_111_building_fire_count))

#build regression model
lm_model_residents_fire <- lm(incident_111_building_fire_count ~ `2010_Census_population`, data = residents_fire_data)
#r squared value
format(round(summary(lm_model_residents_fire)$r.squared,10), nsmall = 10)
```

```
## [1] "0.5973393949"
```


```r
#question: Calculate the chi-square test statistic for testing whether an incident is more likely to last longer than 60 minutes when CO detector is not present.
chi_sq_data <- incidents %>% 
  select(CO_DETECTOR_PRESENT_DESC, TOTAL_INCIDENT_DURATION) %>% 
  filter(!is.na(CO_DETECTOR_PRESENT_DESC)) %>% 
  mutate(incident_duration_less_than_60 = ifelse(TOTAL_INCIDENT_DURATION/60 < 60, "Yes", "No")) %>% 
  select(CO_DETECTOR_PRESENT_DESC, incident_duration_less_than_60)

#chi-square test: null hypothesis: co-detector absent and incident duration greater than 60 are completely independant
#alternate hypothesis: when co-detector is absent then incident duration is likely to be greater than 60 minutes

# Yate's correction is used to avoid overestimation of statistical significance of small data but we have ample data points so let's set it to false and apply chi-square test.
chisq.test(chi_sq_data$CO_DETECTOR_PRESENT_DESC, chi_sq_data$incident_duration_less_than_60, correct = FALSE)
```

```
## 
## 	Pearson's Chi-squared test
## 
## data:  chi_sq_data$CO_DETECTOR_PRESENT_DESC and chi_sq_data$incident_duration_less_than_60
## X-squared = 1171.3, df = 1, p-value < 2.2e-16
```

```r
format(round(chisq.test(chi_sq_data$CO_DETECTOR_PRESENT_DESC, chi_sq_data$incident_duration_less_than_60, correct = FALSE)$statistic,10),nsmall = 6)
```

```
##     X-squared 
## "1171.333976"
```

```r
#By looking at X-squared = 1171.3 and  p-value < 2.2e-16, since p value is very less, we reject null hypothesis. It means when co-detector is absent then incident duration is likely to be greater than 60 minutes
```



#Section 2

```r
#Question: What is the expected value of A when N=10, M=5, and T=20 ?
# positions = 10
# cars = 5
# rounds = 20

#let's create 5 different vectors to store positions for car 1 to 5
car1 <- vector(mode = "integer", length =20)
car2 <- vector(mode = "integer", length =20)
car3 <- vector(mode = "integer", length =20)
car4 <- vector(mode = "integer", length =20)
car5 <- vector(mode = "integer", length =20)

#let's assign initial positions to all 5 cars
car5[1] <- 4
car4[1] <- 3
car3[1] <- 2
car2[1] <- 1
car1[1] <- 0

#update car5 positions in each round until it reaches position 20
i <- 2
while (car5[i-1]<10) {
  car5[i] <- i + 3
  i = i +1
}

car5 <- replace(car5,car5==0,10)

#update car4 positions if car5 has reached 10 position (these can be updated till 9 since car5 is at 10)
for (i in 2:20) {
  ifelse(car5[i-1] == 10, car4[i] <- car4[i-1] + 1, car4[i] <- car4[1])
}

#update car3 positions if car4 has reached 19 position (these can be updated till 8 since car4 will be at 9)
for (i in 2:20) {
  ifelse(car4[i-1] == 9, car3[i] <- car3[i-1] + 1, car3[i] <- car3[1])
}

#update car2 positions if car3 has reached 18 position (these can be updated till 7 since car3 will be at 8)
for (i in 2:20) {
  ifelse(car3[i-1] == 8, car2[i] <- car2[i-1] + 1, car2[i] <- car2[1])
}

#update car1 positions if car2 has reached 17 position (these can be updated till 6 since car2 will be at 7)
for (i in 2:20) {
  ifelse(car2[i-1] == 7, car1[i] <- car1[i-1] + 1, car1[i] <- car1[1])
}

#Car positions
car5
```

```
##  [1]  4  5  6  7  8  9 10 10 10 10 10 10 10 10 10 10 10 10 10 10
```

```r
car4
```

```
##  [1]  3  3  3  3  3  3  3  4  5  6  7  8  9 10 11 12 13 14 15 16
```

```r
car3
```

```
##  [1] 2 2 2 2 2 2 2 2 2 2 2 2 2 3 2 2 2 2 2 2
```

```r
car2
```

```
##  [1] 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1
```

```r
car1
```

```
##  [1] 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
```

```r
car_positions <- cbind.data.frame(car1, car2, car3, car4, car5)

#Let's calculcate average (A) for all positions of car 1 to 5
avg_car1 <- mean(car_positions$car1)
avg_car2 <- mean(car_positions$car2)
avg_car3 <- mean(car_positions$car3)
avg_car4 <- mean(car_positions$car4)
avg_car5 <- mean(car_positions$car5)

probabilty_val <- 0.2 #since equal probability is assigned to each

expected_val_avg <- avg_car1 * probabilty_val + avg_car2 * probabilty_val + avg_car3 * probabilty_val + avg_car4 * probabilty_val + avg_car5 * probabilty_val

format(round(expected_val_avg,10), nsmall = 9)
```

```
## [1] "3.910000000"
```

```r
#Question: What is the expected value of S when N=10, M=5, and T=20 ?
#Let's calculcate standard deviation (S) for all positions of car 1 to 5
std_dev1 <- sd(car_positions$car1)
std_dev2 <- sd(car_positions$car2)
std_dev3 <- sd(car_positions$car3)
std_dev4 <- sd(car_positions$car4)
std_dev5 <- sd(car_positions$car5)

expected_val_std_dev <- std_dev1 * probabilty_val + std_dev2 * probabilty_val + std_dev3 * probabilty_val + std_dev4 * probabilty_val + std_dev5 * probabilty_val

format(round(expected_val_std_dev,10), nsmall = 9)
```

```
## [1] "1.349040801"
```

```r
#Question: What is the standard deviation of A when N=10, M=5, and T=20?
format(round(sd(c(avg_car1, avg_car2, avg_car3, avg_car4, avg_car5)),10), nsmall = 9)
```

```
## [1] "4.057924346"
```

```r
#Question: What is the standard deviation of S when N=10, M=5, and T=20?
format(round(sd(c(std_dev1, std_dev2, std_dev3, std_dev4, std_dev5)),10), nsmall = 9)
```

```
## [1] "1.993273053"
```


```r
#Question: What is the expected value of A when N=25, M=10, and T=50 ?
#here _2 in nomination of variable is done to indicate second set of cars for part 2 of section 2
#positions = 25
#cars = 10
#rounds = 50

#let's create 5 different vectors to store positions for car 1 to 10
car1_2 <- vector(mode = "integer", length =50)
car2_2 <- vector(mode = "integer", length =50)
car3_2 <- vector(mode = "integer", length =50)
car4_2 <- vector(mode = "integer", length =50)
car5_2 <- vector(mode = "integer", length =50)
car6_2 <- vector(mode = "integer", length =50)
car7_2 <- vector(mode = "integer", length =50)
car8_2 <- vector(mode = "integer", length =50)
car9_2 <- vector(mode = "integer", length =50)
car10_2 <- vector(mode = "integer", length =50)


#let's assign initial positions to all 5 cars
car10_2[1] <- 9
car9_2[1] <- 8
car8_2[1] <- 7
car7_2[1] <- 6
car6_2[1] <- 5
car5_2[1] <- 4
car4_2[1] <- 3
car3_2[1] <- 2
car2_2[1] <- 1
car1_2[1] <- 0

#update car10_2 positions in each round until it reaches position 25
i <- 2
while(car10_2[i-1]< 25) {
  car10_2[i] <- i + 8
  i = i +1
}

car10_2 <- replace(car10_2,car10_2==0,25)

#update car9_2 positions if car10_2 has reached 25 position (these can be updated till 24 since car10_2 is at 25 at end of possible rounds to move forward)
for (i in 2:50) {
  ifelse(car10_2[i-1] == 25,  ifelse(car9_2[i-1] == 24, car9_2[i] <- 24, car9_2[i] <- car9_2[i-1] + 1), car9_2[i] <- car9_2[1])
}

#update car8_2 positions if car9_2 has reached 24 position (these can be updated till 23 since car9_2 will possibly be at 24 at end of rounds)
for (i in 2:50) {
  ifelse(car9_2[i-1] == 24, ifelse(car8_2[i-1] == 23, car8_2[i] <- 23, car8_2[i] <- car8_2[i-1] + 1), car8_2[i] <- car8_2[1])
}

#update car7_2 positions if car8_2 has reached 23 position (these can be updated till 22 since car8_2 will possibly be at 23 at end of rounds)
for (i in 2:50) {
  ifelse(car8_2[i-1] == 23, ifelse(car7_2[i-1] == 22, car7_2[i] <- 22, car7_2[i] <- car7_2[i-1] + 1), car7_2[i] <- car7_2[1])
}

#update car6_2 positions if car7_2 has reached 22 position (these can be updated till 21 since car7_2 will possibly be at 22 at end of rounds)
for (i in 2:50) {
  ifelse(car7_2[i-1] == 22, ifelse(car6_2[i-1] == 21, car6_2[i] <- 21, car6_2[i] <- car6_2[i-1] + 1), car6_2[i] <- car6_2[1])
}

#update car5_2 positions if car6_2 has reached 21 position (these can be updated till 20 since car6_2 will possibly be at 21 at end of rounds)
for (i in 2:50) {
  ifelse(car6_2[i-1] == 21, ifelse(car5_2[i-1] == 20, car5_2[i] <- 20, car5_2[i] <- car5_2[i-1] + 1), car5_2[i] <- car5_2[1])
}

#update car4_2 positions if car5_2 has reached 20 position (these can be updated till 19 since car5_2 will possibly be at 20 at end of rounds)
for (i in 2:50) {
  ifelse(car5_2[i-1] == 20, ifelse(car4_2[i-1] == 19, car4_2[i] <- 19, car4_2[i] <- car4_2[i-1] + 1), car4_2[i] <- car4_2[1])
}


#update car3_2 positions if car4_2 has reached  19 position (these can be updated till 18 since car4_2 will possibly be at 19 at end of rounds)
for (i in 2:50) {
  ifelse(car4_2[i-1] == 19, ifelse(car3_2[i-1] == 18, car3_2[i] <- 18, car3_2[i] <- car3_2[i-1] + 1), car3_2[i] <- car3_2[1])
}


#update car2_2 positions if car5_2 has reached 18 position (these can be updated till 17 since car3_2 will possibly be at 18 at end of rounds)
for (i in 2:50) {
  ifelse(car3_2[i-1] == 18, ifelse(car2_2[i-1] == 17, car2_2[i] <- 17, car2_2[i] <- car2_2[i-1] + 1), car2_2[i] <- car2_2[1])
}


#update car1_2 positions if car5_2 has reached 17 position (these can be updated till 16 since car2_2 will possibly be at 17 at end of rounds)
for (i in 2:50) {
  ifelse(car2_2[i-1] == 17, ifelse(car1_2[i-1] == 16, car1_2[i] <- 16, car1_2[i] <- car1_2[i-1] + 1), car1_2[i] <- car1_2[1])
}


#Car positions
car10_2
```

```
##  [1]  9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 25 25 25 25 25 25
## [24] 25 25 25 25 25 25 25 25 25 25 25 25 25 25 25 25 25 25 25 25 25 25 25
## [47] 25 25 25 25
```

```r
car9_2
```

```
##  [1]  8  8  8  8  8  8  8  8  8  8  8  8  8  8  8  8  8  9 10 11 12 13 14
## [24] 15 16 17 18 19 20 21 22 23 24 24 24 24 24 24 24 24 24 24 24 24 24 24
## [47] 24 24 24 24
```

```r
car8_2
```

```
##  [1]  7  7  7  7  7  7  7  7  7  7  7  7  7  7  7  7  7  7  7  7  7  7  7
## [24]  7  7  7  7  7  7  7  7  7  7  8  9 10 11 12 13 14 15 16 17 18 19 20
## [47] 21 22 23 23
```

```r
car7_2
```

```
##  [1] 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6
## [36] 6 6 6 6 6 6 6 6 6 6 6 6 6 6 7
```

```r
car6_2
```

```
##  [1] 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5
## [36] 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5
```

```r
car5_2
```

```
##  [1] 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4
## [36] 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4
```

```r
car4_2
```

```
##  [1] 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3
## [36] 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3
```

```r
car3_2
```

```
##  [1] 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2
## [36] 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2
```

```r
car2_2
```

```
##  [1] 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1
## [36] 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1
```

```r
car1_2
```

```
##  [1] 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
## [36] 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
```

```r
car_positions_2 <- cbind.data.frame(car1_2, car2_2, car3_2, car4_2, car5_2, car6_2, car7_2, car8_2, car9_2, car10_2)

#Let's calculcate average (A) for all positions of car 1 to 5
avg_car1_2 <- mean(car_positions_2$car1_2)
avg_car2_2 <- mean(car_positions_2$car2_2)
avg_car3_2 <- mean(car_positions_2$car3_2)
avg_car4_2 <- mean(car_positions_2$car4_2)
avg_car5_2 <- mean(car_positions_2$car5_2)
avg_car6_2 <- mean(car_positions_2$car6_2)
avg_car7_2 <- mean(car_positions_2$car7_2)
avg_car8_2 <- mean(car_positions_2$car8_2)
avg_car9_2 <- mean(car_positions_2$car9_2)
avg_car10_2 <- mean(car_positions_2$car10_2)

probabilty_val_2 <- 1/10 #since equal probability is assigned to each

expected_val_avg_2 <- avg_car1_2 * probabilty_val_2 + avg_car2_2 * probabilty_val_2 + avg_car3_2 * probabilty_val_2 + avg_car4_2 * probabilty_val_2 + avg_car5_2 * probabilty_val_2 + avg_car6_2 * probabilty_val_2 + avg_car7_2 * probabilty_val_2 + avg_car8_2 * probabilty_val_2 + avg_car9_2 * probabilty_val_2 + avg_car10_2 * probabilty_val_2

format(round(expected_val_avg_2,10), nsmall = 9)
```

```
## [1] "6.950000000"
```

```r
#Question: What is the expected value of S when N=25, M=10, and T=50 ?
#Let's calculcate standard deviation (S) for all positions of car 1 to 10
std_dev1_2 <- sd(car_positions_2$car1_2)
std_dev2_2 <- sd(car_positions_2$car2_2)
std_dev3_2 <- sd(car_positions_2$car3_2)
std_dev4_2 <- sd(car_positions_2$car4_2)
std_dev5_2 <- sd(car_positions_2$car5_2)
std_dev6_2 <- sd(car_positions_2$car6_2)
std_dev7_2 <- sd(car_positions_2$car7_2)
std_dev8_2 <- sd(car_positions_2$car8_2)
std_dev9_2 <- sd(car_positions_2$car9_2)
std_dev10_2 <- sd(car_positions_2$car10_2)

expected_val_std_dev_2 <- std_dev1_2 * probabilty_val_2 + std_dev2_2 * probabilty_val_2 + std_dev3_2 * probabilty_val_2 + std_dev4_2 * probabilty_val_2 + std_dev5_2 * probabilty_val_2 + std_dev6_2 * probabilty_val_2 + std_dev7_2 * probabilty_val_2 + std_dev8_2 * probabilty_val_2 + std_dev9_2 * probabilty_val_2 +  std_dev10_2 * probabilty_val_2

format(round(expected_val_std_dev_2,10), nsmall = 9)
```

```
## [1] "1.723562459"
```

```r
#Question: What is the standard deviation of A when N=25, M=10, and T=50?
format(round(sd(c(avg_car1_2, avg_car2_2, avg_car3_2, avg_car4_2, avg_car5_2, avg_car6_2,avg_car7_2,avg_car8_2,avg_car9_2,avg_car10_2)),10), nsmall = 9)
```

```
## [1] "7.200007716"
```

```r
#Question: What is the standard deviation of S when N=25, M=10, and T=50?
format(round(sd(c(std_dev1_2, std_dev2_2, std_dev3_2, std_dev4_2, std_dev5_2, std_dev6_2, std_dev7_2, std_dev8_2, std_dev9_2, std_dev10_2)),10), nsmall = 9)
```

```
## [1] "2.809149652"
```

#Section 3

```r
#Read boston crime data
crime_data_boston <- read_csv("crime.csv")
```

```
## Parsed with column specification:
## cols(
##   INCIDENT_NUMBER = col_character(),
##   OFFENSE_CODE = col_character(),
##   OFFENSE_CODE_GROUP = col_character(),
##   OFFENSE_DESCRIPTION = col_character(),
##   DISTRICT = col_character(),
##   REPORTING_AREA = col_integer(),
##   SHOOTING = col_character(),
##   OCCURRED_ON_DATE = col_datetime(format = ""),
##   YEAR = col_integer(),
##   MONTH = col_integer(),
##   DAY_OF_WEEK = col_character(),
##   HOUR = col_integer(),
##   UCR_PART = col_character(),
##   STREET = col_character(),
##   Lat = col_double(),
##   Long = col_double(),
##   Location = col_character()
## )
```

```r
#Check if data is loaded properly
head(crime_data_boston)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["INCIDENT_NUMBER"],"name":[1],"type":["chr"],"align":["left"]},{"label":["OFFENSE_CODE"],"name":[2],"type":["chr"],"align":["left"]},{"label":["OFFENSE_CODE_GROUP"],"name":[3],"type":["chr"],"align":["left"]},{"label":["OFFENSE_DESCRIPTION"],"name":[4],"type":["chr"],"align":["left"]},{"label":["DISTRICT"],"name":[5],"type":["chr"],"align":["left"]},{"label":["REPORTING_AREA"],"name":[6],"type":["int"],"align":["right"]},{"label":["SHOOTING"],"name":[7],"type":["chr"],"align":["left"]},{"label":["OCCURRED_ON_DATE"],"name":[8],"type":["S3: POSIXct"],"align":["right"]},{"label":["YEAR"],"name":[9],"type":["int"],"align":["right"]},{"label":["MONTH"],"name":[10],"type":["int"],"align":["right"]},{"label":["DAY_OF_WEEK"],"name":[11],"type":["chr"],"align":["left"]},{"label":["HOUR"],"name":[12],"type":["int"],"align":["right"]},{"label":["UCR_PART"],"name":[13],"type":["chr"],"align":["left"]},{"label":["STREET"],"name":[14],"type":["chr"],"align":["left"]},{"label":["Lat"],"name":[15],"type":["dbl"],"align":["right"]},{"label":["Long"],"name":[16],"type":["dbl"],"align":["right"]},{"label":["Location"],"name":[17],"type":["chr"],"align":["left"]}],"data":[{"1":"I182080058","2":"02403","3":"Disorderly Conduct","4":"DISTURBING THE PEACE","5":"E18","6":"495","7":"NA","8":"2018-10-03 20:13:00","9":"2018","10":"10","11":"Wednesday","12":"20","13":"Part Two","14":"ARLINGTON ST","15":"42.26261","16":"-71.12119","17":"(42.26260773, -71.12118637)"},{"1":"I182080053","2":"03201","3":"Property Lost","4":"PROPERTY - LOST","5":"D14","6":"795","7":"NA","8":"2018-08-30 20:00:00","9":"2018","10":"8","11":"Thursday","12":"20","13":"Part Three","14":"ALLSTON ST","15":"42.35211","16":"-71.13531","17":"(42.35211146, -71.13531147)"},{"1":"I182080052","2":"02647","3":"Other","4":"THREATS TO DO BODILY HARM","5":"B2","6":"329","7":"NA","8":"2018-10-03 19:20:00","9":"2018","10":"10","11":"Wednesday","12":"19","13":"Part Two","14":"DEVON ST","15":"42.30813","16":"-71.07693","17":"(42.30812619, -71.07692974)"},{"1":"I182080051","2":"00413","3":"Aggravated Assault","4":"ASSAULT - AGGRAVATED - BATTERY","5":"A1","6":"92","7":"NA","8":"2018-10-03 20:00:00","9":"2018","10":"10","11":"Wednesday","12":"20","13":"Part One","14":"CAMBRIDGE ST","15":"42.35945","16":"-71.05965","17":"(42.35945371, -71.05964817)"},{"1":"I182080050","2":"03122","3":"Aircraft","4":"AIRCRAFT INCIDENTS","5":"A7","6":"36","7":"NA","8":"2018-10-03 20:49:00","9":"2018","10":"10","11":"Wednesday","12":"20","13":"Part Three","14":"PRESCOTT ST","15":"42.37526","16":"-71.02466","17":"(42.37525782, -71.02466343)"},{"1":"I182080049","2":"01402","3":"Vandalism","4":"VANDALISM","5":"C11","6":"351","7":"NA","8":"2018-10-02 20:40:00","9":"2018","10":"10","11":"Tuesday","12":"20","13":"Part Two","14":"DORCHESTER AVE","15":"42.29920","16":"-71.06047","17":"(42.29919694, -71.06046974)"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

```r
#let's do exploratory data analysis
library(tidyverse)

#Plot information about what crimes are most frequently occuring in Boston.This will help law inforcement as well as community supoort group to divert or allocate type of resources.
crime_data_boston %>% 
  select(OFFENSE_CODE_GROUP, DAY_OF_WEEK) %>% 
  group_by(OFFENSE_CODE_GROUP) %>%
  filter(OFFENSE_CODE_GROUP != "Other") %>% 
  summarise(crime_frequency = n()) %>% 
  arrange(desc(crime_frequency)) %>% 
  head(10) %>% 
  ggplot(aes(x = reorder(OFFENSE_CODE_GROUP, desc(crime_frequency)), y = crime_frequency)) +
  geom_col(fill = "orange", color = "black") +
  ggtitle("Most frequently committed crimes in Boston") +
  labs(x = "Crime Type", y = "Crime Frequency", caption = "Data Source: https://www.kaggle.com, 1k = 1000 records") +
  geom_label(aes(x = OFFENSE_CODE_GROUP, y = crime_frequency, label = paste(round(crime_frequency/1000, digits = 2), "k")))+
  theme(axis.text.x = element_text(angle = 45, hjust = 1)) 
```

![](README_figs/README-Most Frequent Crimes-1.png)<!-- -->


```r
#Let's find out the areas where most crimes are committed in Boston.
crime_data_boston %>% 
  select(STREET) %>% 
  group_by(STREET) %>%
  filter(STREET != "NA") %>% 
  summarise(crime_frequency = n()) %>%
  arrange(desc(crime_frequency)) %>% 
  head(10) %>% 
  ggplot(aes(x = reorder(STREET, crime_frequency), y = crime_frequency)) +
  geom_bar(stat = "identity",fill = "orange", color = "black", position = "dodge") +
  coord_flip() +
  geom_label(aes(x = STREET, y = crime_frequency, label = paste(round(crime_frequency/1000, digits = 2), "k"))) +
  ggtitle("Areas where crimes happens most in Boston") +
  labs(x = "Area Name", y = "Crime Frequency", caption = "Data Source: https://www.kaggle.com, 1k = 1000 records")
```

![](README_figs/README-High Crime Areas-1.png)<!-- -->




