# Mass Calculations

## Aluminum Profile (40×60×4mm)

- Outer area: 40 × 60 = 2400 mm²
- Inner area: (40−8) × (60−8) = 32 × 52 = 1664 mm²
- Cross-section: 736 mm²
- Aluminum density: 2700 kg/m³
- **Linear density: ~2.0 kg/m**

## Per Leg

| Part | Length | Mass |
|------|--------|------|
| Coxa bracket (L1) | 0.25 m | 0.5 kg |
| Femur tube (L2) | 0.35 m | 0.70 kg |
| Tibia tube (L3) | 0.80 m | 1.60 kg |
| Joint mechanisms (3 × 5 kg) | — | 15 kg |
| **Leg total** | | **~17.8 kg** |

Joint weight (5 kg each) includes: motor (3 kg) + 2-stage planetary gearbox + PETG frame + 2× 61910 bearings.

## Full Robot

| Component | Count | Unit mass | Total |
|-----------|-------|-----------|-------|
| Legs | 6 | 17.8 kg | 107 kg |
| Body plywood (780×640×12mm) | 2 | 3.9 kg | 7.8 kg |
| Batteries (M365 Pro) | 2 | 2.9 kg | 5.8 kg |
| Electronics | — | — | ~1 kg |
| **Total** | | | **~122 kg** |

### Body plywood calculation
- Volume per plate: 0.780 × 0.640 × 0.012 = 5.99 × 10⁻³ m³
- Plywood density: 650 kg/m³
- Mass per plate: 3.9 kg

## Power & Battery

- Battery: Xiaomi M365 Pro — 36V, 12.8 Ah = 474 Wh, BMS limit ~30A (~1080W)
- 2 packs in parallel: 60A = 2160W headroom

| Average load | Runtime (1 pack) | Runtime (2 packs) |
|-------------|-----------------|-------------------|
| 500 W | ~57 min | ~114 min |
| 1000 W | ~28 min | ~57 min |

Single pack ruled out: peak draw (~2000W) exceeds single BMS limit.

## Structural Notes

### Leg mount (coxa to body plate)
- Wave gait: 5 legs supporting → 249 N vertical per leg
- Moment at mount ≈ 249 N × 0.8 m moment arm = ~200 N·m
- With 60mm bolt span: bolt tension ≈ 3300 N per bolt
- Plywood pullout (M8 + washer, 12mm): ~2–3 kN — **marginal**

Recommendations:
- Steel backing plates both sides (50×100mm, 3–4mm)
- Bolt spacing ≥ 100mm (reduces bolt tension to ~2000 N)
- 4 bolts instead of 2, or add alignment pin/bracket lip
