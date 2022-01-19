# BMW_M54_E85_remap  "

## BMW M54 E85 map modifications tutorial and files. ##

In this repo, you will find all the E85 modifications i made on my '02 BMW E46 325i ECU.

### In IRL, you need : 
* A BMW E46 with an M54 engine (MS43 ECU)
* A Laptop
* Optionnal : A Spare ECU (you can buy one on EBAY for 50€, don't care if the ECU is blocked with EWS).
* A K-DCAN usb cable.
* Time and patience

### In your laptop you need :
* K-CAN usb cable driver.
* INPA.
* MS4x flasher.
* Tuner Pro.
* Your engine MS430069 version ECU 512k orignial bin file (for example M54B25 for 325i)
* Tuner Pro XDF file for MS430069 version 512k ECU bin file.
* Tuner Pro patchlist XDF file for MS430069 version 512k ECU bin file.

You can download MS4X flasher, bin and xdf files on [ms4x.net](https://www.ms4x.net/index.php?title=Main_Page)

## Before modification ##

It's recommanded to change sparks, carb filter and pump before you put E85 in your car.

## Map modification : ##

All them values are approximate values and each parameters must be fine tune for your engine.

### CAT heating configuration ###

You can switch off the CAT heating to help cold start.
* c_conf_cat = 0.

### Injection tables (easy) : 

With the same quantity of air, the ECU must inject 30% more of E85 than E10 or E5.
*(+30% means a multiplicative value of 1.30 is applied on the table.)*

**Partial load :**

Cold engine (bank 1 and 2)
* ip_ti_tco_1_pl_1_ivvt_n_maf :  *+30%.*
* ip_ti_tco_1_pl_2_ivvt_n_maf :  *+30%.*

Warm engine (bank 1 and 2)
* ip_ti_tco_2_pl_1_ivvt_n_maf :  *+30%.*
* ip_ti_tco_2_pl_2_ivvt_n_maf :  *+30%.*

**Idle :**

Cold engine 
* ip_ti_tco_1_is_ivvt_n_maf :  *+30%.*

Warm engine
* ip_ti_tco_2_is_ivvt_n_maf :  *+30%.*

**Cranking (not easy) :**

Cranking sequence can be divide in three part :
* Pre cold start injection, i think the first pulse of carb injected when the key is turned.
* Cranking injection, the table used during cranking
* Post start enrichment, makes the link between cranking injection table and idle injection table.

Pre injection time basic value
* ip_tipr_cst__tco : *+50% max under 10°C and decrease multiplicative value from 30°C to 90°C . (My car : 30° > 48 ; 60° > 28 ; 90° > 19)*
* Be careful, you can wash your cylinders, do the modification step by step.

Cranking injection time basic value
* ip_ti_cst_n_tco : *+50% for 80rpm and 320rpm line and +35% for 920rpm line.*

Gradual reduction of fuel enrichment at engine start
* ip_ti_cr_cst__cyc__tco : *I don't tune this map yet.

Initialized value for post start enrichment factor 
* ip_ti_cast__tia_tco : *+10% under 30°C maybe more.*

Gradual reduction of fuel enrichment after engine start
* ip_ti_cr_cast__cyc__tco : *I don't tune this map yet.*


### Ignition : 

E85 is more knock resistant than E5 or E10 and has great cooling abilities, so the ECU can command the spark a little bit earlier to get a little more torque.
On this ECU there are two "main" ignition map, one for RON98 and an other for RON91. If the ECU detects knock, it switches the ignition map RON98 to RON91 with less ignition advance to protect the engine. Only the first ignition map will be tune here.

**Partial and full load :**

Target ignition angle for RON98 during part and full load
* ip_iga_ron_98_pl_ivvt_n_maf : *+11% ignition advance, it makes +4deg max on the table. More can be destructive for the engine !*


**Cranking :**

Basing ignition angle at start
* ip_iga_st__n : *Set 3° at 80rpm, 6° at 320rpm and 13.1° at 920.*


### Vanos : 

Intake camshaft setpoint during idle cold engine : 
* ip_cam_sp_tco_1_in_is__n__maf_iv : *Set 120° in all case of the table to avoid camshaft overlap and help cold start.*

### Idle speed : 

Nominal idle speed without additionnal load on the engine : 
* ip_n_sp_is__tco : *Set 1120 @ -9.8° and 900 @ 9.8°. It will help to maintain the idle speed just after a cold start.*

### Optional : immo-off for your spare ECU : 

Open your 512k bin file with patchlist XDF file and apply the immo off patch.

### After modification :

Save your 512k bin file and upload this in your ECU with MS4X flasher.
Erase lambda adaptation with MS4X flasher or INPA.

Test your map and check lambda value and lambda intégrator on INPA : 
* Positive intégrator means your run too lean en negative too rich.
* Check at idle, partial and full load condition (cold and warm engine).
* Adapt your injection map in function of your lambda integrator value.
* If your engine has vaccum leak like my 300k km car you can enrich your injection map more than 30% until you find the leak.
* Don't forget to erase lambda adaptation after every injection map modification.

### Sources : ##

[The bible ms4x.net](https://www.ms4x.net/index.php?title=Main_Page)

[E46 fanatics forum e85 mod tutorial](https://www.e46fanatics.com/threads/m54-e-85-tuning-n-a-ms43.1263149/)
