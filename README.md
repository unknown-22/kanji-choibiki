# 漢字ちょい引き

[English](README_en.md)

読みや文章を入力して、漢字の形と同じ読みの語をすばやく確認する Web アプリです。ノートの横にスマートフォンを置き、手書き中に漢字を調べる使い方を想定しています。

## 主な機能

- 調べた語を大きな文字で表示
- ひらがな・カタカナの読みから同音語の候補を表示
- kuromoji.js による文章の形態素解析と、文中の漢字を含む語の選択
- 日本語の音声入力（長押し／タップで開始・停止）
- 最近調べた語を最大 12 件保存
- OS の設定に応じたダークモード表示

語の意味や筆順を解説する機能はありません。同音語の候補や音声認識の結果から、目的の表記を選んで使います。

## ローカルで起動する

ビルドや npm パッケージのインストールは不要です。Python 3 がある環境では、このディレクトリで次のコマンドを実行してください。

```sh
python3 -m http.server 8000 --bind 127.0.0.1
```

ブラウザで <http://localhost:8000> を開きます。終了するにはターミナルで `Ctrl+C` を押します。

辞書を `fetch` と XMLHttpRequest で読み込むため、`index.html` を直接開くのではなく HTTP サーバーを利用してください。kuromoji.js とフォントを外部から読み込むため、インターネット接続も必要です。

## 使い方

1. 画面上部の「解析辞書：準備完了」を確認します。
2. 入力欄に「はかる」などの読み、または「体重を量る」などの文を入力し、「調べる」を押します。
3. 同じ読みの候補を押すと、その語を大きく表示します。文中のボタンになっている語を押して、調べる対象を切り替えることもできます。
4. 音声入力を使う場合は、マイクの利用を許可し、「押している間 話す」を長押しして話します。「タップで切替」ではタップで開始・停止できます。

音声入力はブラウザの `SpeechRecognition` / `webkitSpeechRecognition` を利用し、対応状況や権限に依存します。利用できない場合は、入力欄から文字を入力するか、端末のキーボードの音声入力を利用してください。

## ファイル構成

```text
.
├── index.html      # 画面・スタイル・検索処理・音声入力
├── homonyms.json   # 読みから同音語の候補を引くデータ
├── dict/           # kuromoji 用の圧縮済み辞書（12 ファイル）
├── README.md       # 日本語版
└── README_en.md    # 英語版
```

`homonyms.json` は IPADIC から生成した同音語データです。ひらがなの読みをキー、`|` 区切りの候補文字列を値とする JSON です。

```json
{"しまい":"仕舞|仕舞い|姉妹"}
```

`dict/*.dat.gz.wasm` の実体は gzip 圧縮された辞書データです。WebAssembly の実行ファイルではありません。配信環境に合わせた拡張子で、`index.html` 内で kuromoji の `.dat.gz` へのリクエストを `.dat.gz.wasm` に書き換えています。

## 公開方法

`index.html`、`homonyms.json`、`dict/` を同じ相対配置のまま静的ホスティングに配置してください。ライセンス表記を含むこの README も配布物に含めてください。

音声入力を使う公開環境は HTTPS で提供し、ブラウザでマイクの利用を許可してください。辞書ファイルは保存されたバイナリのまま配信し、拡張子に応じた変換などを行わないようにします。

## データ保存と外部サービス

- 検索履歴と音声入力モードは、ブラウザの `localStorage`（`kc-hist` / `kc-mode`）に保存します。
- 形態素解析と同音語の検索は、読み込んだ辞書を使ってブラウザ内で実行します。
- kuromoji.js 0.1.2 は jsDelivr、Klee One と BIZ UDPGothic は Google Fonts から読み込みます。
- 音声認識はブラウザが提供する機能を使います。音声の処理先はブラウザの実装に依存し、このアプリのコードでは録音データを保存していません。

## 第三者ソフトウェア・辞書のライセンス

以下は利用している第三者ソフトウェア・辞書のライセンス文です。本プロジェクト独自のコードのライセンスを指定するものではありません。

### 辞書（IPADIC）

kuromoji.js の配布物に含まれる辞書は `mecab-ipadic-2.7.0-20070801` を使用しています。`homonyms.json` も IPADIC から生成しています。以下に、NAIST の著作権表記と ICOT Free Software の免責条項を含む配布元の通知全文を掲載します。

出典：[kuromoji.js 0.1.2 NOTICE.md](https://cdn.jsdelivr.net/npm/kuromoji@0.1.2/NOTICE.md)

```text
Library dependencies
====================

This software includes a binary and/or source version of data from

* mecab-ipadic-2.7.0-20070801

which can be obtained from

http://atilika.com/releases/mecab-ipadic/mecab-ipadic-2.7.0-20070801.tar.gz

or

http://jaist.dl.sourceforge.net/project/mecab/mecab-ipadic/2.7.0-20070801/mecab-ipadic-2.7.0-20070801.tar.gz



Copyright and license
=====================


mecab-ipadic-2.7.0-20070801
---------------------------

Copyright 2000, 2001, 2002, 2003 Nara Institute of Science
and Technology.  All Rights Reserved.

Use, reproduction, and distribution of this software is permitted.
Any copy of this software, whether in its original form or modified,
must include both the above copyright notice and the following
paragraphs.

Nara Institute of Science and Technology (NAIST),
the copyright holders, disclaims all warranties with regard to this
software, including all implied warranties of merchantability and
fitness, in no event shall NAIST be liable for
any special, indirect or consequential damages or any damages
whatsoever resulting from loss of use, data or profits, whether in an
action of contract, negligence or other tortuous action, arising out
of or in connection with the use or performance of this software.

A large portion of the dictionary entries
originate from ICOT Free Software.  The following conditions for ICOT
Free Software applies to the current dictionary as well.

Each User may also freely distribute the Program, whether in its
original form or modified, to any third party or parties, PROVIDED
that the provisions of Section 3 ("NO WARRANTY") will ALWAYS appear
on, or be attached to, the Program, which is distributed substantially
in the same form as set out herein and that such intended
distribution, if actually made, will neither violate or otherwise
contravene any of the laws and regulations of the countries having
jurisdiction over the User or the intended distribution itself.

NO WARRANTY

The program was produced on an experimental basis in the course of the
research and development conducted during the project and is provided
to users as so produced on an experimental basis.  Accordingly, the
program is provided without any warranty whatsoever, whether express,
implied, statutory or otherwise.  The term "warranty" used herein
includes, but is not limited to, any warranty of the quality,
performance, merchantability and fitness for a particular purpose of
the program and the nonexistence of any infringement or violation of
any right of any third party.

Each user of the program will agree and understand, and be deemed to
have agreed and understood, that there is no warranty whatsoever for
the program and, accordingly, the entire risk arising from or
otherwise connected with the program is assumed by the user.

Therefore, neither ICOT, the copyright holder, or any other
organization that participated in or was otherwise related to the
development of the program and their respective officials, directors,
officers and other employees shall be held liable for any and all
damages, including, without limitation, general, special, incidental
and consequential damages, arising out of or otherwise in connection
with the use or inability to use the program or any product, material
or result produced or otherwise obtained by using the program,
regardless of whether they have been advised of, or otherwise had
knowledge of, the possibility of such damages at any time during the
project or thereafter.  Each user will be deemed to have agreed to the
foregoing by his or her commencement of use of the program.  The term
"use" as used herein includes, but is not limited to, the use,
modification, copying and distribution of the program and the
production of secondary products from the program.

In the case where the program, whether in its original form or
modified, was distributed or delivered to or received by a user from
any person, organization or entity other than ICOT, unless it makes or
grants independently of ICOT any specific warranty to the user in
writing, such person, organization or entity, will also be exempted
from and not be held liable to the user for any such damages as noted
above as far as the program is concerned.
˜˜
```

### kuromoji.js 0.1.2

kuromoji.js は Apache License, Version 2.0 で提供されています。

出典：[ソースの著作権表記](https://cdn.jsdelivr.net/npm/kuromoji@0.1.2/src/kuromoji.js)、[LICENSE-2.0.txt](https://cdn.jsdelivr.net/npm/kuromoji@0.1.2/LICENSE-2.0.txt)

```text
Copyright 2014 Takuya Asano
Copyright 2010-2014 Atilika Inc. and contributors
```

以下は Apache License, Version 2.0 の全文です（末尾の適用例を含みます）。

```text

                                 Apache License
                           Version 2.0, January 2004
                        http://www.apache.org/licenses/

   TERMS AND CONDITIONS FOR USE, REPRODUCTION, AND DISTRIBUTION

   1. Definitions.

      "License" shall mean the terms and conditions for use, reproduction,
      and distribution as defined by Sections 1 through 9 of this document.

      "Licensor" shall mean the copyright owner or entity authorized by
      the copyright owner that is granting the License.

      "Legal Entity" shall mean the union of the acting entity and all
      other entities that control, are controlled by, or are under common
      control with that entity. For the purposes of this definition,
      "control" means (i) the power, direct or indirect, to cause the
      direction or management of such entity, whether by contract or
      otherwise, or (ii) ownership of fifty percent (50%) or more of the
      outstanding shares, or (iii) beneficial ownership of such entity.

      "You" (or "Your") shall mean an individual or Legal Entity
      exercising permissions granted by this License.

      "Source" form shall mean the preferred form for making modifications,
      including but not limited to software source code, documentation
      source, and configuration files.

      "Object" form shall mean any form resulting from mechanical
      transformation or translation of a Source form, including but
      not limited to compiled object code, generated documentation,
      and conversions to other media types.

      "Work" shall mean the work of authorship, whether in Source or
      Object form, made available under the License, as indicated by a
      copyright notice that is included in or attached to the work
      (an example is provided in the Appendix below).

      "Derivative Works" shall mean any work, whether in Source or Object
      form, that is based on (or derived from) the Work and for which the
      editorial revisions, annotations, elaborations, or other modifications
      represent, as a whole, an original work of authorship. For the purposes
      of this License, Derivative Works shall not include works that remain
      separable from, or merely link (or bind by name) to the interfaces of,
      the Work and Derivative Works thereof.

      "Contribution" shall mean any work of authorship, including
      the original version of the Work and any modifications or additions
      to that Work or Derivative Works thereof, that is intentionally
      submitted to Licensor for inclusion in the Work by the copyright owner
      or by an individual or Legal Entity authorized to submit on behalf of
      the copyright owner. For the purposes of this definition, "submitted"
      means any form of electronic, verbal, or written communication sent
      to the Licensor or its representatives, including but not limited to
      communication on electronic mailing lists, source code control systems,
      and issue tracking systems that are managed by, or on behalf of, the
      Licensor for the purpose of discussing and improving the Work, but
      excluding communication that is conspicuously marked or otherwise
      designated in writing by the copyright owner as "Not a Contribution."

      "Contributor" shall mean Licensor and any individual or Legal Entity
      on behalf of whom a Contribution has been received by Licensor and
      subsequently incorporated within the Work.

   2. Grant of Copyright License. Subject to the terms and conditions of
      this License, each Contributor hereby grants to You a perpetual,
      worldwide, non-exclusive, no-charge, royalty-free, irrevocable
      copyright license to reproduce, prepare Derivative Works of,
      publicly display, publicly perform, sublicense, and distribute the
      Work and such Derivative Works in Source or Object form.

   3. Grant of Patent License. Subject to the terms and conditions of
      this License, each Contributor hereby grants to You a perpetual,
      worldwide, non-exclusive, no-charge, royalty-free, irrevocable
      (except as stated in this section) patent license to make, have made,
      use, offer to sell, sell, import, and otherwise transfer the Work,
      where such license applies only to those patent claims licensable
      by such Contributor that are necessarily infringed by their
      Contribution(s) alone or by combination of their Contribution(s)
      with the Work to which such Contribution(s) was submitted. If You
      institute patent litigation against any entity (including a
      cross-claim or counterclaim in a lawsuit) alleging that the Work
      or a Contribution incorporated within the Work constitutes direct
      or contributory patent infringement, then any patent licenses
      granted to You under this License for that Work shall terminate
      as of the date such litigation is filed.

   4. Redistribution. You may reproduce and distribute copies of the
      Work or Derivative Works thereof in any medium, with or without
      modifications, and in Source or Object form, provided that You
      meet the following conditions:

      (a) You must give any other recipients of the Work or
          Derivative Works a copy of this License; and

      (b) You must cause any modified files to carry prominent notices
          stating that You changed the files; and

      (c) You must retain, in the Source form of any Derivative Works
          that You distribute, all copyright, patent, trademark, and
          attribution notices from the Source form of the Work,
          excluding those notices that do not pertain to any part of
          the Derivative Works; and

      (d) If the Work includes a "NOTICE" text file as part of its
          distribution, then any Derivative Works that You distribute must
          include a readable copy of the attribution notices contained
          within such NOTICE file, excluding those notices that do not
          pertain to any part of the Derivative Works, in at least one
          of the following places: within a NOTICE text file distributed
          as part of the Derivative Works; within the Source form or
          documentation, if provided along with the Derivative Works; or,
          within a display generated by the Derivative Works, if and
          wherever such third-party notices normally appear. The contents
          of the NOTICE file are for informational purposes only and
          do not modify the License. You may add Your own attribution
          notices within Derivative Works that You distribute, alongside
          or as an addendum to the NOTICE text from the Work, provided
          that such additional attribution notices cannot be construed
          as modifying the License.

      You may add Your own copyright statement to Your modifications and
      may provide additional or different license terms and conditions
      for use, reproduction, or distribution of Your modifications, or
      for any such Derivative Works as a whole, provided Your use,
      reproduction, and distribution of the Work otherwise complies with
      the conditions stated in this License.

   5. Submission of Contributions. Unless You explicitly state otherwise,
      any Contribution intentionally submitted for inclusion in the Work
      by You to the Licensor shall be under the terms and conditions of
      this License, without any additional terms or conditions.
      Notwithstanding the above, nothing herein shall supersede or modify
      the terms of any separate license agreement you may have executed
      with Licensor regarding such Contributions.

   6. Trademarks. This License does not grant permission to use the trade
      names, trademarks, service marks, or product names of the Licensor,
      except as required for reasonable and customary use in describing the
      origin of the Work and reproducing the content of the NOTICE file.

   7. Disclaimer of Warranty. Unless required by applicable law or
      agreed to in writing, Licensor provides the Work (and each
      Contributor provides its Contributions) on an "AS IS" BASIS,
      WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or
      implied, including, without limitation, any warranties or conditions
      of TITLE, NON-INFRINGEMENT, MERCHANTABILITY, or FITNESS FOR A
      PARTICULAR PURPOSE. You are solely responsible for determining the
      appropriateness of using or redistributing the Work and assume any
      risks associated with Your exercise of permissions under this License.

   8. Limitation of Liability. In no event and under no legal theory,
      whether in tort (including negligence), contract, or otherwise,
      unless required by applicable law (such as deliberate and grossly
      negligent acts) or agreed to in writing, shall any Contributor be
      liable to You for damages, including any direct, indirect, special,
      incidental, or consequential damages of any character arising as a
      result of this License or out of the use or inability to use the
      Work (including but not limited to damages for loss of goodwill,
      work stoppage, computer failure or malfunction, or any and all
      other commercial damages or losses), even if such Contributor
      has been advised of the possibility of such damages.

   9. Accepting Warranty or Additional Liability. While redistributing
      the Work or Derivative Works thereof, You may choose to offer,
      and charge a fee for, acceptance of support, warranty, indemnity,
      or other liability obligations and/or rights consistent with this
      License. However, in accepting such obligations, You may act only
      on Your own behalf and on Your sole responsibility, not on behalf
      of any other Contributor, and only if You agree to indemnify,
      defend, and hold each Contributor harmless for any liability
      incurred by, or claims asserted against, such Contributor by reason
      of your accepting any such warranty or additional liability.

   END OF TERMS AND CONDITIONS

   APPENDIX: How to apply the Apache License to your work.

      To apply the Apache License to your work, attach the following
      boilerplate notice, with the fields enclosed by brackets "[]"
      replaced with your own identifying information. (Don't include
      the brackets!)  The text should be enclosed in the appropriate
      comment syntax for the file format. We also recommend that a
      file or class name and description of purpose be included on the
      same "printed page" as the copyright notice for easier
      identification within third-party archives.

   Copyright [yyyy] [name of copyright owner]

   Licensed under the Apache License, Version 2.0 (the "License");
   you may not use this file except in compliance with the License.
   You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

   Unless required by applicable law or agreed to in writing, software
   distributed under the License is distributed on an "AS IS" BASIS,
   WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
   See the License for the specific language governing permissions and
   limitations under the License.
```
