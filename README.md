# GNSS Controller

This controller library is use to extract the GNSS data from the GNSS receiver.
that data is access by the `data logger` through the neom9n.Datafeed.

## Mga Ano data loader
Mga Ano data loader is a data loader that is use to load the Mga Ano data to the GNSS receiver.
This data is use to speed up the GNSS receiver to get the fix position.

In order to use this data loader, you need to download the Mga Ano data from the u-blox website
and update dashcam with it on regular basis.

The controller will wait to get accurate time from the GNSS receiver before loading the Mga Ano data.
If accurate time is not available with in 5 sec the entire Mga Ano data will be loaded to the receiver.
if accurate time is available with in 5 sec only 23 days worth of Mga Ano data will be loaded to the receiver.


## Architecture
### messageRegistry
Hold the map of message.Handlers / ubx message id. That will be used by the message.Decoder to decode the UBX message.

### message.Decoder
Is responsible for decoding the UBX message from the GNSS receiver.
Once the message is decoded the message. Decoder will look up the message Handlers from the messageRegistry and pass the current message to each of them each of them.

### Time setter message handler
Time setter is a UBX message handler that is register to receive `NavPvt` message from the GNSS receiver.
When the `NavPvt` message is received the time setter will check the precision of time.
If the precision is good enough the dashcam system time will be set to the GNSS receiver time.

### Datafeed handler
Handle multiple ubx.Messages from the GNSS receiver. Each messages will processed and data will be Collected in the Data structure.
each time an ubx.NavDop message the handleDataFunc will be called with the current data. This is how the `data logger` will get the data from the GNSS receiver.
