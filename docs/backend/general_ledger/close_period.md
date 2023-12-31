# Close Period

``` mermaid
sequenceDiagram
    actor caller as Caller
    box General Ledger module
    participant period as Period
    participant ledger as Ledger
    end
    box Report module
    participant report as Report
    end

    caller ->>+ period: Trigger close period

    period ->> period: Update period to closed and get next fiscalYear + number

    opt
        period ->> period: Create next period if not exists yet
    end

    period ->> period: Update next period to current period

    period ->> ledger: Create all ledgers for next period

    period ->> report: (Re)calculate report values for old period

    period ->>- caller: Response
```

***The report part is not implemented yet.***
