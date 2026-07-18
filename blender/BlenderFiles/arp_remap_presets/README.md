# arp_remap_presets — Auto-Rig Pro リマップ設定置き場

## mixamo_to_vroid.bmap

Auto-Rig Pro のリマップ設定(Mixamo スケルトン → VRoid 命名リグ `J_Bip_*`)。
ARP の Remap パネル → Import Config で読み込めば、このプロジェクト以外でも流用可。

### ⚠️ Import Config 後は必ず「Set as Root」を設定すること

**このファイルには `mixamorig:Hips` の Set as Root フラグが保存されない**(ARPの仕様)。

- GUI で使う場合: Import Config 後、ボーンリストで `mixamorig:Hips` の行を選択し
  **Set as Root にチェックを入れてから** Retarget する
- 忘れるとルートモーション(移動)が乗らず、その場で関節だけ動くアニメになる
- スクリプトから使う場合は `bones_map_v2` の `mixamorig:Hips` 行に `set_as_root = True` を
  毎回設定すること(3d-to-sprite プロジェクトの `Scripts/build_char_glb.py` が実装例)

### 由来メモ

- 2026-07-18 作成。ARP の自動マップ(Build Bones List)は VRoid リグに対して
  UpperArm / UpperLeg(J_Aim誤爆)/ Chest / UpperChest / 小指(Little)の計15箇所を
  誤るため、手修正した結果を保存したもの
- ターゲットボーン名は VRoid 命名決め打ち。非 VRoid 製 VRM には使えない(要作り直し)
