🌍 **Location Verification System**🌍 

A smart system that checks whether a user is really where they claim to be to prevent fraud, meet legal requirements, and keep the experience smooth for genuine users.

**What Problem Does This Solve?**

When someone signs up for a financial app or makes a transaction, the system needs to confirm their location is legitimate. Without this, bad actors can:

Pretend to be in a different country to bypass restrictions

Use fake GPS apps to spoof their location

Access services from sanctioned or restricted regions

At the same time, if the system is too strict, real users get blocked — causing frustration and lost business.

This project builds a system that gets that balance right.


**How It Works** 


Think of it like a smart security guard at a building entrance. Instead of just checking one ID, it looks at multiple clues before deciding whether to let someone in:

Where does your phone's GPS say you are?

What Wi-Fi networks are nearby? (matches your claimed location?)

What does your internet address (IP) suggest about your location?

Is your phone behaving normally? (no fake GPS apps, not an emulator)

Does this match how you've behaved before? (unusual travel? new device?)

If everything checks out →  Let them through (no friction)
If something looks off →  Ask for extra verification (SMS OTP)
If it looks really suspicious →  Flag for human review

**Location Spoofing Detection — Technical Layers**

**Layer 1** — Device Integrity Checks

Detect if mock location apps (e.g., Fake GPS) are installed or enabled
​

Check for root/jailbreak indicators on iOS and Android

Identify emulator or virtual device signatures

Verify developer mode is not active for location overrides

**Layer 2** — Multi-Signal Cross-Validation

Compare GPS coordinates vs. IP Geolocation — flag mismatches (e.g., GPS → New York, IP → India)
​

Compare against Wi-Fi triangulation (harder to spoof than GPS alone)

Compare against Cell Tower data for urban accuracy

**Layer 3** — Signal Anomaly Detection

Analyze GPS signal power levels — spoofed signals are often too strong or unnaturally consistent
​

Detect impossible travel: user in London 5 mins ago, now GPS shows Tokyo

Monitor velocity checks between consecutive location pings

**Layer 4** — Environmental Consistency

Correlate accelerometer/gyroscope with location movement (no movement on sensors but GPS shows travel = suspicious)
​

Validate timezone consistency with claimed GPS location

Match daylight/sensor light patterns with claimed geographic time

**Layer 5** — Behavioral ML Model

Build a per-user location behavior profile (normal device, normal region, normal hours)

Flag deviations from established patterns (new device + new location + high-value transaction = high risk)

Continuous retraining from manual review feedback loops


**Goals of This Project**

Reduce the number of real users accidentally blocked (false positives)

Catch fraudsters faking their location (false negatives)

Keep the experience fast and smooth for genuine users

Stay compliant with international regulations (KYC, AML, OFAC)

Make the system smarter over time using feedback from past decisions

**Key Features** --------------

| Feature                   | What It Does                                                                             |
| ------------------------- | ---------------------------------------------------------------------------------------- |
|  Multi-Signal Detection | Combines GPS, Wi-Fi, cell towers, and IP address — not just one source                   |
|  Spoofing Detection    | Catches fake GPS apps, VPNs, rooted phones, and device simulators                        |
|  Smart Risk Scoring     | Assigns a risk level (Low / Medium / High) to each verification attempt                  |
|  Risk-Based Decisions   | Low risk = auto-approved; Medium = extra check; High = human review                      |
|  Compliance Engine      | Enforces country-specific rules and blocks transactions from restricted regions          |
|  Live Dashboard         | Tracks how many users are blocked, fraud caught, and system performance in real time     |
|  Learns Over Time       | Gets smarter using feedback — past review decisions train the system to be more accurate |

**Decision Flow (How a Verification Request is Handled)**
<img width="881" height="846" alt="Location Verification Service drawio" src="https://github.com/user-attachments/assets/5601c930-e652-4b99-a78d-d51c984197e7" />

**Rule**: Never hard-block a user outright — always offer an alternative path. This reduces support tickets and protects genuine users.


**How Spoofing is Detected — Step by Step**

**Step 1** — Check the Device First
Before even looking at location, the system checks if the phone itself can be trusted:

Is it a real device or a computer simulator?

Is it rooted/jailbroken (which allows system-level location faking)?

Is a fake GPS app installed or running?

**Step 2 ** — Compare Multiple Location Sources
GPS alone can be faked. So the system cross-checks:

GPS coordinates vs. IP address location

GPS vs. nearby Wi-Fi networks

GPS vs. cell towers in the area

If GPS says "New York" but everything else says "India" → 🚨 Flag it.

**Step 3 ** — Check if the Journey Makes Physical Sense
Was the user in London 5 minutes ago and now GPS says Tokyo? → Impossible travel detected.

Is the phone showing movement on GPS but the motion sensor says it's sitting still? → Likely faked.

**Step 4 ** — Check the Environment
Does the timezone on the device match the claimed GPS location?

Do light sensor and pressure sensor readings match the expected environment for that location?

**Step 5 ** — Compare Against the User's Own History
Does this location match where the user usually operates?

Is this a new device + new location + large transaction all at once? → Higher risk.

The system builds a normal behavior profile per user and flags anything unusual.

**Risk-Based Decision Framework**

Risk Score 0–30  → AUTO APPROVE       (Low friction, trusted user path)
Risk Score 31–70 → STEP-UP VERIFY     (SMS OTP / re-prompt location permission)
Risk Score 71–100 → MANUAL REVIEW     (Human review queue with rich context)



**How Success is Measured**

| Metric              | What It Means                             | Target                       |
| ------------------- | ----------------------------------------- | ---------------------------- |
| False Positive Rate | % of blocks that were actually real users | Reduce from ~90% → below 50% |
| Block Rate          | % of all users getting blocked            | Reduce from ~5% → 2–3%       |
| Fraud Catch Rate    | % of actual fraud caught                  | Maintain or improve          |
| Verification Speed  | Time taken to verify a user               | Under 3 seconds              |
| Manual Review Load  | Cases escalated to human reviewers        | Reduce by 30%                |
| Conversion Rate     | % of users completing onboarding          | Improve by 1–2%              |


**Project Roadmap**

**Phase 1** — Build the Foundation (Weeks 1–6)

Collect signals from GPS, IP address, and Wi-Fi

Basic device checks (fake GPS, emulator detection)

Simple risk rules to flag obvious fraud

API that responds in under 1 second

Basic dashboard to track block rates

**Phase 2 **— Make It Smarter (Weeks 7–14)

Add a learning model that improves from past decisions

Run the new logic alongside the old one first (no risk to users) — then gradually switch over

A/B testing to measure if changes actually improve things

Feedback loop: human review decisions improve future accuracy

**Phase 3 **— Scale & Compliance (Weeks 15–24)

Add country-specific rules (e.g., OFAC sanctions, GDPR privacy)

Geo-fencing to block transactions from restricted regions

Motion sensor correlation for deeper spoofing detection

On-device checks (runs on phone, faster and more private)

**Real-World Context**

This system was designed with global fintech use cases in mind — inspired by challenges faced at companies like Wise, which processes cross-border payments and caters to million users. Location verification is a core requirement for:

Meeting international KYC/AML regulations

Supporting instant payments securely

Reducing onboarding drop-off while keeping fraud low


**About This Project**

Built as part of a portfolio exploring fraud detection and identity verification systems in fintech. Feedback and contributions are welcome — feel free to open an issue or pull request.

