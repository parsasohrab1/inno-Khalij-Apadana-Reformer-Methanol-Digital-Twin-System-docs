# TRL assessment (evidence-based)

**Level reached: TRL 4 for the individual components, on SYNTHETIC data only.** TRL 5 is not claimed.

TRL 4 here = components validated in a laboratory environment. Evidence: `pytest` (CI on every push) runs 9 tests on

| Component | Module | Validated against |
|---|---|---|
| Creep rupture law (Larson-Miller type) + Robinson damage rule + remaining life | `creep.py` | monotonicity, exact recovery of a known law from noise-free tests, damage arithmetic |
| Catalyst activity trend, time to threshold with an interval | `catalyst.py` | synthetic decay with known slope (error under 15 % at 5,000 samples; the SRS target is MAPE under 15 %) |
| Syngas module ratio and band check | `syngas.py` | formula |
| Tiered alerts | `alerts.py` | tier boundaries |
| Synthetic data generator (SRS section 3) | `simulator.py` | label consistency, determinism |

## Not done (and why)
- The rupture-law constants are **illustrative**, not alloy data; fit them to laboratory rupture tests (`fit_rupture_law`) before any use.
- **No plant (DCS) data** has been used, so accuracy on a real unit is unknown (TRL 5 needs a relevant environment).
- The joint decision loop that couples tube life, catalyst state and the module-ratio optimiser (the product's core claim, SRS section 1) is **intentionally not implemented** in this public repository, pending the patent attorney's review.
- No streaming/Kafka/TimescaleDB layer yet; latency (NFR-SAFE-01) and availability (NFR-AVAIL-01) are not measured.

## Next steps toward TRL 5
1. Obtain historian data and inspection records (tube wall measurements, catalyst replacement dates) under a data-sharing agreement.
2. Fit and validate the rupture law and the catalyst trend on that data with unit-held-out splits.
3. Build the ingestion service and measure latency.
