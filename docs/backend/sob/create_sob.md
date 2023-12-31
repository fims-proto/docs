# Create SoB

``` mermaid
sequenceDiagram
    actor caller as Caller
    participant sob as SoB
    participant gl as General Ledger
    participant num as Numbering
    participant report as Report

    caller ->>+ sob: Trigger create SoB
    sob ->> sob: Create SoB

    sob ->>+ gl: Initialize general ledger
    gl ->> gl: Create all accounts
    gl ->> gl: Create the 1st period
    gl ->> num: Create identifier configuration for voucher in the 1st period
    gl ->> gl: Create all ledgers in the 1st period
    gl -->>- sob: Finish

    sob ->>+ report: Initialize reports
    report ->> report: Create report templates
    report -->>- sob: Finish
    sob ->>- caller: Response
```
