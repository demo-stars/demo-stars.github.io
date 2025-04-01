{:.no_toc}
* toc
{:toc}

# Abstract

Recent breakthroughs in singing voice synthesis (SVS) have heightened the demand for high-quality annotated datasets, yet manual annotation remains prohibitively labor-intensive and resource-intensive. Existing automatic singing annotation (ASA) methods, however, primarily tackle isolated aspects of the annotation pipeline. To address this fundamental challenge, we present STARS, which is, to our knowledge, the first unified framework that simultaneously addresses singing transcription, alignment, and refined style annotation. Our framework delivers comprehensive multi-level annotations encompassing: (1) precise phoneme-audio alignment, (2) robust note transcription and temporal localization, (3) expressive vocal technique identification, and (4) global stylistic characterization including emotion and pace. The proposed architecture employs hierarchical acoustic feature processing across frame, word, phoneme, note, and sentence levels. The novel non-autoregressive local acoustic encoders enable structured hierarchical representation learning. Experimental validation confirms the framework's superior performance across multiple evaluation dimensions compared to existing annotation approaches. Furthermore, applications in SVS training demonstrate that models utilizing STARS-annotated data achieve significantly enhanced perceptual naturalness and precise style control. This work not only overcomes critical scalability challenges in the creation of singing datasets but also pioneers new methodologies for controllable singing voice synthesis.

![overall](./overall.png)

# ASA (Automatic Singing Annotation)

To assess the performance of STARS in the lyric and note alignment task, we randomly select samples to showcase. The images below represent the mel-spectrogram and phoneme/note segmentation results, while the second section shows the Dynamic Programming (DP) matrix, where the red lines indicate phoneme correspondence. In the third section, we show the f0, ground truth MIDI, and predicted MIDI.

---

### Example 1

**Word**: 也 许 下 个 冬 天 &lt;AP&gt; 也 许 还 十 年

**Phoneme**: ie x v x ia g e d ong t ian &lt;AP&gt; ie x v h ai sh i n ian

<div style="display: block; text-align: center;">
  <img src="figs/la/0000.png" alt="Mel-spectrogram and DP matrix" style="width: 100%; max-width: 900px;">
  <p><strong>Mel-spectrogram and DP matrix</strong></p>
  <img src="figs/note/0000.png" alt="Note Transcription" style="width: 100%; max-width: 600px;">
  <p><strong>Note Transcription</strong></p>
  <audio controls style="width: 40%; max-width: 600px;">
	<source src="wavs/la/0000.wav" type="audio/wav">
  </audio>
  <p><strong>Audio</strong></p>
</div>

---

### Example 2

**Word**: 一 次 就 好 &lt;AP&gt; 我 带 你 去 看 天 荒 地 老

**Phoneme**: i c i j iou h ao &lt;AP&gt; uo d ai n i q v k an t ian h uang d i l ao

<div style="display: block; text-align: center;">
  <img src="figs/la/0001.png" alt="Mel-spectrogram and DP matrix" style="width: 100%; max-width: 900px;">
  <p><strong>Mel-spectrogram and DP matrix</strong></p>
  <img src="figs/note/0001.png" alt="Note Transcription" style="width: 100%; max-width: 600px;">
  <p><strong>Note Transcription</strong></p>
  <audio controls style="width: 40%; max-width: 600px;">
	<source src="wavs/la/0001.wav" type="audio/wav">
  </audio>
  <p><strong>Audio</strong></p>
</div>

---

### Example 3

**Word**: my head's under water but &lt;AP&gt; i'm breathing fine &lt;AP&gt;

**Phoneme**: M AY1 HH EH1 D Z AH1 N D ER0 W AA1 T ER0 B AH1 T &lt;AP&gt; AY1 M B R IY1 DH IH0 NG IH1 N F AY1 N &lt;AP&gt;

<div style="display: block; text-align: center;">
  <img src="figs/la/0002.png" alt="Mel-spectrogram and DP matrix" style="width: 100%; max-width: 900px;">
  <p><strong>Mel-spectrogram and DP matrix</strong></p>
  <img src="figs/note/0002.png" alt="Note Transcription" style="width: 100%; max-width: 600px;">
  <p><strong>Note Transcription</strong></p>
  <audio controls style="width: 40%; max-width: 600px;">
	<source src="wavs/la/0002.wav" type="audio/wav">
  </audio>
  <p><strong>Audio</strong></p>
</div>

---

# SVS (Singing Voice Synthesis)

We introduce a method to integrate global style and phoneme-level technique embeddings into the Singing Voice Synthesis (SVS) model to enable style control. The term **Real** refers to training with the ground truth labels from the dataset, **Pred** refers to training with our model's annotations, and **Mix** refers to training with a combination of real data and our model-annotated data. During inference, we use the real dataset annotations.

### Global Style Control

For global styles, we specify the following attributes for each test target:
- **Range**: low, medium, high
- **Pace**: slow, moderate, fast
- **Emotion**: happy, sad

### Phoneme-Level Technique Control

For phoneme-level styles, we assign one of the following techniques to each phoneme in the target content: mixed, falsetto, breathy, pharyngeal, vibrato,  glissando, weak, strong, bubble.

---

### Example 1

**Word**: &lt;SP&gt; 不 再 看 天 上 太 阳 透 过 云 彩 的 光

**Global Style (range, pace, emotion)**: high, moderate, sad

**Phoneme with Technique**: &lt;SP&gt;(0), b(2), u(2), z(2), ai(2), k(2), an(2,6), an(2,6), t(2), ian(2), sh(2), ang(2), t(2), ai(2), iang(2), t(2), ou(2), g(2), uo(2,6), uo(2,6), vn(2), c(2), ai(2), d(2), e(2,6), e(2,6), g(2), uang(2)

_(0: no technique, 1: mixed, 2: falsetto, 3: breathy, 4: pharyngeal, 5: vibrato, 6: glissando, 7: weak, 8: strong, 9: bubble)_

#### Ground Truth

<table style='width: 20%;'>
	<thead>
		<tr>
			<th style='text-align: center'>Ground Truth</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style='text-align: center'><audio controls style='width: 150px;'><source src='wavs/svs/gt/0000.wav' type='audio/wav'></audio></td>
		</tr>
	</tbody>
</table>

#### Model Training Comparisons

<table style='width: 60%;'>
	<thead>
		<tr>
			<th style='text-align: center'>Real Train</th>
			<th style='text-align: center'>Mix Train</th>
			<th style='text-align: center'>Pred Train</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style='text-align: center'><audio controls style='width: 150px;'><source src='wavs/svs/real/0000.wav' type='audio/wav'></audio></td>
			<td style='text-align: center'><audio controls style='width: 150px;'><source src='wavs/svs/mix/0000.wav' type='audio/wav'></audio></td>
	  <td style='text-align: center'><audio controls style='width: 150px;'><source src='wavs/svs/pred/0000.wav' type='audio/wav'></audio></td>
		</tr>
	</tbody>
</table>

---

### Example 2

**Word**: 在 阳 光 灿 烂 的 日 子 里 开 怀 大 笑

**Global Style (range, pace, emotion)**: medium, fast, happy

**Phoneme with Technique**: z(8), ai(8), iang(6,8), iang(6,8), g(8), uang(8), c(8), an(6,8), an(6,8), l(8), an(8), d(8), e(8), r(8), i(8), z(8), i(8), l(8), i(8), k(8), ai(8), h(8), uai(8), d(8), a(6,8), a(6,8), x(8), iao(8)

_(0: no technique, 1: mixed, 2: falsetto, 3: breathy, 4: pharyngeal, 5: vibrato, 6: glissando, 7: weak, 8: strong, 9: bubble)_

#### Ground Truth

<table style='width: 20%;'>
	<thead>
		<tr>
			<th style='text-align: center'>Ground Truth</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style='text-align: center'><audio controls style='width: 150px;'><source src='wavs/svs/gt/0001.wav' type='audio/wav'></audio></td>
		</tr>
	</tbody>
</table>

#### Model Training Comparisons

<table style='width: 60%;'>
	<thead>
		<tr>
			<th style='text-align: center'>Real Train</th>
			<th style='text-align: center'>Mix Train</th>
			<th style='text-align: center'>Pred Train</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style='text-align: center'><audio controls style='width: 150px;'><source src='wavs/svs/real/0001.wav' type='audio/wav'></audio></td>
			<td style='text-align: center'><audio controls style='width: 150px;'><source src='wavs/svs/mix/0001.wav' type='audio/wav'></audio></td>
	  <td style='text-align: center'><audio controls style='width: 150px;'><source src='wavs/svs/pred/0001.wav' type='audio/wav'></audio></td>
		</tr>
	</tbody>
</table>

---

### Example 3

**Word**: &lt;SP&gt; 远 处 蔚 蓝 天 空 下 涌 动 着 &lt;AP&gt; 金 色 的 麦 浪

**Global Style (range, pace, emotion)**: low, slow, happy

**Phoneme with Technique**: &lt;SP&gt;(0), van(3), ch(3), u(3), uei(3), l(3), an(3), t(3), ian(3), k(3), ong(3), x(3), ia(3), iong(3), d(3), ong(3), zh(3), e(3), &lt;AP&gt;(0), j(3), in(3), s(3), e(3), d(3), e(3), m(3), ai(3), l(3), ang(3)

_(0: no technique, 1: mixed, 2: falsetto, 3: breathy, 4: pharyngeal, 5: vibrato, 6: glissando, 7: weak, 8: strong, 9: bubble)_

#### Ground Truth

<table style='width: 20%;'>
	<thead>
		<tr>
			<th style='text-align: center'>Ground Truth</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style='text-align: center'><audio controls style='width: 150px;'><source src='wavs/svs/gt/0002.wav' type='audio/wav'></audio></td>
		</tr>
	</tbody>
</table>

#### Model Training Comparisons

<table style='width: 60%;'>
	<thead>
		<tr>
			<th style='text-align: center'>Real Train</th>
			<th style='text-align: center'>Mix Train</th>
			<th style='text-align: center'>Pred Train</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style='text-align: center'><audio controls style='width: 150px;'><source src='wavs/svs/real/0002.wav' type='audio/wav'></audio></td>
	  <td style='text-align: center'><audio controls style='width: 150px;'><source src='wavs/svs/mix/0002.wav' type='audio/wav'></audio></td>
			<td style='text-align: center'><audio controls style='width: 150px;'><source src='wavs/svs/pred/0002.wav' type='audio/wav'></audio></td>
		</tr>
	</tbody>
</table>

# Long SVS (Long Singing Voice Synthesis)
We introduce a method to integrate global style and phoneme-level technique embeddings into the Singing Voice Synthesis (SVS) model to enable style control. We use the SVS system trained with our model's annotations. During inference, we use the predicted annotations. We first generated segments of the entire song and then stitched them together to form the complete piece.

### Global Style Control

For global styles, we specify the following attributes for each test target:
- **Range**: low, medium, high
- **Pace**: slow, moderate, fast
- **Emotion**: happy, sad

### Phoneme-Level Technique Control

For phoneme-level styles, we assign one of the following techniques to each phoneme in the target content: mixed, falsetto, breathy, pharyngeal, vibrato,  glissando, weak, strong, bubble. As the technical specifications and phonetic annotations would be too lengthy, we'll provide a simplified description of the technique sequence.

---

### Example 1

**Word**: &lt;AP&gt; 是 故 意 的 吗 是 我 得 罪 谁 了 吗 &lt;AP&gt; 这 一 天 竟 然 &lt;AP&gt; 每 件 事 情 都 失 算 &lt;AP&gt; 只 想 转 个 弯 &lt;AP&gt; 却 绕 到 了 飞 机 场 &lt;AP&gt;  发 现 没 钱 在 身 上 &lt;AP&gt; 乌 云 乌 云 快 走 开 &lt;AP&gt; 你 可 知 道 我 不 常 带 把 伞 &lt;AP&gt; 带 把 伞 &lt;AP&gt; 乌 云 乌 云 快 走 开 &lt;AP&gt; 感 觉 你 在 挑 战 我 的 乐 观 &lt;AP&gt; 的 乐 观 &lt;AP&gt; 你 还 想 怎 么 样 &lt;AP&gt; 搞 得 我 快 抓 狂 &lt;AP&gt; 求 你 帮 个 忙 &lt;AP&gt; 乌 云 乌 云 别 找 我 麻 烦 &lt;AP&gt; 是 注 定 的 吗 我 穿 上 了 白 衬 衫 &lt;AP&gt; 拿 一 杯 咖 啡 &lt;AP&gt; 偏 在 我 身 上 倒 翻 &lt;AP&gt; 不 如 跑 一 趟 &lt;AP&gt; 商 店 它 却 刚 打 烊 &lt;AP&gt; 妙 不 可 言 的 下 场 &lt;AP&gt;  乌 云 乌 云 快 走 开 &lt;AP&gt;你 可 知 道 我 不 常 带 把 伞 &lt;AP&gt; 带 把 伞 &lt;AP&gt; 乌 云 乌 云 快 走 开 &lt;AP&gt; 感 觉 你 在 挑 战 我 的 乐 观 &lt;AP&gt; 的 乐 观 &lt;AP&gt; 你 还 想 怎 么 样 &lt;AP&gt; 搞 得 我 快 抓 狂 &lt;AP&gt; 求 你 帮 个 忙 &lt;AP&gt; 乌 云 乌 云 别 找 我 麻 烦 &lt;AP&gt; 

**Global Style (range, pace, emotion)**: low, moderate, sad

**Phoneme with Technique**: The song primarily employs falsetto technique, with occasional vibrato at phrase endings in some segments.

#### Genration Results

<table style='width: 20%;'>
	<thead>
		<tr>
			<th style='text-align: center'>Genration Results</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style='text-align: center'><audio controls style='width: 150px;'><source src='wavs/lsvs/0001.wav' type='audio/wav'></audio></td>
		</tr>
	</tbody>
</table>

---

### Example 2

**Word**: &lt;AP&gt; 能 够 握 紧 的 就 别 放 了 &lt;AP&gt; 能 够 拥 抱 的 &lt;AP&gt; 就 别 拉 扯 &lt;AP&gt; 时 间 着 急 的 &lt;AP&gt; 冲 刷 着 &lt;AP&gt; 剩 下 了 什 么 &lt;AP&gt; 原 谅 走 过 的 那 些 曲 折 &lt;AP&gt; 原 来 留 下 的 &lt;AP&gt; 都 是 真 的 &lt;AP&gt; 纵 然 似 梦 啊 &lt;AP&gt; 半 醒 着 &lt;AP&gt; 笑 着 哭 着 都 快 活 &lt;AP&gt; 谁 让 &lt;AP&gt; 时 间 是 让 人 促 不 及 防 的 东 西 &lt;AP&gt; 晴 时 有 风 阴 有 时 雨 &lt;AP&gt; 争 不 过 朝 夕 &lt;AP&gt; 又 念 着 往 昔 &lt;AP&gt; 偷 走 了 情 丝 却 留 住 一 个 你 &lt;AP&gt; 岁 月 是 一 场 有 去 无 回 &lt;AP&gt; 的 旅 行 &lt;AP&gt; 好 的 坏 的 &lt;AP&gt; 都 是 风 景 &lt;AP&gt; 别 怪 我 贪 心 &lt;AP&gt; 只 是 不 愿 醒 &lt;AP&gt; 因 为 你 只 为 你 愿 和 我 一 起 &lt;AP&gt; 看 云 淡 风 轻 &lt;AP&gt;

&lt;AP&gt; 时 间 是 让 人 促 不 及 防 的 东 西 &lt;AP&gt; 晴 时 有 风 阴 有 时 雨 &lt;AP&gt; 争 不 过 朝 夕 &lt;AP&gt; 又 念 着 往 昔 &lt;AP&gt; 偷 走 了 青 丝 却 留 住 一 个 你 &lt;AP&gt; 岁 月 是 一 场 有 去 无 回 的 旅 行 &lt;AP&gt; 好 的 坏 的 &lt;AP&gt; 都 是 风 景 &lt;AP&gt; 别 怪 我 贪 心 &lt;AP&gt; 只 是 不 愿 醒 &lt;AP&gt; 因 为 你 只 为 你 愿 和 我 一 起 &lt;AP&gt; 看 云 淡 风 轻 &lt;AP&gt;

**Global Style (range, pace, emotion)**: medium->high, moderate, sad

**Phoneme with Technique**:  The first half uses chest voice, while the latter section transitions to mixed voice.

#### Genration Results

<table style='width: 20%;'>
	<thead>
		<tr>
			<th style='text-align: center'>Genration Results</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style='text-align: center'><audio controls style='width: 150px;'><source src='wavs/lsvs/0002.wav' type='audio/wav'></audio></td>
		</tr>
	</tbody>
</table>

---

### Example 3

**Word**: 让 我 掉 下 眼 泪 的 &lt;AP&gt; 不 止 昨 夜 的 酒 &lt;AP&gt; 让 我 依 依 不 舍 的 &lt;AP&gt; 不 止 你 的 温 柔 &lt;AP&gt; 余 路 还 要 走 多 久 &lt;AP&gt; 你 攥 着 我 的 手 &lt;AP&gt; 让 我 感 到 为 难 的 &lt;AP&gt; 是 挣 扎 的 自 由 &lt;AP&gt; 分 别 总 是 在 九 月 &lt;AP&gt; 回 忆 是 思 念 的 愁 &lt;AP&gt; 深 秋 嫩 绿 的 垂 柳 &lt;AP&gt; 亲 吻 着 我 的 额 头 &lt;AP&gt; 在 那 座 阴 雨 的 小 城 里 &lt;AP&gt; 我 从 未 忘 记 你 &lt;AP&gt; 成 都 &lt;AP&gt; 带 不走 的 &lt;AP&gt; 只 有 你 &lt;AP&gt; 和 我 在 成 都 的 街 头 走 一 走 &lt;AP&gt; 直 到 所 有 的 灯 都 熄 灭 了 也 不 停 留 &lt;AP&gt; 你 会 挽 着 我 的 衣 袖 &lt;AP&gt; 我 会 把 手 揣 进 裤 兜 &lt;AP&gt; 走 到 玉 林 路 的 尽 头 &lt;AP&gt; 坐 在 小 酒 馆 的 门 口

**Global Style (range, pace, emotion)**: low->medium, moderate, sad

**Phoneme with Technique**: Mixed voice is predominantly utilized as the foundational vocal technique.

#### Genration Results

<table style='width: 20%;'>
	<thead>
		<tr>
			<th style='text-align: center'>Genration Results</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td style='text-align: center'><audio controls style='width: 150px;'><source src='wavs/lsvs/0003.wav' type='audio/wav'></audio></td>
		</tr>
	</tbody>
</table>