
# STX-TaxVerse - StackTax Smart Contract

**StackTax** is a comprehensive Clarity smart contract designed to manage complex tax computations with support for progressive tax brackets, multi-currency conversions, deductions, refund processing, and detailed reporting. It is built for transparency, modularity, and extensibility.

---

## 🔐 Authorization

* **Admin Role**: Only the administrator (set at contract deployment) can perform sensitive actions like updating exchange rates, registering deduction types, approving deductions, and issuing refunds.
* **Errors**: All unauthorized attempts return `ERR-NOT-AUTHORIZED`.

---

## 🧮 Core Features

### 📊 Tax Brackets

* **Progressive Tax System**: Tax rates are defined in brackets with thresholds and corresponding percentages.
* Stored in the `income-tax-brackets` map.
* Each income category can have up to 10 progressive brackets.
* Example bracket entry includes threshold, percentage, and a human-readable description.

### 💱 Multi-Currency Support

* Exchange rates between currencies are stored in `currency-exchange-rates`.
* Conversion scales by `1e8` to preserve precision.
* `convert-between-currencies` performs currency conversions securely.

### 💸 Deductions

* Defined via `available-deductions` with caps and optional approval.
* Taxpayers submit deduction requests through `submit-deduction-request`.
* Admins approve deductions via `approve-deduction-request`.
* Only approved deductions count toward tax reduction.

### 🧾 Refunds

* Admins can refund taxes using `issue-tax-refund`.
* Refunds are converted to STX (base currency) before being transferred.
* Transaction is logged with amount, timestamp, and currency.

---

## 📁 Data Structures

### Taxpayer Profile (`taxpayer-profiles`)

Tracks individual taxpayer data:

* `cumulative-tax-paid`
* `cumulative-tax-refunded`
* `most-recent-payment`
* `taxpayer-category`
* `claimed-deductions` (with approval status)
* `transaction-history` (including timestamps and currency)

### Deduction Record

```clarity
{
  deduction-code: string,
  deduction-amount: uint,
  deduction-approved: bool
}
```

### Transaction History Entry

```clarity
{
  transaction-amount: uint,
  transaction-timestamp: uint,
  transaction-currency: string
}
```

---

## 🛠️ Public Functions

### Admin Functions

* `update-exchange-rate`: Update currency rates.
* `register-deduction-type`: Define new deduction types.
* `approve-deduction-request`: Approve specific taxpayer deductions.
* `issue-tax-refund`: Refund STX to users (converted from another currency if needed).

### User Functions

* `submit-deduction-request`: Submit a deduction for review/auto-approval.

---

## 🔍 Read-Only Functions

* `get-taxpayer-profile`: View full taxpayer record.
* `get-currency-rate`: Get latest currency exchange rate.
* `get-deduction-info`: Retrieve deduction definition details.
* `get-tax-bracket-info`: View current tax brackets.
* `calculate-progressive-tax`: Estimate tax owed based on income.
* `generate-annual-tax-report`: Full taxpayer report with payments, refunds, deductions.
* `calculate-net-tax-obligation`: Tax owed after deductions.

---

## 🔒 Error Codes

| Code | Description          |
| ---- | -------------------- |
| 100  | Not Authorized       |
| 101  | Invalid Amount       |
| 102  | Tax Rate Not Found   |
| 103  | Insufficient Balance |
| 104  | Invalid Tax Rate     |
| 105  | Invalid Currency     |
| 106  | Invalid Deduction    |
| 107  | Refund Not Allowed   |
| 108  | Invalid Period       |
| 109  | Transfer Failed      |

---
