# Lab 1 Notes – Carlos Eduardo

Time zone: America/New_York
ID line used in every screenshot:

```bash
whoami && TZ='America/New_York' date && ec2-metadata -i && uptime -s
```

> Each run: at least 3 sentences (what changed, errors hit and fixes, what I newly understood)
> plus at least one exact output line only that run would have.

---

## How I ran these labs

Every run was a brand-new Amazon Linux 2023 instance in us-east-1, launched
one at a time, with Steps 0–4 of the runbook typed by hand: `whoami`/`pwd`,
install git, clone the course repo, run the installer, explore the COBOL core
in `/opt/balerica-bank`, then activate the venv, run the ID line and start
uvicorn. The steps and outputs were the same every time; what changed between
runs was the instance itself (new Instance ID, new private IP, new uvicorn PID).

Runs 1–3 were launched one at a time by hand. Runs 4–6 were launched from my
launch template (confirmed OK by Theo). The template is not baked: it does not
clone the repo or run the installer, so Steps 0–4 were still done by hand on
each one. Each template launch was a single instance, so every run has its own
boot time.

Early on I tried launching 3 instances at once from the template. When I ran
the ID line, all 3 had the same boot time (UTC) because they came from the same
launch request. I didn't use those for my runs; I launched one instance per run
instead.

---

## Run 1 – i-0c7c062d3ad3d66d5

**What changed:** New instance (private IP `172.31.31.212`), launched on its own
and taken from launch to `Application startup complete.` with Steps 0–4 by hand.
This was the first run where I also opened the API console in my browser
(`http://<public-ip>:8000/docs`).

**Errors hit and fixes:** No errors in the lab steps. The uvicorn log showed
`GET /favicon.ico 404 Not Found` when I opened the browser. Nothing needed
fixing: the browser asks every website for a tab icon automatically, and this
API simply doesn't have one.

**What I newly understood:** The uvicorn window is a live access log. Every
request from my browser shows up with my IP address, the path, and the HTTP
status code (200 = OK, 404 = not found). A 404 isn't always a problem; you have
to read *what* was not found.

**Output line unique to this run:**
```text
INFO:     Started server process [35874]
```

---

## Run 2 – i-00066206f7feebf52

**What changed:** New instance (private IP `172.31.24.163`). Same steps and same
output as Run 1. I didn't open the browser this time, so there were no request
lines after startup.

**Errors hit and fixes:** No errors. I pasted `./cobol/bin/account_lookup 999999`
and `echo $?` together. Both ran fine, because the shell runs them in order and
`echo $?` reports the program right before it, but the runbook says to paste
one command at a time.

**What I newly understood:** `echo $?` only shows the return code of the *last*
command that ran. That's why it has to come right after the COBOL program:
`0` for account 100001 (found) and `4` for 999999 (not found). The program didn't
crash; it reported what happened with a number, the mainframe way.

**Output line unique to this run:**
```text
INFO:     Started server process [35272]
```

---

## Run 3 – i-0a031128731725ade

**What changed:** New instance (private IP `172.31.19.156`). The difference this
run: `cat data/payments.dat` showed **two** `TXN0001` lines, because I ran
`payment_post TXN0001 100001 125.50 USD INV-1001` twice.

**Errors hit and fixes:** No error message. The program accepted the same
transaction ID twice and posted it twice (`STATUS=POSTED`, return code 0 both
times). Nothing to fix for the lab, but it's worth knowing that the program allows it.

**What I newly understood:** `payment_post` opens the journal with `OPEN EXTEND`,
which means *append only*. Every call adds a new fixed-width line to the end of
`payments.dat` and never overwrites or checks earlier lines. The COBOL core
doesn't check for duplicate TXN IDs, so the same payment can be journaled twice.
In real banking, that's why every payment needs a unique ID and why systems check
for duplicates (idempotency) before posting.

**Output line unique to this run:**
```text
TXN0001             100001000000012550USDPOSTED  INV-1001
TXN0001             100001000000012550USDPOSTED  INV-1001
```

---

## Run 4 – i-0852c3271ac4dfc45

**What changed:** New instance (private IP `172.31.22.215`), first run on day 2.
The `date` line now said **EST** instead of **EDT**, because daylight saving time
ended overnight (Nov 1).

**Errors hit and fixes:** No errors. Same steps, same output.

**What I newly understood:** `TZ='America/New_York' date` shows my local time,
but `uptime -s` shows the boot time in the server's own clock, which is **UTC**
on Amazon Linux. On Saturday local time was UTC−4 (EDT); from Sunday it is
UTC−5 (EST). So boot 16:00:50 UTC = 11:00 EST, about 22 minutes before my
screenshot, which is inside the "not after the screenshot, not more than 24h
before it" rule.

**Output line unique to this run:**
```text
INFO:     Started server process [36044]
```

---

## Run 5 – i-0879f1f31633df516

**What changed:** New instance (private IP `172.31.24.89`). I opened `/docs` in
the browser again, so the uvicorn log showed four requests:
`GET /`, `GET /favicon.ico` (404), `GET /docs` and `GET /openapi.json`.

**Errors hit and fixes:** Same harmless `404 Not Found` for `/favicon.ico` as in
Run 1. No other errors.

**What I newly understood:** Loading the `/docs` page makes the browser also
request `/openapi.json`. That file is the API description FastAPI generates
automatically from the code, and the docs page is built from it. The COBOL
programs are never involved in that 404; it comes from the Python web layer.

**Output line unique to this run:**
```text
INFO:     Started server process [35624]
```

---

## Run 6 – i-009fb79b23ec80955

**What changed:** New instance (private IP `172.31.22.114`), launched about
14 minutes after Run 5. Same steps and output; Alice still showed
`BALANCE=5000.00` on a fresh install.

**Errors hit and fixes:** No errors.

**What I newly understood:** The bank is the same on every brand-new server
because the installer rebuilds it the same way every time: it seeds the same
3 accounts and compiles the same COBOL programs. That's why the runbook says
you "cannot break anything" a re-run won't fix. I also read the journal line
by hand: `000000012550` is $125.50, because the decimal point is implied (PIC
9(10)V99) and never stored in the file.

**Output line unique to this run:**
```text
INFO:     Started server process [35626]
```

---

## Run 7 – i-xxxxxxxxxxxxxxxxx

**What changed:**

**Errors hit and fixes:**

**What I newly understood:**

**Output line unique to this run:**
```text

```

---

<a id="cobol-practice"></a>
## COBOL practice

**Run used:** Run 6
**Instance ID:** i-009fb79b23ec80955 (boot 2026-11-01 17:14:40 UTC)
**Folder:** /opt/balerica-bank
**Date:** Sun Nov 1, 2026, 13:03–13:24 EST (second terminal, uvicorn still running in the first)

My TXN IDs start with my initials: `TXN-CE-0001`, `TXN-CE-0002`.

### Return code 0 – success (`cobol_rc0.jpg`, 13:03:50 EST)
```bash
cd /opt/balerica-bank
./cobol/bin/payment_post TXN-CE-0001 100001 125.50 USD INV-CE-0001
echo $?
```
```text
TXN=TXN-CE-0001
ACCOUNT=100001
AMOUNT=125.50
CURRENCY=USD
STATUS=POSTED
REASON=APPROVED
0
```
Account 100001 is ACTIVE, the amount is above zero and below the balance,
so every check in `VALIDATE-PAYMENT` passed and the payment posted.

### Return code 4 – account not found (`cobol_rc4.jpg`, 13:07:17 EST)
```bash
./cobol/bin/account_lookup 999999
echo $?
```
```text
ERROR=ACCOUNT NOT FOUND
ACCOUNT=999999
4
```
4 is a *warning*, not a crash: the program read the whole ledger, didn't find
999999, and reported it.

### Return code 8 – business rejection, zero amount (`cobol_rc8.jpg`, 13:09:26 EST)
```bash
./cobol/bin/payment_post TXN-CE-0002 100001 0.00 USD ZERO-TEST
echo $?
```
```text
TXN=TXN-CE-0002
ACCOUNT=100001
AMOUNT=0.00
CURRENCY=USD
STATUS=REJECTED
REASON=AMOUNT MUST BE GREATER THAN ZERO
8
```
The account is real and active, but a bank rule says a payment must be more
than zero. That's a *business* rejection (8), different from "account doesn't
exist" (4) and from "the program couldn't run properly" (12).

### Return code 12 – environment failure, wrong folder (`cobol_rc12.jpg`, 13:11:47 EST)
```bash
cd ~
pwd
/opt/balerica-bank/cobol/bin/account_lookup 100001
echo $?
```
```text
/home/ec2-user
ERROR=CANNOT OPEN data/accounts.dat
FILE-STATUS=35
12
```
Same valid account (100001) that returns 0 from the right folder. The program
opens `data/accounts.dat` *relative to the current folder*, and
`/home/ec2-user/data/accounts.dat` doesn't exist. COBOL file status 35 means
"file not found" when opening, so the program stopped with 12. The fix is
`cd /opt/balerica-bank` first, which matches the runbook's Troubleshooting table.

### Ledger decoded by hand with `cut -c` (`cobol_ledger_decode.jpg`, 13:16:35 EST)
```bash
cat data/accounts.dat
cut -c1-6   data/accounts.dat
cut -c7-36  data/accounts.dat
cut -c37-44 data/accounts.dat
cut -c45-56 data/accounts.dat
cut -c57-59 data/accounts.dat
```
```text
100001Alice Johnson                 ACTIVE  000000500000USD
100002Robert Smith                  ACTIVE  000000275000USD
100003Maria Garcia                  ACTIVE  000000810000USD
```

| Account (1-6) | Name (7-36) | Status (37-44) | Balance raw (45-56) | Balance | Currency (57-59) |
|---|---|---|---|---|---|
| 100001 | Alice Johnson | ACTIVE | 000000500000 | 5,000.00 | USD |
| 100002 | Robert Smith | ACTIVE | 000000275000 | 2,750.00 | USD |
| 100003 | Maria Garcia | ACTIVE | 000000810000 | 8,100.00 | USD |

### Alice's balance decoded by hand
- Alice's record: `100001Alice Johnson                 ACTIVE  000000500000USD`
- Columns 45-56: `000000500000`
- The field is `PIC 9(10)V99`: 10 whole-number digits, then 2 decimal digits,
  with the `V` meaning the decimal point is *implied* (never stored).
- `0000005000` | `00` → **$5,000.00**

### Journal lines for my TXN IDs (13:19:55 EST)
```bash
grep 'TXN-CE' data/payments.dat
./cobol/bin/account_lookup 100001
```
```text
TXN-CE-0001         100001000000012550USDPOSTED  INV-CE-0001
TXN-CE-0002         100001000000000000USDREJECTEDZERO-TEST
ACCOUNT=100001
NAME=Alice Johnson
STATUS=ACTIVE
BALANCE=5000.00
CURRENCY=USD
```

Decoding my journal lines (TXN 20 chars | account 6 | amount 12 | currency 3 | status 8 | reference 16):
- `TXN-CE-0001` → account 100001, `000000012550` = $125.50, USD, **POSTED**, INV-CE-0001
- `TXN-CE-0002` → account 100001, `000000000000` = $0.00, USD, **REJECTED**, ZERO-TEST

**Did the balance change?** No. After `TXN-CE-0001` posted, `account_lookup 100001`
still shows `BALANCE=5000.00`. `payment_post` only opens `accounts.dat` for
*input* (read-only) and appends the result to `payments.dat` with `OPEN EXTEND`.
It records the decision in the journal but never rewrites the ledger. Also, the
rejected payment was still written to the journal (`REJECTED`), so the journal
is an audit trail of every decision, not just the successful ones.

### Git clone on EC2 (`git_clone.jpg`, 13:24:01 EST)
Run 6, `i-009fb79b23ec80955`. Clone only: no push from EC2, no GitHub token on the server.
```bash
cd ~
git clone https://github.com/charlesedward-ops/seir-foundations-labs.git
ls seir-foundations-labs/module1
```
```text
Cloning into 'seir-foundations-labs'...
Receiving objects: 100% (82/82), 1.62 MiB | 47.38 MiB/s, done.
Resolving deltas: 100% (20/20), done.
lab0.5_basics_contd  lab0_basics
```

---

## Mistakes I made
- Launched 3 instances at once from my launch template. They all had the same
  boot time, so I didn't use them and launched one instance per run instead.
- Ran `payment_post TXN0001 ...` twice in Run 3, which journaled the same
  transaction twice (the COBOL core does not block duplicate TXN IDs).
- In every run, pasted two commands at once (`account_lookup 999999` and
  `echo $?`) instead of one at a time like the runbook says.

- Pasted `clear` and the ID line together during COBOL practice. The terminal
  showed both lines, then `clear` ran and wiped the screen, including the
  ID-line command itself. So `cobol_rc12.jpg`, `cobol_ledger_decode.jpg` and
  `git_clone.jpg` show the ID-line *output* (user, date, instance ID, boot time)
  but not the command. **Lesson learned:** run `clear` by itself, wait for the
  empty screen, then paste the ID line. Putting them on one line
  (`clear && whoami ...`) has the same problem.

## Commands I now know by heart
- `whoami && TZ='America/New_York' date && ec2-metadata -i && uptime -s`
- `sudo dnf install -y git`
- `git clone https://github.com/BalericaAI/SEIR-Foundations.git`
- `bash SEIR-Foundations/module1/lesson1_ec2/API/install/install.sh`
- `cd /opt/balerica-bank` and `ls`
- `./cobol/bin/account_lookup 100001` then `echo $?`
- `./cobol/bin/payment_post TXN0001 100001 125.50 USD INV-1001`
- `cat data/payments.dat`
- `source venv/bin/activate`
- `uvicorn app.main:app --host 0.0.0.0 --port 8000`
- `Ctrl+C` to stop the server

---

<a id="bam"></a>
## BAM (extra credit)

### BAM_2.1 – Bedrock `/ask`
- Instance ID (from screenshot):
- IAM role name:
- Inference profile ID (starts with `us.`):
- AWS_REGION:
- Question asked (one only my ledger can answer, e.g. about TXN-CE-0001):
- Full response:
```json

```

### BAM_2.2 – Baked launch template
- Instance ID (from screenshot):
- Launch template ID (starts with `lt-`):
- Version number:
- Final User Data script:
```bash

```

### BAM_2.3 – Golden AMI
- AMI ID (starts with `ami-`):
- Snapshot ID (starts with `snap-`):
- Source instance ID:
- AMI-launched instance ID (from screenshot):
- AMI deregistered and snapshot deleted: ⬜
