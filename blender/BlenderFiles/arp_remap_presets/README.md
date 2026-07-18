# arp_remap_presets — Auto-Rig Pro リマップ設定置き場

## mixamo_to_vroid.bmap

Auto-Rig Pro のリマップ設定(Mixamo スケルトン → VRoid 命名リグ `J_Bip_*`)。
ARP の Remap パネル → Import Config で読み込めば、このプロジェクト以外でも流用可。

### ⚠️ Import Config 後は必ず「Set as Root」を設定すること

**このファイルには `mixamorig:Hips` の Set as Root フラグが保存されない**(ARPの仕様)。

- GUI で使う場合: Import Config 後、ボーンリストで `mixamorig:Hips` の行を選択し
  **Set as Root にチェックを入れてから** Retarget する
- 忘れるとルートモーション(移動)が乗らず、その場で関節だけ動くアニメになる
- スクリプトから使う場合は Import Config 後に `bones_map_v2` の `mixamorig:Hips` 行へ
  `set_as_root = True` を毎回設定すること

### ⚠️ 揺れもの(VRMスプリングボーン)はリターゲットで動かない

リターゲットでキーフレームが乗るのはマッピングされたヒューマノイドボーン(`J_Bip_*`)だけ。
髪・スカート・胸などの揺れものボーン(VRoid では `J_Sec_*` 系)には何も乗らず、親に固定追従する(剛体化)。

そもそも VRM の揺れものは「実行時にシミュレートする」VRM 拡張の機能で、
glTF/FBX へ変換した時点で物理定義ごと失われる。対処の選択肢:

1. **切り捨てる** — 低解像度スプライトなど、揺れが視認できない用途ならこれで十分
2. **Blender でベイクする** — VRM アドオンのスプリングボーンシミュレーションで揺らし、
   `J_Sec_*` へキーフレームとして焼いてからエクスポートする。どのフォーマットにも運べる汎用解
3. **持ち出し先で再実装** — エンジン側のジグルボーン等で揺れを付け直す
4. **VRM 対応ランタイムに留まる** — UniVRM / three-vrm / godot-vrm 等は実行時に揺らせる
   (変換を挟まない構成のみ)

### 由来メモ

- 2026-07-18 作成。ARP の自動マップ(Build Bones List)は VRoid リグに対して
  UpperArm / UpperLeg(J_Aim誤爆)/ Chest / UpperChest / 小指(Little)の計15箇所を
  誤るため、手修正した結果を保存したもの
- ターゲットボーン名は VRoid 命名決め打ち。非 VRoid 製 VRM には使えない(要作り直し)
