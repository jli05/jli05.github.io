---
title: Code examples reading DataBento Live Data
layout: post
---

To dump all into a `dbn` file, then convert to `csv` or `DataFrame`.

```python
import databento as db

# First, create a live client and connect
live_client = db.Live(key="YOUR_API_KEY")

# Next, we will subscribe to the ohlcv-1s schema for a few symbols
live_client.subscribe(
    dataset="DBEQ.BASIC",
    schema="trades",
    stype_in="raw_symbol",
    symbols=["AAPL"],
)
# Now, we will open a file for writing and start streaming
with open("example.dbn", "wb") as output:
    live_client.add_stream(output)
    live_client.start()

    # We will wait for 5 seconds before closing the connection
    live_client.block_for_close(timeout=5)

# Finally, we will open the DBN file
dbn_store = db.from_dbn("example.dbn")
print(dbn_store.to_df(schema="trades"))
``` 

Or use callback function to handle data.

An incoming message can be SymbolMappingMsg, OHLCVMsg, SystemMsg, ErrorMsg, etc.
[Example of handling them](https://databento.com/docs/examples/basics-live/live-dispatch/overview).

`Live.add_callback()` [documentation](https://databento.com/docs/api-reference-live/client/add-callback#parameters?historical=python&live=python&reference=python)

Another example code:

```python
def handler(msg):
    if isinstance(msg, SymbolMappingMsg):
          # handle the symbol mapping msg
    elif isinstance(msg, OHLCVMsg):
         print(msg.close)
    elif isinstance(msg, ErrorMsg):
          # handle the error msg
    elif isinstance(msg, SystemMsg):
          # handle system msg
    else:
           raise RuntimeError

if __name__ == '__main__':
    client = Live(key="<key>")

    client.subscribe(
        dataset="DBEQ.BASIC",
        schema="ohlcv-1s",
        stype_in="raw_symbol",
        symbols=["AAPL"]
    )

    client.add_callback(handler)

    client.start()

    client.block_for_close(timeout=15)
```
