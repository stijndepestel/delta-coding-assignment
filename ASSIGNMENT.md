# Engineering Test

So you want to join the engineering team at Delta? That's awesome!

We have created a small exercise in order to gain some insights into your architectural skills.

## Assignment

### Part 1

We currently integrate with multiple data providers, all providing price data.

These integrations range from real-time data feeds to periodically polling APIs for all available data.

Roughly speaking, you can assume that these integrations provide a non-stop flow of the following data events:

```
struct PriceEvent {
    baseCurrency: string;
    quoteCurrency: string;
    exchange: string;
    price: float64;
    timestampInMs: uint64;
}
```

An example of an event:

```json
{
    "baseCurrency": "BTC",
    "quoteCurrency": "EUR",
    "exchange": "Coinbase",
    "price": 58880.8,
    "timestampInMs": 1709508720000, 
}
```

Your assignment consists on designing an optimal way to ingest this data, process it and store it somewhere given the following requirements:

 1. The most recent price must be retrievable, given a certain `(baseCurrency, quoteCurrency, exchange)` tuple (or an array of them).
 2. Historical data must be retrievable, but the granularity can be more coarse the older the data is (generally only [OHLC](https://en.wikipedia.org/wiki/Open-high-low-close_chart) is kept, make a proposal). This will be queried again in a key-value (same primary key as above) fashion, but time-based range queries are possible as well.
 3. A list of all possible exchanges, their pairs (`(baseCurrency, quoteCurrency)`) and the last time a price point has been observed should be retrievable.

You are expected to come up with a design plan on how to tackle this data ingestion and querying assignment.

Scale-wise, you can expect a non-trival amount of incoming price events every couple of seconds. If possible, make separate proposals on how to tackle extra incoming load (read vs write).

You can assume a 70/30 ratio of reads/writes, where the recency of the data correlates with how hot it is.

### Part 2

Your 2nd task is to develop code that processes a series of `PriceEvent`s and stores the resulting data in the data structures/stores you selected earlier. Feel free to use placeholder code for communications with the chosen data storage systems.

NOTE: don't mock **how** you mutate/query the store, only mock the actual underlying communication with the remote data store (eg `fetchFooById()` must not be mocked, `executeQuery()` may).

Additionally, create a basic API. This doesn't have to be a web-based API; a simple JS interface or class will do. This API should offer methods to fulfill the requirements of part 1: fetching the latest price, retrieving historical data for a given tuple and getting a list of the most recent prices. You can mock the data store communications in this part as well.

### Technologies & tools

You are free to use all the tools that you want or feel you need to!

However, since we use JavaScript (TypeScript is preferred) and Node.JS (latest LTS) at Delta, it is strongly recommended to write your application code using this environment.

### Tips

- Start small and build up gradually. Try to document all your trains of thought, even if they seem insignificant or stupid.
- If possible, provide multiple alternatives and explain why the one is preferred over the other.
- Look into providing metrics/proof showing that your suggested solution works well.
- Keep resource and budget constraints in mind, the lower the better.
- KISS, when possible. Simplicity trumps complexity.
- Be prepared to get questions later about choices you've made.

## Timeframe

Spend as much time on this as you want, but don't go overboard. Again, keep it as simple as possible while still fulfilling the requirements.

## Delivery

Send us a link to an accessible git repository so we can checkout your design and code.

## Questions?

Feel free to contact us if something would not be clear.