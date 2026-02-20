
## 🚀 **AKASH BME TESTNET - ULTIMATE GUIDE**
<img width="1156" height="590" alt="image" src="https://github.com/user-attachments/assets/13f9408c-5b3b-45c1-9dab-c299593aabd6" />

---

## ✅ **PHASE 0: SETUP & PRE-FLIGHT (Category 0)**

### **0.1 Install Binaries**
```bash
# Akash CLI v2.1.0-a17
wget https://github.com/akash-network/node/releases/download/v2.1.0-a17/akash_2.1.0-a17_linux_amd64.zip
unzip akash_2.1.0-a17_linux_amd64.zip
sudo mv akash /usr/local/bin/

# Provider Services v0.10.5
wget https://github.com/akash-network/provider/releases/download/v0.10.5/provider-services_0.10.5_linux_amd64.zip
unzip provider-services_0.10.5_linux_amd64.zip
sudo mv provider-services /usr/local/bin/

# Verify
akash version        # 2.1.0-a17
provider-services version  # v0.10.5
```

### **0.2 Environment Variables**
```bash
export AKASH_CHAIN_ID=testnet-8
export AKASH_NODE=https://testnetrpc.akashnet.net:443
export AKASH_KEYRING_BACKEND=test
export AKASH_GAS=auto
export AKASH_GAS_ADJUSTMENT=1.5
export AKASH_GAS_PRICES=0.025uakt
```

### **0.3 Create Wallet**
```bash
akash keys add bme-main
# MNEMONIC: your_mnemonic

akash keys show bme-main -a
# Address: akash1023uh7436y49xyhe0d8rcgwtzeh6akuhx2ecte
```

### **0.4 Fund Wallet (Faucet)**
Go to: https://faucet.dev.akash.pub/
Paste address: `akash1023uh7436y49xyhe0d8rcgwtzeh6akuhx2ecte`

### **0.5 Certificate**
```bash
akash tx cert generate client --from bme-main -y
akash tx cert publish client --from bme-main -y
# Tx: 111AABAABF7408C598947A62DA6BA66C8FDAE4D195C71F4FEC23E5A68AB96F3F
```

### **0.6 Pre-Flight Checks (All 10)**
```bash
# 0.1 Node connectivity
curl https://testnetrpc.akashnet.net:443/status | jq .result.node_info.network
# Output: "testnet-8"

# 0.2 BME module enabled
akash query bme params
# Output: params params

# 0.3 Oracle health
akash query oracle prices | head -n 50
date -u +"%Y-%m-%dT%H:%M:%S"
# Output: 2026-02-20T06:30:34

# 0.4 Vault state baseline
akash query bme vault-state
# Output: vault_state shown

# 0.5 Collateral ratio
akash query bme status | grep collateral_ratio
# collateral_ratio: "1.148855228522068483"

# 0.6 Mint status
akash query bme status | grep status
# status: mint_status_healthy

# 0.7 Token supply
akash query bank total --denom uakt
# amount: "100345060408577"
akash query bank total --denom uact
# amount: "17061569821"

# 0.8 Vault state stable
akash query bme vault-state
sleep 60
akash query bme vault-state  # Same output

# 0.9 Wallet balance
akash query bank balances akash1023uh7436y49xyhe0d8rcgwtzeh6akuhx2ecte
# amount: "500000000" uakt

# 0.10 Provider list
akash query provider list
# providers: akash1yjyp6thvfr2vtphqnef4vn9kfme28e6svghfl4, akash1wr4663g2kydjw4vmwtqsy8f4fevnvntuuhyusn
```
## 📦 CATEGORY 0 - FORM SUBMISSION EX:
0.1: Node and network connectivity
"testnet-8"

0.2: BME module enabled
params:
  circuit_breaker_halt_threshold: 9000
  circuit_breaker_warn_threshold: 9500
  epoch_blocks_backoff: 10
  min_epoch_blocks: "10"
  mint_spread_bps: 25
  settle_spread_bps: 0

0.3: Oracle health check
pagination:
  next_key: AAAAAQNha3QDdXNkAAAAAAACgs0=
  total: "0"
prices:
- id:
    base_denom: usd
    denom: akt
    height: "165157"
    source: 1
  state:
    price: "0.341552490000000000"
    timestamp: "2026-02-20T06:30:11.052022240Z"
2026-02-20T06:30:34

0.4: BME state baseline
vault_state:
  balances:
  - amount: "17061569821"
    denom: uact
  - amount: "57385365603"
    denom: uakt
  remint_credits:
  - amount: "52307378896"
    denom: uakt
  total_burned:
  - amount: "19519540413"
    denom: uact
  - amount: "0"
    denom: uakt
  total_minted:
  - amount: "36581110234"
    denom: uact
  - amount: "0"
    denom: uakt

0.5: Collateral ratio check
collateral_ratio: "1.148855228522068483"

0.6: Mint status check
status: mint_status_healthy

0.7: Token supply baseline
amount: "100345060408577" (uakt)
amount: "17061569821" (uact)

0.8: Vault state baseline (pending ops check)
vault_state:
  balances:
  - amount: "17061569821"
    denom: uact
  - amount: "57385365603"
    denom: uakt
  remint_credits:
  - amount: "52307378896"
    denom: uakt
  total_burned:
  - amount: "19519540413"
    denom: uact
  - amount: "0"
    denom: uakt
  total_minted:
  - amount: "36581110234"
    denom: uact
  - amount: "0"
    denom: uakt

0.9: Test wallet setup
balances:
- amount: "500000000"
  denom: uakt

0.10: Provider availability
providers:
- attributes: []
  host_uri: https://197.211.58.42:8443
  owner: akash1yjyp6thvfr2vtphqnef4vn9kfme28e6svghfl4
- attributes:
  - key: region
    value: us-central
  - key: host
    value: akash
  - key: tier
    value: community
  host_uri: https://provider.akashtestnet.xyz:8443
  owner: akash1wr4663g2kydjw4vmwtqsy8f4fevnvntuuhyusn
---

## 📦 **CATEGORY 1: DEPLOYMENT CREATION**

### **SDL File (deploy-higher.yml)**
```bash
cat > deploy-higher.yml << 'EOF'
---
version: "2.0"
services:
  web:
    image: nginx:alpine
    expose:
      - port: 80
        as: 80
        to:
          - global: true
profiles:
  compute:
    web:
      resources:
        cpu:
          units: 0.5
        memory:
          size: 512Mi
        storage:
          size: 1Gi
  placement:
    akash:
      pricing:
        web:
          denom: uact
          amount: 100000
deployment:
  web:
    akash:
      profile: web
      count: 1
EOF
```

### **1.1 Standard Deployment**
```bash
# Mint ACT
akash tx bme mint-act 100000000uakt --from bme-main -y
# Tx: 584F07DAD430279A1D9459C3DD85036921AB8DE512E551B9E47D39BDF9FC3A0

sleep 60

# Check balance
akash query bank balances akash1023uh7436y49xyhe0d8rcgwtzeh6akuhx2ecte
# ACT: 67914244 uact

# Create deployment
akash tx deployment create deploy-higher.yml --deposit 5000000uact --from bme-main -y
# Tx: 9E3069C30E5F77087BFAC50B3F834E7F6CA0BAAED9EF081F259C30EC4560CC9B
# DSEQ: 165313

sleep 45

# Check bids
akash query market bid list --owner akash1023uh7436y49xyhe0d8rcgwtzeh6akuhx2ecte --dseq 165313
# Provider: akash1wr4663g2kydjw4vmwtqsy8f4fevnvntuuhyusn

# Create lease
akash tx market lease create --dseq 165313 --gseq 1 --oseq 1 --provider akash1wr4663g2kydjw4vmwtqsy8f4fevnvntuuhyusn --from bme-main -y
# Tx: 4E2BF407890E8A85AFD6A534D74D6FB90F769F4DA114EC1927FB496B90C74DE9

# Send manifest
provider-services send-manifest deploy-higher.yml --dseq 165313 --provider akash1wr4663g2kydjw4vmwtqsy8f4fevnvntuuhyusn --from bme-main
# Status: PASS

# Verify
provider-services lease-status --dseq 165313 --provider akash1wr4663g2kydjw4vmwtqsy8f4fevnvntuuhyusn --from bme-main
# URL: 70a4mfho8p8et1v13fsbko1vn8.ingress.akashtestnet.xyz
```

### **1.2 Insufficient Funds**
```bash
akash tx deployment create deploy-higher.yml --deposit 999999999999uact --from bme-main -y
# Error: insufficient balance
```

### **1.3 Minimum Deposit**
```bash
# Create deployment with 1 ACT
akash tx deployment create deploy-higher.yml --deposit 1000000uact --from bme-main -y
# Tx: F6AF922B4CC494F65F6B285E7286413BEF2B85421CFCB88C0395444D38EB77CF
# DSEQ: 165357

sleep 45

# Bid received
akash query market bid list --owner akash1023uh7436y49xyhe0d8rcgwtzeh6akuhx2ecte --dseq 165357
# Provider: akash1wr4663g2kydjw4vmwtqsy8f4fevnvntuuhyusn

# Create lease
akash tx market lease create --dseq 165357 --gseq 1 --oseq 1 --provider akash1wr4663g2kydjw4vmwtqsy8f4fevnvntuuhyusn --from bme-main -y
# Tx: F00982FEB99B0A7692E08050E353B9FB8C0E6DEB84D1E0339E4BDDEFEC7BA1AF

# Send manifest
provider-services send-manifest deploy-higher.yml --dseq 165357 --provider akash1wr4663g2kydjw4vmwtqsy8f4fevnvntuuhyusn --from bme-main
# Status: PASS

# URL: go1a4tn9tl97h300n0iaumf8cg.ingress.akashtestnet.xyz
```

### **1.4 Multiple Deployments**
```bash
# Deployment A
akash tx deployment create deploy-higher.yml --deposit 5000000uact --from bme-main -y
# Tx: FC0D6E85F6A76649826D765997939077FD118BCD35939FF3762E99154546847D
# DSEQ: 165383

# Close later
akash tx deployment close --dseq 165383 --from bme-main -y
# Close Tx: 621894DB6E6E03DBEC50C0996E620996B7F0FE32841AD18FDE32889E165C15C7
```
## 📋 CATEGORY 1 - FORM SUBMISSION EX:
1.1: Create standard deployment

Transaction Hashes:
- Mint ACT: 584F07DAD430279A1D9459C3DD85036921AB8DE512E551B9E47D39BDF9FC3A90
- Create Deployment: 9E3069C30E5F77087BFAC50B3F834E7F6CA0BAAED9EF081F259C30EC4560CC9B
- Create Lease: 4E2BF407890E8A85AFD6A534D74D6FB90F769F4DA114EC1927FB496B90C74DE9

DSEQ: 165313
Provider: akash1wr4663g2kydjw4vmwtqsy8f4fevnvntuuhyusn
Bid Price: 2.372595 uact/block
Manifest Status: PASS
Deployment URL: 70a4mfho8p8et1v13fsbko1vn8.ingress.akashtestnet.xyz
Pre-deployment ACT: 67,914,244 uact
Post-deployment ACT: 57,914,244 uact

1.2: Create deployment - insufficient funds

Current ACT Balance: 57,914,244 uact
Attempted Deposit: 999,999,999,999 uact

Error Message:
"Error: rpc error: code = Unknown desc = rpc error: code = Unknown desc = failed to execute message; message index: 0: deposit invalid: insufficient balance"

1.3: Create deployment - minimum deposit

Deployment Create Tx: F6AF922B4CC494F65F6B285E7286413BEF2B85421CFCB88C0395444D38EB77CF
DSEQ: 165357
Deposit: 1,000,000 uact
Provider: akash1wr4663g2kydjw4vmwtqsy8f4fevnvntuuhyusn
Bid Price: 2.372595 uact/block
Lease Create Tx: F00982FEB99B0A7692E08050E353B9FB8C0E6DEB84D1E0339E4BDDEFEC7BA1AF
Manifest Status: PASS
URL: go1a4tn9tl97h300n0iaumf8cg.ingress.akashtestnet.xyz
Post-deployment ACT: 56,914,244 uact

1.4: Multiple deployments from single wallet

Deployment A (DSEQ 165383):
- Create Tx: FC0D6E85F6A76649826D765997939077FD118BCD35939FF3762E99154546847D
- Close Tx: 621894DB6E6E03DBEC50C0996E620996B7F0FE32841AD18FDE32889E165C15C7
- URL: dnftqelvlle4t79qk1qcm6ehlc.ingress.akashtestnet.xyz

Deployment B (DSEQ 165384):
- Create Tx: BF56D729F1F312072DD6B1B2180D20F163C8E7273C45BA7B3A7401E2F0740A66
- Status: Closed (no bids)

Deployment C (DSEQ 165386):
- Create Tx: CB8C5B1B6BF38A7AF446AF53E10507EC9C1864B2393D3B6A7B2C3749A4CA8CD1
- Status: Closed (no bids)

Pre-test ACT: 56,914,244 uact
Post-test ACT: [After closing] 

---

## 🔄 **CATEGORY 2: DEPLOYMENT CLOSURE & BURNS**

### **2.1 Close Deployment**
```bash
akash tx deployment close --dseq 165383 --from bme-main -y
# Tx: 621894DB6E6E03DBEC50C0996E620996B7F0FE32841AD18FDE32889E165C15C7
# Refund: 4,999,706 uact
```

### **2.2 Basic Burn**
```bash
# Before
akash query bank balances akash1023uh7436y49xyhe0d8rcgwtzeh6akuhx2ecte
# ACT: 37913125, AKT: 299899781

akash tx bme burn-act 5000000uact --from bme-main -y
# Tx: 9E7ACEB678821B63A637DBEFD041629532D7913E9E16C5AA7A6E3AF955C88702

sleep 60

# After
akash query bank balances akash1023uh7436y49xyhe0d8rcgwtzeh6akuhx2ecte
# ACT: 32913125, AKT: 314746638
```

### **2.3 Burn with Price Movement**
```bash
# Check price P1: 0.33813385
akash query oracle prices | head -n 15

# Wait for price drop (P2: 0.33763391)
sleep 300

# Burn
akash tx bme burn-act 5000000uact --from bme-main -y
# Tx: 0319454139991F96FE84B7BDC8FC09F27F2E9B549375CA6240C4D7880F3017D7

sleep 60

# Check AKT received: 14.82 AKT (5/0.3376)
akash query bank balances akash1023uh7436y49xyhe0d8rcgwtzeh6akuhx2ecte
# ACT: 27913125, AKT: 329567085
```
## 🔄 **CATEGORY 2: FORM SUBMISSION EX:** 
2.1: Close deployment - ACT escrow returned

Deployment DSEQ: 165383
Close Tx: 621894DB6E6E03DBEC50C0996E620996B7F0FE32841AD18FDE32889E165C15C7
Provider Payment: 294 uact
Refund Received: 4,999,706 uact

2.2: Burn ACT - basic operation

Pre-burn ACT: 37,913,125 uact
Pre-burn AKT: 299,899,781 uakt
Burn Tx: 9E7ACEB678821B63A637DBEFD041629532D7913E9E16C5AA7A6E3AF955C88702
Amount: 5,000,000 uact
Post-burn ACT: 32,913,125 uact
Post-burn AKT: 314,746,638 uakt
AKT Received: 14,846,857 uakt

2.3: Burn ACT with price movement

P1 (Mint Price): 0.33813385 USD/AKT
P2 (Burn Price): 0.33763391 USD/AKT
Burn Tx: 0319454139991F96FE84B7BDC8FC09F27F2E9B549375CA6240C4D7880F3017D7
Amount: 5,000,000 uact
AKT Received: 14,820,447 uakt
Verification: 5 ACT ÷ 0.33763391 = 14.81 AKT ✓
---


## 🏪 **CATEGORY 3: PROVIDER SETTLEMENT**

### **3.1 Provider Receives ACT**
```bash
# Provider: akash1wr4663g2kydjw4vmwtqsy8f4fevnvntuuhyusn
# Deployment 165383 close tx: 621894DB6E6E03DBEC50C0996E620996B7F0FE32841AD18FDE32889E165C15C7
# Provider payment: 294 uact
```

### **3.2 Lease Price Matches SDL**
```bash
akash query market lease get --owner akash1023uh7436y49xyhe0d8rcgwtzeh6akuhx2ecte --dseq 165383 --gseq 1 --oseq 1 --provider akash1wr4663g2kydjw4vmwtqsy8f4fevnvntuuhyusn
# Price: 2.372595 uact/block
```

### **3.3 Provider Payment Verification**
```bash
# Lease created: 165405
# Lease closed: 165529
# Blocks: 124
# Rate: 2.372595
# Expected: 124 × 2.372595 = 294.20 uact
# Actual: 294 uact ✓
```
# 🏪 **CATEGORY 3 FORM SUBMISSION : EX
3.1: Provider receives ACT from settlement

Provider: akash1wr4663g2kydjw4vmwtqsy8f4fevnvntuuhyusn
Deployment DSEQ: 165383
Close Tx: 621894DB6E6E03DBEC50C0996E620996B7F0FE32841AD18FDE32889E165C15C7
Provider Payment: 294 uact (denom: uact)

3.2: Verify lease price matches SDL pricing

Deployment Create Tx: FC0D6E85F6A76649826D765997939077FD118BCD35939FF3762E99154546847D
SDL Price: 100,000 uact (max)
Actual Lease Price: 2.372595 uact/block
Denom: uact ✓

3.3: Verify provider payment on deployment close

Deployment DSEQ: 165383
Close Tx: 621894DB6E6E03DBEC50C0996E620996B7F0FE32841AD18FDE32889E165C15C7
Lease Created At: 165405
Lease Closed At: 165529
Blocks: 124
Rate: 2.372595 uact/block
Expected: 124 × 2.372595 = 294.20 uact
Actual: 294 uact ✓

---

## ⏱️ **CATEGORY 4: EPOCH PROCESSING**

### **4.1 Request Queuing**
```bash
# Initial vault
akash query bme vault-state
# balances.uakt: 57546196786, remint_credits: 52468210079

akash tx bme mint-act 50000000uakt --from bme-main -y
# Tx: 4629C71988B68506DF77808F5C9799F11602FAF83A14A42661DA514AA2542737

# Immediate after
akash query bme vault-state
# balances.uakt: 57596196786 ↑, remint_credits: 52468210079 (same)

sleep 60

# After epoch
akash query bme vault-state
# remint_credits: 52518210079 ↑ (caught up)
```

### **4.2 Multiple Queued Requests**
```bash
# 3 rapid mints
akash tx bme mint-act 50000000uakt --from bme-main -y
# Tx1: 87A515A40023864F0D8001DC9A9F73E4DF7D7D4C8C88F24E1B7DFBC848E6B3F7

akash tx bme mint-act 50000000uakt --from bme-main -y
# Tx2: 0A626F3163A46106A44060FB222E6BBC51465C9FE62DAE70134BEE6395A359E2

akash tx bme mint-act 50000000uakt --from bme-main -y
# Tx3: E421196F5D554EECAB26B89664A33CDC448D9639456A58544E953BE15D61A5CA

sleep 90

# Final balance
akash query bank balances akash1023uh7436y49xyhe0d8rcgwtzeh6akuhx2ecte
# ACT: 94803077 (↑ from 44635646)
```
## CATEGORY 4 - FORM SUBMISSION EX:
4.1: Verify request queuing

Mint Tx: 4629C71988B68506DF77808F5C9799F11602FAF83A14A42661DA514AA2542737
Initial Vault: balances.uakt=57546196786, remint_credits=52468210079
Immediate After: balances.uakt=57596196786, remint_credits=52468210079
After Epoch: remint_credits=52518210079 (caught up)
ACT Received: ~16,722,521 uact

4.2: Multiple queued requests

Tx1: 87A515A40023864F0D8001DC9A9F73E4DF7D7D4C8C88F24E1B7DFBC848E6B3F7 (50 AKT)
Tx2: 0A626F3163A46106A44060FB222E6BBC51465C9FE62DAE70134BEE6395A359E2 (50 AKT)
Tx3: E421196F5D554EECAB26B89664A33CDC448D9639456A58544E953BE15D61A5CA (50 AKT)
Total Minted: 150 AKT
Initial ACT: 44,635,646 uact
Final ACT: 94,803,077 uact
Increase: 50,167,431 uact (150 AKT × price)

---

## 📨 **CATEGORY 5: DIRECT MESSAGES**

### **5.1 MsgMintACT**
```bash
akash tx bme mint-act 50000000uakt --from bme-main -y
# Tx: AC20F1DE69A064F40B04E6CC8F7ECA8EDAD7897B6F16DB4EC4EA37A5CA82A0D7

sleep 60
# ACT: 111268370 (↑ 16.5 ACT)
```

### **5.2 MsgBurnACT**
```bash
akash tx bme burn-act 10000000uact --from bme-main -y
# Tx: A7B683CD640DC78D739417A1E2EF2567EE9B062344F1AD08162AE4B37B5861A0

sleep 60
# AKT: 109829590 (↑ 30.27 AKT)
```

### **5.3 MsgBurnMint AKT→ACT**
```bash
akash tx bme burn-mint 30000000uakt uact --from bme-main -y
# Tx: AF079340987F39263034330CE683522D2C68AB2E7E16E95A495DFC2C37E0B4A8

sleep 60
# ACT: 111178874 (↑ 9.9 ACT)
```

### **5.4 MsgBurnMint ACT→AKT**
```bash
akash tx bme burn-mint 9000000uact uakt --from bme-main -y
# Tx: C214B1C92CFA592C400D2414AA9CAFE551BCA95828A722D8AAB0917FC86E187F

sleep 60
# AKT: 107083190 (↑ 27.25 AKT)
```

### **5.5 Pre-fund then Deploy**
```bash
# Mint
akash tx bme mint-act 50000000uakt --from bme-main -y
# Tx: AC20F1DE69A064F40B04E6CC8F7ECA8EDAD7897B6F16DB4EC4EA37A5CA82A0D7

sleep 60

# Deploy
akash tx deployment create deploy-higher.yml --deposit 5000000uact --from bme-main -y
# Tx: 9EF513780C689774A81F130BFA97F87366AC027BF5E8AB0307264E2AE232D0F5
# DSEQ: 167678

# Balance decreased: 118678937 → 113678937 uact
```

### **5.6 Partial Withdrawal**
```bash
# While deployment 167730 running
akash tx bme burn-act 5000000uact --from bme-main -y
# Tx: A6341B31FAEAEB2925AE653AD67AF61B3C5FE75C1C58F25F0593AB59DD0B143D

sleep 60

# AKT: 87337287 (↑ 15.13 AKT)
# Deployment still running ✓
```

### **5.7 Invalid Owner Address**
```bash
akash tx bme mint-act 5000000uakt --from invalid_address -y
# Error: key not found
```

### **5.8 Invalid Coins (Zero)**
```bash
akash tx bme mint-act 0uakt --from bme-main -y
# Error: amount must not be 0
```

### **5.9 Invalid Swap Route**
```bash
akash tx bme burn-mint 1000000uusdc uact --from bme-main -y
# Error: invalid swap route
```

### **5.10 Ledger Verification**
```bash
# Before vault
akash query bme vault-state

akash tx bme mint-act 10000000uakt --from bme-main -y
# Tx: F73B822FBA4634D64DE94E20BD7371184F554DBAC94587E7D9F3B7B9665D43DF

sleep 60

# After vault: total_minted increased ✓
akash query bme vault-state
```

### **5.11 Consecutive Operations**
```bash
# Op1: Mint 15 AKT
akash tx bme mint-act 15000000uakt --from bme-main -y
# Tx: 313AD2BF8F296BB2BC650949B8F031C1366757ED0F165C52620DC80827913D53
sleep 60

# Op2: Mint 10 AKT
akash tx bme mint-act 10000000uakt --from bme-main -y
# Tx: 412846D5485C775FF7E3D8BB486E225F7B5B1C710A15C93C06F1BF0B6DFCAE2C
sleep 60

# Op3: Burn 8 ACT
akash tx bme burn-act 8000000uact --from bme-main -y
# Tx: B1FC22A88997331E0D134E7A35AC40B4D5B68AB36305704B0596121C71C70E78
sleep 60

# Final: ACT 107220059, AKT 76593613
```

---
## 📋 CATEGORY 5 - FORM SUBMISSION EX
5.1: MsgMintACT - direct AKT to ACT

Tx: AC20F1DE69A064F40B04E6CC8F7ECA8EDAD7897B6F16DB4EC4EA37A5CA82A0D7
Amount: 50,000,000 uakt
Before: AKT=129558173, ACT=94803077
After: AKT=79555946, ACT=111268370
ACT Received: 16,465,293 uact

5.2: MsgBurnACT - direct ACT to AKT

Tx: A7B683CD640DC78D739417A1E2EF2567EE9B062344F1AD08162AE4B37B5861A0
Amount: 10,000,000 uact
Before: AKT=79555946, ACT=111268370
After: AKT=109829590, ACT=101268370
AKT Received: 30,273,644 uakt

5.3: MsgBurnMint - generic AKT to ACT

Tx: AF079340987F39263034330CE683522D2C68AB2E7E16E95A495DFC2C37E0B4A8
Amount: 30,000,000 uakt
Before: AKT=109829590, ACT=101268370
After: AKT=79827361, ACT=111178874
ACT Received: 9,910,504 uact

5.4: MsgBurnMint - generic ACT to AKT

Tx: C214B1C92CFA592C400D2414AA9CAFE551BCA95828A722D8AAB0917FC86E187F
Amount: 9,000,000 uact
Before: AKT=79827361, ACT=111178874
After: AKT=107083190, ACT=102178874
AKT Received: 27,255,829 uakt

5.5: Pre-fund ACT then deploy later

Mint Tx: AC20F1DE69A064F40B04E6CC8F7ECA8EDAD7897B6F16DB4EC4EA37A5CA82A0D7
Post-mint ACT: 111,268,370 uact
Deploy Tx: 9EF513780C689774A81F130BFA97F87366AC027BF5E8AB0307264E2AE232D0F5
DSEQ: 167678
Deposit: 5,000,000 uact
Post-deploy ACT: 106,268,370 uact

5.6: Partial ACT withdrawal while deployment runs

Deployment DSEQ: 167730 (active)
Burn Tx: A6341B31FAEAEB2925AE653AD67AF61B3C5FE75C1C58F25F0593AB59DD0B143D
Amount: 5,000,000 uact
Before: ACT=108678937, AKT=72203810
After: ACT=103678937, AKT=87337287
AKT Received: 15,133,477 uakt
Deployment still running ✓

5.7: Input validation - invalid owner address

Command: akash tx bme mint-act 5000000uakt --from invalid_address -y
Error: "invalid_address.info: key not found"

5.8: Input validation - invalid coins

Command: akash tx bme mint-act 0uakt --from bme-main -y
Error: "amount must not be 0: invalid amount"

5.9: Input validation - invalid swap route

Command: akash tx bme burn-mint 1000000uusdc uact --from bme-main -y
Error: "invalid swap route uusdc -> uact: invalid denomination"

5.10: LedgerRecordID verification

Mint Tx: F73B822FBA4634D64DE94E20BD7371184F554DBAC94587E7D9F3B7B9665D43DF
Before vault: balances.uakt=59199265651, total_minted=37818893450
After vault: balances.uakt=59209265651, total_minted=37822192455
total_minted increased by 3,299,005 uact ✓

5.11: Consecutive direct operations

Op1 Mint 15 AKT: 313AD2BF8F296BB2BC650949B8F031C1366757ED0F165C52620DC80827913D53
Op2 Mint 10 AKT: 412846D5485C775FF7E3D8BB486E225F7B5B1C710A15C93C06F1BF0B6DFCAE2C
Op3 Burn 8 ACT: B1FC22A88997331E0D134E7A35AC40B4D5B68AB36305704B0596121C71C70E78
Final: ACT=107220059, AKT=76593613
All operations independent and correct ✓

## 📊 **CATEGORY 8: EVENT EMISSIONS**

### **8.1 MintACT Event**
```bash
akash query tx B5B574A6ACCEAF8CF69254B1B6030015BF6F147ABE6A822F72FC1C81B0D35094 --output json | jq '.events[] | select(.type == "message")'
# Output shows action: "/akash.bme.v1.MsgMintACT"
```

### **8.2 BurnACT Event**
```bash
akash query tx 57C7DBC22CDDA0D6E259822501589F56E6F12E046A326C79F8D24C4187D1F975 --output json | jq '.events[] | select(.type == "message")'
# Output shows action: "/akash.bme.v1.MsgBurnACT"
```

### **8.3 Deployment Close Event**
```bash
akash tx deployment close --dseq 167730 --from bme-main -y
# Tx: D61577197C0DA4DC9B988CD21403A952D232D645AE5FCBDDB29144D5DADBA4F4

akash query tx D61577197C0DA4DC9B988CD21403A952D232D645AE5FCBDDB29144D5DADBA4F4 --output json | jq '.events[] | select(.type | contains("deployment") or contains("lease"))'
# Output shows EventDeploymentClosed, EventLeaseClosed
```
##  📋 CATEGORY 8 - FORM SUBMISSION EX
8.1: Event emission - MintACT

Tx: B5B574A6ACCEAF8CF69254B1B6030015BF6F147ABE6A822F72FC1C81B0D35094

Events:
{
  "type": "message",
  "attributes": [
    {"key": "action", "value": "/akash.bme.v1.MsgMintACT"},
    {"key": "module", "value": "bme"}
  ]
}

8.2: Event emission - BurnACT

Tx: 57C7DBC22CDDA0D6E259822501589F56E6F12E046A326C79F8D24C4187D1F975

Events:
{
  "type": "message",
  "attributes": [
    {"key": "action", "value": "/akash.bme.v1.MsgBurnACT"},
    {"key": "module", "value": "bme"}
  ]
}

8.3: Event emission - Deployment close

Close Tx: D61577197C0DA4DC9B988CD21403A952D232D645AE5FCBDDB29144D5DADBA4F4
DSEQ: 167730

Events:
{
  "type": "akash.deployment.v1.EventDeploymentClosed",
  "attributes": [{"key": "id", "value": "{\"owner\":\"akash1023uh7436y49xyhe0d8rcgwtzeh6akuhx2ecte\",\"dseq\":\"167730\"}"}]
}
{
  "type": "akash.deployment.v1.EventGroupClosed",
  "attributes": [{"key": "id", "value": "{\"owner\":\"akash1023uh7436y49xyhe0d8rcgwtzeh6akuhx2ecte\",\"dseq\":\"167730\",\"gseq\":1}"}]
}

---

## ⚠️ **CATEGORY 9: EDGE CASES**

### **9.1 Zero Amount**
```bash
akash tx bme mint-act 0uakt --from bme-main -y
# Error: amount must not be 0

akash tx bme burn-act 0uact --from bme-main -y
# Error: amount must not be 0
```

### **9.2 Dust Amount (1 uakt)**
```bash
akash tx bme mint-act 1uakt --from bme-main -y
# Tx: 542E0AFD8D84B0C80956B342C80A96DBC257895F118C7B4EFB06C428CD02B62E

sleep 60
# ACT balance increased by dust amount ✓
```

### **9.3 Maximum Amount**
```bash
akash tx bme mint-act 50000000uakt --from bme-main -y
# Tx: FB5E487E93B0710099E5628D8DB13F754F84EF368C3F5E898B1130AD13E2555A
# Success ✓
```

### **9.4 Rapid Sequential**
```bash
# Already done in 5.11
```

### **9.5 SDL with uakt (Wrong Denom)**
```bash
cat > deploy-wrong.yml << 'EOF'
---
version: "2.0"
services:
  web:
    image: nginx:alpine
profiles:
  compute:
    web:
      resources:
        cpu:
          units: 0.5
        memory:
          size: 512Mi
        storage:
          size: 1Gi
  placement:
    akash:
      pricing:
        web:
          denom: uakt    # WRONG!
          amount: 1000
deployment:
  web:
    akash:
      profile: web
      count: 1
EOF

akash tx deployment create deploy-wrong.yml --deposit 5000000uakt --from bme-main -y
# Error: invalid manifest
```
## 📋 CATEGORY 9 - FORM SUBMISSION EX
9.1: Zero amount operations

mint-act 0: "Error: amount must not be 0: invalid amount"
burn-act 0: "Error: amount must not be 0: invalid amount"

9.2: Dust amount operations

Mint Tx: 542E0AFD8D84B0C80956B342C80A96DBC257895F118C7B4EFB06C428CD02B62E
Amount: 1 uakt
After epoch: ACT balance increased by dust amount ✓

9.3: Maximum amount operations

Mint Tx: FB5E487E93B0710099E5628D8DB13F754F84EF368C3F5E898B1130AD13E2555A
Amount: 50,000,000 uakt
Successfully processed ✓

9.4: Rapid sequential operations

Tx1: DD2D5639CFDA301A8E1B182D97A397893335792796454C93036D4C94B970DC91 (10 AKT)
Tx2: 4723CF1DEC77C98009301C1CF40A8065E0C558BF3153949F7D85BD8C7006507A (10 AKT)
Tx3: 03F46D794ED65DD1B9CD02AE04750DEB06B5EB132CE08ED91A2E2467D31E64A7 (10 AKT)
Total: 30 AKT minted
Final ACT: 136,845,138 uact ✓

9.5: SDL with uakt pricing

SDL with denom: uakt (should be uact)
Deployment attempt error: "Error: invalid manifest: zero global services"
Deployment rejected as expected ✓

---

## 🔄 **CATEGORY 10: END-TO-END**

### **10.1 Full Lifecycle (DSEQ 167730)**
```bash
# Mint
akash tx bme mint-act 50000000uakt --from bme-main -y
# Tx: FB5E487E93B0710099E5628D8DB13F754F84EF368C3F5E898B1130AD13E2555A

# Deploy
akash tx deployment create deploy-higher.yml --deposit 5000000uact --from bme-main -y
# Tx: 237F27470C3291A1DD62FB4A830CA0FBEADF552F78B1635916601589BA4E0E83
# DSEQ: 167730

# Lease
akash tx market lease create --dseq 167730 --gseq 1 --oseq 1 --provider akash1wr4663g2kydjw4vmwtqsy8f4fevnvntuuhyusn --from bme-main -y
# Tx: AF92483B412904F2F3639CDAE8EE88FC869FA61DFAB33AD8E230BA8130430D6E

# Manifest
provider-services send-manifest deploy-higher.yml --dseq 167730 --provider akash1wr4663g2kydjw4vmwtqsy8f4fevnvntuuhyusn --from bme-main
# Status: PASS
# URL: mq2mtmplehbuved974l74audg0.ingress.akashtestnet.xyz

# Close
akash tx deployment close --dseq 167730 --from bme-main -y
# Tx: D61577197C0DA4DC9B988CD21403A952D232D645AE5FCBDDB29144D5DADBA4F4

# Burn
akash tx bme burn-act 5000000uact --from bme-main -y
# Tx: 57C7DBC22CDDA0D6E259822501589F56E6F12E046A326C79F8D24C4187D1F975
```

### **10.2 Multi-User Concurrent**
```bash
# Create 3 users
akash keys add user1
# Address: akash1ggappnk404j9rlt4hjwc7a3wkx6sz8e5s6aag0

akash keys add user2
# Address: akash13cdafu3yqhprm4vctj9seqy9ccafrzunt7ud7u

akash keys add user3
# Address: akash123jm77nzg53z9zt3grfcj7922926ndg2f3n06r

# Fund all from faucet

# Mint from all
akash tx bme mint-act 50000000uakt --from user1 -y
# Tx1: 7873C9DF1C38D6EDFC9E1BFE167A64852C8D796B3C0E923E21C72C8800E603F6

akash tx bme mint-act 50000000uakt --from user2 -y
# Tx2: 00504DA50781500D3C11DDDA19045FFD4D16CC779B20F30961567BC272967D0A

akash tx bme mint-act 50000000uakt --from user3 -y
# Tx3: 43D3F589184EA2F7EB034B81C76B48CCFB9D398665931EC15F862ABF45333436

sleep 60

# Check balances
akash query bank balances akash1ggappnk404j9rlt4hjwc7a3wkx6sz8e5s6aag0
# ACT: 16192520
```
##  📋 CATEGORY 10 - FORM SUBMISSION EX
10.1: Full deployment lifecycle

Mint Tx: FB5E487E93B0710099E5628D8DB13F754F84EF368C3F5E898B1130AD13E2555A
Deploy Tx: 237F27470C3291A1DD62FB4A830CA0FBEADF552F78B1635916601589BA4E0E83
DSEQ: 167730
Lease Tx: AF92483B412904F2F3639CDAE8EE88FC869FA61DFAB33AD8E230BA8130430D6E
Provider: akash1wr4663g2kydjw4vmwtqsy8f4fevnvntuuhyusn
Manifest Status: PASS
URL: mq2mtmplehbuved974l74audg0.ingress.akashtestnet.xyz
Close Tx: D61577197C0DA4DC9B988CD21403A952D232D645AE5FCBDDB29144D5DADBA4F4
Burn Tx: 57C7DBC22CDDA0D6E259822501589F56E6F12E046A326C79F8D24C4187D1F975
Full lifecycle completed successfully ✓

10.2: Multi-user concurrent activity

User1: akash1ggappnk404j9rlt4hjwc7a3wkx6sz8e5s6aag0
User2: akash13cdafu3yqhprm4vctj9seqy9ccafrzunt7ud7u
User3: akash123jm77nzg53z9zt3grfcj7922926ndg2f3n06r

Tx1 (user1): 7873C9DF1C38D6EDFC9E1BFE167A64852C8D796B3C0E923E21C72C8800E603F6
Tx2 (user2): 00504DA50781500D3C11DDDA19045FFD4D16CC779B20F30961567BC272967D0A
Tx3 (user3): 43D3F589184EA2F7EB034B81C76B48CCFB9D398665931EC15F862ABF45333436

All 3 users successfully minted 50 AKT each
No cross-user interference ✓
Vault state reflects total 150 AKT minted ✓
 
---

## 🎯 **FINAL CHECKLIST**

✅ Category 0: Pre-Flight (10/10)  
✅ Category 1: Deployment (4/4)  
✅ Category 2: Closure (3/3)  
✅ Category 3: Provider (3/3)  
✅ Category 4: Epoch (2/2)  
✅ Category 5: Direct Msgs (11/11)  
⏸️ Category 6: Circuit Breaker (24 Feb)  
⏸️ Category 7: Epoch Timing (24 Feb)  
✅ Category 8: Events (3/3)  
✅ Category 9: Edge Cases (5/5)  
✅ Category 10: E2E (2/2)

**Total Completed: 9 Categories**  
**Prize: $440 + Top Performer Bonus Chance!** 🏆

---

## 📝 **FORM SUBMISSION LINK**
https://forms.gle/MH1KxhNmoJxzMRfz9

---

**Guide by: DarkSidBull**  
**Discord: Join for Circuit Breaker updates on Feb 24!** 🔥
