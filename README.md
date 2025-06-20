# DFID: David Filament RFID 
(Draft under development)

<img src="dfid.png" alt="isolated" width="128"/>

The DFID is a proposal to store and transfer the filament configuration
data with a RFID chip. It is an open implementation and supported by the
DMC(David Multi-material Core). The idea of this ID scheme is for the
convenient using of the filament information. Although certain level of
security is implemented, it does not go extreme considering the cost may
be involved. With the optional RSA1024, a counterfeit ID has been
reasonably prevented, however, the ID clone is still possible.

# Data structure

The DFID contains three parts: an Open Header, a Data Body and an
optional RAS1024 key. The Data Body can be encrypted; while the Open
Header is always in plain data. The data in each part is briefly listed
below with more details in the next session.

Open Header

<table>
<colgroup>
<col style="width: 26%" />
<col style="width: 8%" />
<col style="width: 64%" />
</colgroup>
<thead>
<tr>
<th>Name</th>
<th>bytes</th>
<th>Note</th>
</tr>
</thead>
<tbody>
<tr>
<td>key</td>
<td>4</td>
<td>0xDF1DBABE</td>
</tr>
<tr>
<td>FUID</td>
<td>8</td>
<td>(see next session)</td>
</tr>
<tr>
<td>diameter</td>
<td>1</td>
<td>Round to 0.1mm per unit, e.g. 3mm 30, 1.75mm 18</td>
</tr>
<tr>
<td>material1</td>
<td>1</td>
<td>ABS, PLA, PETG, TPE, TPU</td>
</tr>
<tr>
<td>temp_print_n</td>
<td>2</td>
<td>Recommended print temperature in PTEMP format</td>
</tr>
<tr>
<td>Total header size</td>
<td>16</td>
<td></td>
</tr>
</tbody>
</table>

Data body

<table>
<colgroup>
<col style="width: 28%" />
<col style="width: 8%" />
<col style="width: 63%" />
</colgroup>
<thead>
<tr>
<th>Name</th>
<th>bytes</th>
<th>Note</th>
</tr>
</thead>
<tbody>
<tr>
<td>material2</td>
<td>1</td>
<td>CF, GF, PF(plant fibre), wooden power, iron powder, copper powder,
quartz powder, carbon power, ceramic powder</td>
</tr>
<tr>
<td>colour1</td>
<td>3</td>
<td>Main colour in RGB</td>
</tr>
<tr>
<td>colour2</td>
<td>3</td>
<td>Secondary colour in RGB</td>
</tr>
<tr>
<td>finish</td>
<td>1</td>
<td>3bit transparency level; 5bit surface feeling: normal, matte, satin,
brushed, gloss, high gloss, silk, rock, wooden, metallic</td>
</tr>
<tr>
<td>density</td>
<td>2</td>
<td>8bit integer 8bit fraction</td>
</tr>
<tr>
<td>flow</td>
<td>4</td>
<td>MFI (Melt-flow index)</td>
</tr>
<tr>
<td>temp_print_max</td>
<td>2</td>
<td>Maximum print temperature in PTEMP format</td>
</tr>
<tr>
<td>temp_print_min</td>
<td>2</td>
<td>Minimum print temperature in PTEMP format</td>
</tr>
<tr>
<td>temp_bed_max</td>
<td>1</td>
<td>Max hotbed temperature in BTEMP format</td>
</tr>
<tr>
<td>temp_bed_min</td>
<td>1</td>
<td>Min hotbed temperature in BTEMP format</td>
</tr>
<tr>
<td>temp_print_0</td>
<td>2</td>
<td>Recommended first layer print temperature (PTEMP)</td>
</tr>
<tr>
<td>temp_bed_0</td>
<td>1</td>
<td>Recommended first layer hotbed temp (BTEMP)</td>
</tr>
<tr>
<td>temp_bed_n</td>
<td>1</td>
<td>Recommended continue layer hotbed temp (BTEMP)</td>
</tr>
<tr>
<td>temp_cab</td>
<td>1</td>
<td>Recommended cabin temperature in BTEMP format</td>
</tr>
<tr>
<td>temp_dry</td>
<td>1</td>
<td>Drying temperature (BTEMP)</td>
</tr>
<tr>
<td>temp_store</td>
<td>1</td>
<td>Store temperature (BTEMP)</td>
</tr>
<tr>
<td>speed_print_0_04x10</td>
<td>1</td>
<td>First layer print speed in 10 mm/s at 0.4mm nozzle diameter or
equivalent</td>
</tr>
<tr>
<td>speed_print_n_04x10</td>
<td>1</td>
<td>Continuous layer print speed in 10 mm/s at 0.4mm nozzle diameter or
equivalent</td>
</tr>
<tr>
<td>cooling_fan</td>
<td>1</td>
<td>4bit: first layer; 4bit: continuous print</td>
</tr>
<tr>
<td>weight</td>
<td>2</td>
<td>Filament weight in 0.1gram (filament length can be calculated from
the density)</td>
</tr>
<tr>
<td>spool_weight</td>
<td>2</td>
<td>Spool weight in 0.1gram</td>
</tr>
<tr>
<td>spool_width</td>
<td>2</td>
<td>Spool width in mm</td>
</tr>
<tr>
<td>spool_outer</td>
<td>2</td>
<td>Spool outer diameter in mm</td>
</tr>
<tr>
<td>spool_hole</td>
<td>1</td>
<td>Spool hold diameter in mm</td>
</tr>
<tr>
<td>feature</td>
<td>4</td>
<td>0xffffffff: not defined; other value may indicate: hardened nozzle
required, water absorb, conductive, magnetic, flexible, elastic, UV
resistance, UV sensitive, water soluble, food quality, medical quality,
highly flammable, powder filed, fibre filed, fluorescent, rainbow
colour, supporting material, live material</td>
</tr>
<tr>
<td>Manufacture time</td>
<td>4</td>
<td>Seconds elapsed in 32bit since 2025-01-01 0’00”00</td>
</tr>
<tr>
<td>Vendor name</td>
<td>12</td>
<td>Padded with 0 if shorter</td>
</tr>
<tr>
<td>Brand name</td>
<td>12</td>
<td>Padded with 0 if shorter</td>
</tr>
<tr>
<td>Product name</td>
<td>12</td>
<td>Padded with 0 if shorter</td>
</tr>
<tr>
<td>URL</td>
<td>16</td>
<td>Padded with 0 if shorter</td>
</tr>
<tr>
<td>Serial number</td>
<td>4</td>
<td>Vendor defined, the DMC will just display it in hex format</td>
</tr>
<tr>
<td>User define</td>
<td>16</td>
<td>e.g. the serial number extension</td>
</tr>
<tr>
<td>Reserved</td>
<td>4</td>
<td>random number for the moment</td>
</tr>
<tr>
<td>CRC32</td>
<td>4</td>
<td>CRC32 checksum for all the data above including the header</td>
</tr>
<tr>
<td>Total size</td>
<td>127</td>
<td></td>
</tr>
</tbody>
</table>

RSA1024 public key 128bytes

Example code in C,

Example code in Python,

# Data definition

The FUID is a 64bit filament UUID, consisting of 48bit vendor ID and
16bit product ID, i.e. FUID\_V and FUID\_P. The FUID\_V has A or B
version, which uses the MSB of the 64bit value. The rest 47bits support
14 decimal digits, which include 3 digits area code (same as the
telephone country codes) and 11 digits representative phone number of
the vendor with padding zeros preferably at the beginning of each part.
The 16bit FUID\_P is totally defined by the vendor and the details can
be link to the vendor’s website via the URL provided in the data body.
This scheme would allow the vendors to manage their own filament UUID
without the need for an authoritative allocation.

The A version FUID means open access to the data body, while version B
is for data with the RSA1024 encryption, the public key is also stored
on the chip.

Example: A086\_13510\_099486\_65535 or B044\_01913\_342000\_12345

The PTEMP format is a signed 16bit integer in half kelvin. The value of
0xffff means not defined.

The BTEMP is an unsigned 8bit integer in kelvin with an offset of 220K,
i.e., a ‘0’ means 220K or -53C. The value of 0xff means not defined.

# RFID Implementation

The RFID chip is recommended to use NTAG213 (144 bytes) for the A
version or NTAG215 (504 bytes) for the B version. The F08 chip (for both
ver A and ver B) is also supported.

