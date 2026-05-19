# ZMK → QMK Conversion Plan
## Corne (ZMK) → TOTEM (QMK)

---

## Dropped Keys

The Corne has 6 columns per side; the TOTEM has 5 + 1 outer pinky on the bottom row only.
The outermost column (col 0 left, col 11 right) rows 0–1 are dropped. Row 2 outer maps to the TOTEM outer pinky key.

| Side | Row | ZMK Key | Fate |
|------|-----|---------|------|
| Left | 0 | `ESC` | Dropped |
| Left | 1 | `MT(LGUI, TAB)` | Dropped |
| Right | 0 | `DELETE` | Dropped |
| Right | 1 | `APOS` | Dropped |
| Left | 2 | `LSHFT` | → TOTEM left outer pinky key |
| Right | 2 | `LSHFT` | → TOTEM right outer pinky key |

---

## Layer Mapping

| Index | QMK Name | ZMK Source |
|-------|----------|------------|
| 0 | `_DEFAULT` | `default_layer` |
| 1 | `_NUM` | `num_layer` |
| 2 | `_SYM` | `sym_layer` |
| 3 | `_NAV` | `nav_layer` |
| 4 | `_MEDIA` | `media_layer` |
| 5 | `_GAME` | `test` |
| 6 | `_MOUSE` | `layer_7` (mouse) |
| 7 | `_ADJUST` | `adjust_layer` (tri-layer: 1+2) |

---

## Key Conversions

| ZMK | QMK |
|-----|-----|
| `&hrl MOD KEY` / `&hrr MOD KEY` | `MT(MOD, KC)` |
| `&lt_alt N KC` | `LT(N, KC)` |
| `&lt N KC` | `LT(N, KC)` |
| `&mo N` | `MO(N)` |
| `&tog N` | `TG(N)` |
| `&trans` | `_______` |
| `&none` | `XXXXXXX` |
| `&mmv MOVE_UP/DOWN/LEFT/RIGHT` | `KC_MS_UP/DOWN/LEFT/RIGHT` |
| `&msc SCRL_UP/DOWN/LEFT/RIGHT` | `KC_WH_U/D/L/R` |
| `&mkp MB1/MB2/MB3` | `KC_BTN1/BTN2/BTN3` |
| `shake_mouse` macro | `SHAKE_MOUSE` custom keycode |
| `conditional_layers (1+2 → 7)` | `update_tri_layer_state()` in `layer_state_set_user` |
| `bt BT_CLR` / `bt BT_SEL x` | `XXXXXXX` (no QMK equivalent) |

---

## Behavioral Approximations

- **`hrl`/`hrr`** used `flavor = "balanced"` + `require-prior-idle-ms = 150` + `tapping-term-ms = 250`
  → QMK: `MT()` with `TAPPING_TERM 250`, `QUICK_TAP_TERM 175`, and `PERMISSIVE_HOLD` in `config.h`

- **`lt_alt`** used `flavor = "tap-preferred"` + `tapping-term-ms = 200` + `quick-tap-ms = 180`
  → QMK: standard `LT()` — tap-preferred by default when `HOLD_ON_OTHER_KEY_PRESS` is not set

---

## Special Notes

- **`_GAME` layer**: `&tog 5` was on the dropped right outer column → relocated to the **right outer bottom key** so the layer can be toggled off
- **Mouse keys**: require `MOUSEKEY_ENABLE = yes` in `rules.mk`
- **`shake_mouse`**: implemented as a `SHAKE_MOUSE` custom keycode that calls `tap_code(KC_MS_LEFT)` then `tap_code(KC_MS_RIGHT)`
- **Tri-layer**: handled via `layer_state_set_user` + `update_tri_layer_state(state, _NUM, _SYM, _ADJUST)`

---

## Files to Create/Modify

| File | Action | Purpose |
|------|--------|---------|
| `keymaps/hamihu/keymap.c` | Rewrite | 8-layer keymap + macros + tri-layer logic |
| `keymaps/hamihu/rules.mk` | Create | `MOUSEKEY_ENABLE = yes` |
| `keymaps/hamihu/config.h` | Create | `TAPPING_TERM`, `QUICK_TAP_TERM`, `PERMISSIVE_HOLD` |
