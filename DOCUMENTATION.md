# MICKYETS Business Manager — How It Works

This is a plain-language guide to what MICKYETS does and how its numbers
are calculated, written for running the WiFi and Stationary business —
not a developer document (see README.md for that).

---

## 1. The core idea

MICKYETS tracks money in three layers:

1. **What happened** — every income entry, expense, sale, credit given,
   inventory change you record.
2. **What it means** — Revenue, Cost, Net Balance, and Profit, calculated
   the same way everywhere in the app (explained below).
3. **What happens automatically** — a daily rule that skims a percentage
   of profit into savings and forwards the rest toward whatever goal
   you're currently working on.

Everything else in the app (Dashboard, Reports, Settings) is a view onto
those three layers.

---

## 2. The modules

| Module | What it's for |
|---|---|
| **Dashboard** | The daily/weekly/monthly overview — see section 4 |
| **Income** | Manual income entries (Stationary, WiFi) |
| **Expenses** | Money spent, by category (Home, Breakfast, Lunch, Supper, Transport, Debt, Airtime, Other, Home Rent) |
| **Inventory** | Stock you hold — quantity, cost price, selling price, reorder level |
| **Daily Sales** | Itemized sales, optionally linked to an Inventory item, with capital and profit tracked per sale |
| **Credit** | Money customers owe you (accounts receivable) — record it, mark it paid when they settle up |
| **Savings** | Three buckets — Business, Home, Emergency — each with a target, balance, and history |
| **Goals** | Specific things you're saving toward (a laptop, rent, stock restock) — separate from the three savings buckets |
| **Reports** | Monthly summaries, best/worst months, all-time totals |
| **Settings** | Savings automation rules, backups, Google Drive, account/login |

---

## 3. The standard formula — used everywhere, no exceptions

Every number in the app that says "Income", "Profit", "Net Balance", etc.
is built from the same four definitions:

```
Revenue      = Income entries + Sales amounts
Cost         = Expenses + Sales capital (cost of goods sold)
Net Balance  = Revenue − Expenses                    (cash flow)
Profit       = Net Balance − Sales capital            (true profitability)
```

**Why two different "how much did I make" numbers exist:**

- **Net Balance** answers "how much more cash do I have?" It only looks
  at money in vs money out — it doesn't care what your stock cost you.
- **Profit** answers "did I actually make money?" It backs out what the
  goods you sold cost you to acquire (the **capital**), so a sale that
  brought in 200,000 but cost you 180,000 in stock shows as only 20,000
  of real profit — Net Balance alone wouldn't show that.

**Capital**, on a sale, is either:
- auto-filled from the linked Inventory item's cost price × quantity sold, or
- typed in by hand if the sale isn't linked to an Inventory item.

---

## 4. The Dashboard

### Day / Week / Month view

At the top of the Dashboard, the **Day / Week / Month** tabs switch what
period the four main stat cards (Income, Expenditure, Net Balance,
Profit) are calculated over. The **← →** arrows move backward and
forward through history one period at a time — yesterday, the day
before, last week, last month, and so on. **Today** jumps straight back
to the current period. You can't navigate into the future.

### The always-visible row

Below the period cards, four figures that aren't tied to whichever
period you're viewing — they're always current:

- **Cash at Hand** — see section 6
- **Total Capital (stock value)** — your current Inventory, valued at
  cost price (quantity × unit cost, summed across every item). This is
  "how much capital is tied up in stock right now."
- **Savings Growth Rate** — this month's savings deposits vs last
  month's, as a percentage change
- **Current Goal** — whichever goal is currently receiving automatic
  deposits, and its progress — see section 6

### Today's Money Flow

A quick, always-today snapshot: whether money came in today at all (a
green/red indicator), today's sales total, and how much customers
currently owe you on credit.

---

## 5. Daily savings automation

Every time you add, edit, or delete an income entry, expense, or sale,
the app recalculates that day's automation from scratch:

```
Day's profit = that day's Revenue − that day's Expenses − that day's Capital

If profit > 0:
  Total to save = profit × (savings rate, default 20%)
  Emergency  gets  (Emergency split %, default 20%) of that total
  Business   gets  (Business split %,  default 40%) of that total
  Home       gets  (Home split %,      default 40%) of that total
```

**Worked example:** a day with UGX 20,000 profit, at the default 20%
rate: 4,000 total gets saved — Emergency 800, Business 1,600, Home
1,600 (20/40/40 of the 4,000).

This is **idempotent** — recalculating the same date twice never
double-deposits. If you correct an entry and a day's profit changes, the
next recalculation tops up or claws back only the difference, and the
history shows an "Auto" tag so you can always tell an automatic entry
apart from a manual deposit.

You can change the rate and the three split percentages any time in
**Settings → Daily Savings Automation** (the three splits must add up to
100%). There's also a **Recalculate for All Past Dates** button there,
useful after changing the settings or restoring a backup.

---

## 6. Cash at Hand and Goals

Whatever profit is left over *after* the daily savings cut goes into
**Cash at Hand** — a holding area for money that hasn't been assigned
anywhere yet.

**If you have an active goal**, Cash at Hand doesn't sit still: it's
immediately forwarded into that goal, in full, every time it's
recalculated. "Active goal" means the oldest goal you created that
hasn't hit its target yet — goals are worked through in the order you
made them.

**When a goal reaches 100%** of its target, it's marked achieved, and
from that point on, new money automatically shifts to the *next* goal in
line — no action needed from you. If a deposit happens to overshoot a
goal's target (e.g. it needed 5,000 more but 8,000 arrived), the extra
simply stays credited to that goal; it doesn't roll over to the next one.

**If you have no goals at all, or every goal is already achieved**, Cash
at Hand just accumulates and stays visible on the Dashboard — nothing
forces you to create a goal, it's just where uncommitted profit shows up
if there's nowhere automatic for it to go.

You can also deposit into or withdraw from a goal manually any time from
the Goals page, independent of the automation.

---

## 7. Inventory and Daily Sales working together

When you record a sale and link it to an Inventory item:

- that item's stock quantity goes down by the quantity sold
- the sale's **capital** auto-fills from that item's cost price × quantity
  (you can still overwrite it by hand if the actual cost differed)
- **profit** on that sale is always `amount received − capital`

If a sale isn't linked to any Inventory item (a walk-in service, a
one-off), capital defaults to 0 unless you type one in yourself — so
profit equals the full amount received unless you tell it otherwise.

---

## 8. Credit (accounts receivable)

When a customer takes stock or WiFi time without paying immediately,
record it under **Credit** — it does **not** count as income yet. Once
they pay and you mark it paid, *that's* when it becomes an income entry
(dated the day it was actually paid), and everything downstream — Net
Balance, Profit, savings automation — reacts to it at that point, not
when the credit was first given.

The Dashboard's "Outstanding credit" figure is the total of everything
marked unpaid, so you always know how much is owed to you right now.

---

## 9. Backups

Two independent backup methods, both manual/opt-in — nothing syncs on
its own:

- **Phone storage / USB** — writes a timestamped copy into a
  `MICKYETS-Backups` folder on shared storage (the same folder a PC sees
  when the phone is plugged in over USB)
- **Google Drive** — after a one-time connection (Settings → Google
  Drive Backup), "Backup Now" uploads the current data as a single file
  only MICKYETS can see or touch in your Drive

Restoring from either fully replaces current data, and always asks for
confirmation first.

---

## 10. Login and accounts

The first time the app runs (on a phone or a freshly installed desktop
copy), it asks you to set a username and password instead of showing the
dashboard. After that, it's a normal login screen. Sessions last 30 days
before you're asked to sign in again. Passwords are hashed (never stored
in plain text) using `scrypt`, a slow, industry-standard hash built into
Node itself.

A phone install and a desktop install each keep their own separate
login and data — they don't sync with each other automatically. Use a
backup (section 9) to move data from one to the other.
