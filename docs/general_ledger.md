# General Ledger 总账 (WIP)

## Entities and Relations

For diagram simplicity, `SoB` and `Period` are not connected with others:

- All entities are logically belongs to a `SoB`
- `Voucher` and `Ledger` are logically belongs to a `Period`

``` mermaid
---
title: General Ledger ER
---
erDiagram
    VOUCHER ||--|{ LINE_ITEM: "contains (at least 2)"
    LINE_ITEM }o--|| ACCOUNT: "placed in"
    LINE_ITEM }o..o{ AUXILIARY_ACCOUNT: "placed in"
    
    ACCOUNT ||..o| ACCOUNT: "belongs to"
    AUXILIARY_CATEGORY ||..o{ AUXILIARY_ACCOUNT: contains
    ACCOUNT ||..o{ AUXILIARY_CATEGORY: has
    
    ACCOUNT ||--|{ LEDGER: "records in"
    AUXILIARY_ACCOUNT ||--|{ AUXILIARY_LEDGER: "records in"
    
    SOB {
        UUID id
        string name
        string description
        string baseCurrency
        int startingPeriodYear
        int startingPeriodMonth
        int[] accountsCodeLength
    }
    
    ACCOUNT {
        UUID id
        UUID sobId
        string title
        string accountNumber
        int[] numberHierarchy
        int level
        Account superiorAccount
        int class
        int group
        BalanceDirection balanceDirection
        AuxiliaryCategory[] auxiliaryCategories
    }
    
    AUXILIARY_CATEGORY {
        UUID id
        UUID sobId
        string key
        string title
        bool isStandard
    }
    
    AUXILIARY_ACCOUNT {
        UUID id
        AuxiliaryCategory category
        string key
        string title
        string description
    }
    
    PERIOD {
        UUID id
        UUID sobId
        int fiscalYear
        int periodNumber
        Time openingTime
        Time endingTime
        bool isClosed
        bool isCurrent
    }
    
    VOUCHER {
        UUID id
        UUID sobId
        UUID periodId
        VoucherType voucherType
        string headerText
        string documentNumber
        int attachmentQuantity
        Decimal debit
        Decimal credit
        UUID creator
        UUID reviewer
        UUID auditor
        UUID poster
        bool isReviewed
        bool isAudited
        bool isPosted
        Time transactionTime
        LineItem[] lineItems
    }
    
    LINE_ITEM {
        UUID id
        Account account
        AuxiliaryAccounts[] auxiliaryAccounts
        string text
        Decimal debit
        Decimal credit
    }
    
    LEDGER {
        UUID id
        UUID periodId
        Account account
        Decimal openingDebitBalance
        Decimal openingCreditBalance
        Decimal periodDebit
        Decimal periodCredit
        Decimal endingDebitBalance
        Decimal endingCreditBalance
    }
    
    AUXILIARY_LEDGER {
        UUID id
        UUID periodId
        AuxiliaryAccount auxiliaryAccount
        Decimal openingDebitBalance
        Decimal openingCreditBalance
        Decimal periodDebit
        Decimal periodCredit
        Decimal endingDebitBalance
        Decimal endingCreditBalance
    }
```
