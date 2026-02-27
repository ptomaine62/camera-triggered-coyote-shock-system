# シャッター連動COYOTEビリビリシステム (Camera-Triggered COYOTE Shock System)

[English description follows Japanese]

デジタルカメラのシャッター（ストロボ同調信号）と、DG-LAB Coyoteを完全に同期させるためのハードウェアハック・プロジェクトです。
ボクが考案したこのシステムは、PCやマイコンを使ったプログラミングを一切必要としません。PawPrintの「外部電圧検出機能」と、コンデンサを用いた簡易RC遅延回路を組み合わせることで、**「シャッターを切った瞬間に、遅延ゼロで確実な刺激を送信する」**完全スタンドアロンのシステムを実現します。

This is a hardware hack project to perfectly synchronize a digital camera's shutter (flash sync signal) with the DG-LAB Coyote.
This system requires absolutely no programming with a PC or microcontrollers. By combining the PawPrint's "External Voltage Detection" feature with a simple RC delay circuit using a capacitor, it achieves a completely standalone system that **"sends a reliable stimulus with zero latency the exact moment the shutter is released."**

---

## 🌟 特徴 / Features

* **PC不要・ノーコード (No PC / No Code)**
  パソコンやPythonスクリプト等のプログラミングは一切不要です。
  *Requires no programming like Python scripts or a PC.*
* **遅延ゼロ (Zero Latency)**
  カメラのPCシンクロ接点を直接利用するため、ソフトウェア制御のようなタイムラグがありません。
  *Directly utilizes the camera's PC sync terminal, eliminating the lag associated with software control.*
* **100%の安定性 (100% Reliable)**
  10µFのコンデンサによるRC遅延回路により、カメラの数ミリ秒のショート信号をアプリが検知可能な時間（約0.1秒）に物理的に引き伸ばし、信号の取りこぼしを防ぎます。
  *An RC delay circuit with a 10µF capacitor physically stretches the camera's millisecond short signal to a duration the app can detect (~0.1 seconds), preventing missed triggers.*

## 🛠 用意するもの / Requirements

1. **デジタルカメラ / Digital Camera (Mirrorless or DSLR)**
   ホットシューを備えたもの（例：SONY α7Cなど）/ *Must have a hot shoe (e.g., SONY a7C).*
2. **ホットシューアダプター / Hot Shoe Sync Adapter**
   3.5mmジャック出力があるもの（例：エツミ E-6785）/ *Must have a 3.5mm output jack.*
3. **3.5mm オーディオケーブル / 3.5mm Audio Cable**
   片側を切断して使用します。/ *Cut one end to expose the wires.*
4. **USB Type-C DIP化基板 / USB Type-C Breakout Board**
   24ピン仕様で、`CC1`（または `A5`/`B5`）ピンが引き出せるもの。/ *24-pin board that exposes the `CC1` (or `A5`/`B5`) pin.*
5. **積層セラミックコンデンサ / 10µF Ceramic Capacitor**
   10µF (印字: 106) が最適です。極性なし、耐圧10V以上。/ *10µF (marked '106') is optimal. Non-polarized, 10V or higher.*
6. **DG-LAB Coyote & PawPrint**

## 🔌 配線図 / Wiring Diagram

<img width="1600" height="818" alt="001" src="https://github.com/user-attachments/assets/9ff2b07c-6390-46d1-8a02-eee5954c0630" />
<img width="1600" height="818" alt="002" src="https://github.com/user-attachments/assets/4ff72345-e1c6-445b-8a5b-96e34bb10f01" />


## ⚙️ 工作の手順 / Assembly Steps

1. 3.5mmオーディオケーブルの片方を切り落とし、内部の「芯線（プラス）」と「シールド線（マイナス/GND）」を剥き出しにします。
   *Cut one end of the 3.5mm audio cable and strip it to expose the inner "Tip (positive)" wire and the outer "Sleeve (negative/GND)" wire.*
2. 芯線と10µFコンデンサの片方の足をねじり合わせ、Type-C基板の `A5` (または `B5`) ピンにハンダ付けします。
   （※A5とB5をハンダでブリッジさせておくと、Type-Cを表裏どちらで挿しても機能します）
   *Twist the Tip wire with one leg of the 10µF capacitor and solder them to the `A5` (or `B5`) pin on the Type-C board.*
   *(Tip: Bridge A5 and B5 with solder so the Type-C works reversibly.)*
3. シールド線とコンデンサのもう片方の足をねじり合わせ、基板の `GND` ピンにハンダ付けします。
   *Twist the Sleeve wire with the other leg of the capacitor and solder them to the `GND` pin.*
4. 完成した基板をPawPrintのUSB-Cポートに接続し、3.5mmプラグをカメラのホットシューアダプターに接続します。
   *Plug the finished board into the PawPrint's USB-C port, and the 3.5mm plug into the camera's hot shoe adapter.*

## 📱 アプリの設定 / App Configuration

PawPrintの公式機能である「外部電圧検出トリガー（外部电压检测触发）」を使用します。
*Use the official "External Voltage Detection Trigger" feature in the PawPrint app.*

1. **トリガー追加 / Add Trigger:** 「外部電圧検出（External Voltage）」
2. **測定モード / Measurement Mode:** 「高電平（High Level / 内蔵プルアップ）」
3. **目標電圧範囲 / Target Voltage Range:** `0.80V 〜 1.50V`
   （待機時の約1.0Vをセーフゾーンとし、シャッターを切って0Vに落ちた時だけトリガーを発動させます / *Sets the ~1.0V standby voltage as the safe zone, triggering only when it drops to 0V upon shutter release.*）
4. **強度の一時的な変更 / Temporary Intensity Change:** 「パラメータ決定（参数决定）」を選択し、お好みの強度（例: `+10 ~ +40`）に設定します。/ *Select 'Parameter Determination' and set your desired intensity.*

## 💡 トラブルシューティング / Troubleshooting

* **たまに反応しない（取りこぼす）場合 / Occasional Missed Triggers:**
  コンデンサの容量が足りず、ONの時間が短すぎる可能性があります。10µFのコンデンサをもう一つ並列に追加（A5とGNDの間に追加）して容量を20µFに増やし、持続時間を約0.2秒に引き伸ばしてください。
  *The capacitor value might be too low, making the ON duration too short. Add another 10µF capacitor in parallel to increase the capacitance to 20µF, extending the duration to ~0.2s.*
* **連写した時に2枚目が反応しない場合 / Missed Triggers During Continuous Shooting:**
  コンデンサの容量が大きすぎて、ONの時間が長すぎる状態です。10µFを外して、4.7µF（印字: 475）などの少し小さなコンデンサに変更して、レスポンスのキレを上げてください。
  *The capacitance might be too high, keeping the signal ON for too long. Replace the 10µF with a 4.7µF (marked '475') to improve responsiveness.*

## ⚠️ 免責事項 / Disclaimer

本プロジェクトは実験的なハードウェア改造を含みます。カメラのシンクロ接点やPawPrintのUSB-Cポートの仕様変更等により動作しない場合があります。本情報を使用したことによる機材の破損等について、作者は一切の責任を負いません。自己責任（DIY）でお楽しみください。
*This project involves experimental hardware modifications. It may not work depending on your camera's sync terminal or changes to the PawPrint's USB-C specifications. The author takes no responsibility for any damage to equipment caused by using this information. Please enjoy at your own risk (DIY).*
