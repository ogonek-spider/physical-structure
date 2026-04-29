# Changelog

## [Unreleased]

- Renamed `model/` folder to `cad/`.
- Added `OldVersions/` to `.gitignore` (matches all nested `OldVersions` folders).
- Added Inventor 2026 ↔ Python solver integration (per `docs/CLAUDE_CODE_TASK.md`):
  - `ilogic/extract_pose_and_solve.iLogicVb` — one-button rule: reads 18 joint
    angles by name, writes `hexapod_pose.json`, runs the solver under
    `PYTHONIOENCODING=utf-8`, displays the moments table in a Consolas WinForms
    dialog with copy-to-clipboard. Uses `BeginOutputReadLine` + double
    `WaitForExit` to avoid pipe deadlock.
  - `ilogic/extract_model.iLogicVb` — reads User Parameters
    (`coxa_length`/`femur_length`/`tibia_length`/`body_radius`,
    optional `body_mass_offset_*`) and per-occurrence `MassProperties.Mass`
    for body and the 6 legs. Per-link CoM is read from
    `MassProperties.CenterOfMass` (assembly cm → world m), then projected
    into the link-local frame via forward kinematics computed from the
    current joint angles (local CoM is pose-invariant). Falls back to
    `[length/2, 0, 0]` per-link if the segment / joint is missing, with a
    `*_com_simplified` flag in the JSON. Hexapod leg-occurrence mapping
    is non-trivial (L3 ↔ `leg_Right_ass:3`, R3 ↔ `leg_Left_ass:3`).
  - `README_HEXAPOD.md` — setup (User Parameters, joint renaming convention
    `joint_L1_coxa`…`joint_R3_tibia`, rule import) + workflow + sanity-check
    + troubleshooting.
  - JSON written via `InvariantCulture` (locale-safe `0.05`, not `0,05`),
    UTF-8 without BOM.
- iLogic compile fixes after first paste-into-Inventor attempt:
  - iLogic requires the entry to be exactly `Sub Main() … End Sub`, with
    helpers as siblings and **no** top-level executable code. Two rounds
    of fixes converged on this structure: all `Const`/`Dim` arrays live
    inside `Sub Main()` (top-level `Dim … = New T() {…}` triggers BC30024
    "statement not valid in namespace"), and there are no naked statement
    calls at file scope (those produce "rule must contain Sub Main()").
  - Replaced non-existent `RevoluteJointDefinition` / `CylindricalJointDefinition`
    casts with `AssemblyJoint.Definition.JointType` check
    (`kRotationalJointType` / `kCylindricalJointType`) and direct access to
    `Definition.AngularPosition.Value` (radians).
  - Removed the `MassProperties.Accuracy = …` optimization entirely:
    both `kVeryHighAccuracy` and `kVeryHighMassPropertiesAccuracy`
    failed to resolve in Inventor 2026's iLogic compiler. Default
    accuracy is sufficient for static torque calculations.
- `extract_model.iLogicVb`: link lengths and per-leg attachment now
  derived from geometry instead of User Parameters:
  - `coxa_length` / `femur_length` measured as Euclidean distances
    between sub-occurrence origins (`oCoxa.Transformation` translation
    → `oFemur` translation → `oTibia` translation, world cm → m).
    Assumes each part's origin is placed on its joint axis (standard
    modeling practice).
  - `coxa_attachment` (per-leg, body-local) and `yaw_offset` (per-leg,
    `atan2(attach_y, attach_x)`) computed from coxa occurrence position
    relative to the body. Removed `body_radius` parameter and hardcoded
    yaw fallback (`±60/±90/±120°`).
  - Only `tibia_length` remains as a User Parameter (no joint exists at
    the foot tip to measure against). Auto-created at 150 mm if missing.
