
# Stacks-TrustAnchor - Smart Contract

This Clarity smart contract provides a decentralized framework for registering, inspecting, and monitoring websites for safety and threat assessment within a trust-based ecosystem. It enables the secure onboarding of websites, endorsement validation, sentinel monitoring, threat registration, and reward or penalty mechanisms based on credibility scores.

---

## 📦 Features

* ✅ **Site Registration**
  Websites can be registered with a security endorsement and locked collateral to ensure commitment.

* 🔒 **Threat Detection System**
  Malicious sites can be logged with proof documentation, severity level, and victim count.

* 🧠 **Sentinel Monitoring**
  Sentinels are external actors who perform site assessments and accumulate credibility scores.

* 🛠️ **Validation Logic**
  Built-in validation for identifiers, endorsements, threat magnitude, and more.

* 🔍 **Performance Logging**
  Tracks how well sentinels perform with verified submissions, precision metrics, and credibility.

* ⏸️ **System Pause State**
  Contract includes a global pause feature to halt operations if needed.

---

## 🏗️ Data Structures

### `registered_sites`

Stores data about registered websites.

```clarity
{
  site_proprietor: principal,
  authentication_level: (string-ascii 20),
  registration_epoch: uint,
  threat_metric: uint,
  incident_tally: uint,
  locked_assets: uint,
  safety_check_epoch: uint,
  security_endorsement: (string-ascii 50)
}
```

### `malicious_site_registry`

Tracks reported malicious sites with metadata for detection.

```clarity
{
  alerting_entity: principal,
  detection_epoch: uint,
  proof_documentation: (string-ascii 500),
  confirmation_state: (string-ascii 20),
  threat_magnitude: uint,
  victim_count: uint
}
```

### `sentinel_registry`

Logs registered sentinels and their activity/credibility.

### `sentinel_performance_log`

Tracks each sentinel's interaction with specific websites.

### `site_inspection_records`

Records site inspections and safety evaluations.

---

## ⚙️ System Constants

| Constant                      | Description                             |
| ----------------------------- | --------------------------------------- |
| `INACTIVITY_WINDOW`           | 24-hour inactivity window (in seconds)  |
| `BASE_COLLATERAL_REQUIREMENT` | Minimum STX required to register a site |
| `TRUSTWORTHINESS_BASELINE`    | Baseline credibility score              |
| `EVIDENCE_STRING_LIMIT`       | Max string length for proof submissions |

---

## ❗ Error Codes

| Code          | Meaning             |
| ------------- | ------------------- |
| `u100`        | Access forbidden    |
| `u101`        | Duplicate entry     |
| `u102`        | Entry missing       |
| `u103`        | Operation blocked   |
| `u104`        | Missing collateral  |
| `u105`        | Time restriction    |
| `u106`        | Limit breach        |
| `u107`        | Temporal error      |
| `u400`–`u405` | Invalid input types |

---

## ✅ Core Public Function

### `register_secure_site`

Registers a new website with a required STX collateral and security endorsement.

#### Parameters:

* `web_identifier` – ASCII-255 string, validated for length and unsafe characters
* `security_endorsement` – ASCII-50 string, validated for format

#### Rules:

* Must be called by the `system_controller`
* Caller must have enough STX for the `collateral_requirement`
* Site must not already be registered

#### Returns:

* `(ok true)` on success
* Specific error codes on failure

---

## 🔐 Input Validation Functions

Each input type (web ID, endorsements, proof documentation, threat level, protection level) includes strong format validation, rejecting potentially dangerous or malformed data early in execution.

---

## 🔎 Read-Only Functions

* `check_threat_status`: Checks if a website is marked as malicious.
* `fetch_sentinel_rating`: Retrieves a sentinel's credibility score.
* `fetch_site_status`: Returns metadata for a registered site.

---
