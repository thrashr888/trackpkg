# Track Shipment Packages

Blatantly forked and renamed from https://github.com/jwalanta/trackpkg. It hasn't
seen updates in a while and I wanted it under a different name for myself. Thanks
@jwalanta!

Commandline tool for tracking shipment packages. Currently supports UPS, USPS and FedEx.

The tracking information is stored as a json file at `~/.wheres-my-ship`. 

## Building and installing

Assumes that you already have golang

```
$ go build
$ go install
```


## Usage

### Adding tracking number

Syntax: 

`wheres-my-ship add <tracking_number> <description>`

Example:

```
$ wheres-my-ship add 1Z3Y60630359612867 Order from Amazon
$ wheres-my-ship add 568403913053849 Gift for mom
$ wheres-my-ship add 9200191035346014134451 Documents for Bob
```

### Updating tracking information

Adding won't update the tracking information. To update tracking status of all shipments:

`$ wheres-my-ship update`

Example:

```
$ wheres-my-ship update
  1 1Z3Y60630359612867        UPS    Updated - 02/15 07:46 PM Spokane, WA, United States Departure Scan
  2 568403913053849           FEDEX  Updated - 02/16 07:17 AM HUTCHINS, TX Departed FedEx location
  3 9200191035346014134451    USPS   Updated - 02/15 03:34 PM SAINT PAUL, MN 55121 Arrived at USPS Origin Facility
```


### List all packages

Lists all shipments, along with expected delivery date:

Syntax:

`wheres-my-ship list [<tracking_number> | <item_number>]`

Example:

```
$ wheres-my-ship list
  1 1Z3Y60630359612867        UPS    02/21 +5d  Order from Amazon
  2 568403913053849           FEDEX  02/18 +2d  Gift for mom
  3 9200191035346014134451    USPS   02/18 +2d  Documents for Bob
```


### Detail tracking information

Shows all the tracking detail of the shipment. Either tracking number of item number can be used as argument.

Syntax:

`wheres-my-ship detail [<tracking_number> | <item_number>]`

Example:

```
$ wheres-my-ship detail 1
  1 1Z3Y60630359612867        UPS    02/21 +5d  Order from Amazon
    - 02/15 07:46 PM  Spokane, WA, United States     Departure Scan
    - 02/15 11:47 AM  Spokane, WA, United States     Arrival Scan
    - 02/15 11:17 AM  Coeur d'Alene, ID, United States Departure Scan
    - 02/15 10:56 AM  Coeur d'Alene, ID, United States Origin Scan
    - 02/15 09:57 AM  United States                  Order Processed: Ready for UPS

$ wheres-my-ship detail 568403913053849
  2 568403913053849           FEDEX  02/18 +2d  Gift for mom
    - 02/16 07:17 AM  HUTCHINS, TX                   Departed FedEx location
    - 02/16 04:48 AM  HUTCHINS, TX                   Arrived at FedEx location
    - 02/15 09:30 PM  IRVING, TX                     Left FedEx origin facility
    - 02/15 05:15 PM  IRVING, TX                     Arrived at FedEx location
    - 02/15 09:45 AM                                 Shipment information sent to FedEx
    - 12/31 04:00 PM  IRVING, TX                     Picked up
```


### Remove package

Syntax:

`wheres-my-ship remove <tracking_number> | <item_number>`

Example:

```
$ wheres-my-ship list
  1 1Z3Y60630359612867        UPS    02/21 +5d  Order from Amazon
  2 568403913053849           FEDEX  02/18 +2d  Gift for mom
  3 9200191035346014134451    USPS   02/18 +2d  Documents for Bob

$ wheres-my-ship remove 3

$ wheres-my-ship list
  1 1Z3Y60630359612867        UPS    02/21 +5d  Order from Amazon
  2 568403913053849           FEDEX  02/18 +2d  Gift for mom
```

### Cleanup

Removes delivered packages

`$ wheres-my-ship cleanup`
