% 算術階層と極限計算可能性について
% 〜2019/04/30 巨大数勉強会〜 
% 発表者：$\lambda x.x$

# この発表について

- 私は巨大数について全然詳しくないです
  - フィッシュさんの本を読んで勉強中
- 今回話すのは計算論についてです
- 今回の計算論の話は，巨大数を考える上で役に立つかもしれない（立たないかもしれない）

# 仮定する知識
- 初歩的なプログラミング能力
  - 構文解析して意味付けするようなプログラミングをしていればだいたいOK（構文木を作るような電卓のプログラムとか）
- 情報，工学系2年次くらいの計算論の知識
  - チューリングマシン，万能チューリングマシン 
  - 決定可能，半決定可能，決定不能問題など
- ある程度の論理式を読む能力（$\forall, \exists$など）
  - 1階述語論理の完全性などがわかれば十分

# 今回の目標
1. 極限補題を理解する
    - $g$が極限計算可能 $\Leftrightarrow$ $g\preceq_{T} Halt（の特性関数）$ 
1. （ほとんど同じようなことだけど）極限計算可能集合全体$= \Delta_2$を理解する
    - $\mathbb{A}$の特性関数が極限計算可能である  $\Leftrightarrow \mathbb{A} \in  \Delta_2$


# 発表の流れ
1. チューリングマシンと極限計算可能についての概要
1. 決定可能，半決定可能，決定不能についてのおさらい
1. ストリーム計算
1. 神託付きチューリングマシンとTuring還元
1. 休憩（10分くらい）
1. 算術階層について
1. 算術階層の厳密性
1. 極限計算可能性
1. 極限定理
1. （時間があれば）機械学習と極限計算可能性の話（計算論的学習理論）

# 記号の定義

数学記号や略称はいくつか常識の範囲内で使いますが，予め注意しておくのは以下くらい？（あとは気持ちを読み取ってください）

- $\subseteq$: 部分集合，$\subset$: 真部分集合
- $dom(f)$: 関数fの定義域，$f \subseteq \mathbb{A} \to \mathbb{A}$: $f$ は $\mathbb{A} \to \mathbb{A}$ の部分関数 
- $\mathbb{A}^C$: 集合$\mathbb{A}$の補集合，$\mathbb{A}^k$: 集合$\mathbb{A}$の$k$個の直積 ，$\mathbb{B}^{\mathbb{A}}$: $\{f| f: \mathbb{A} \to \mathbb{B} \}$
- $TM$: チューリングマシン（文脈で分かる場合，$TM$全体の集合），$UTM$： 万能$TM$
- $\Sigma$: 文字集合（有限集合），$\Sigma^*$: すべての文字列からなる集合，$L$: 言語（$\Sigma^*$の部分集合）
- $<a_1, a_2, ... , a_n >$: $\Sigma^* \times  \Sigma^* \times ... \times \Sigma^* \to \Sigma^*$: 復号容易な文字列の列の圧縮（自然数の圧縮の可能性もある）
 

# 参考文献

1. 木原貴行，計算可能性理論特論・講義ノート，\url{http://www.math.mi.i.nagoya-u.ac.jp/~kihara/pdf/teach/computability2017fall.pdf}
    - 基本的な定義などは，この資料をベースに
    - 計算論の基本的な内容から，極限計算可能性，アルゴリズム情報理論など多様な内容がわかり易く解説されていてしかも無料で見れるので，日本人に生まれてきたことを感謝することになる
2. 照井一成．算術的階層の厳密性と形式的手法の限界について，\url{http://www.kurims.kyoto-u.ac.jp/~terui/incomp.pdf}
    - 不完全性定理や真理述語の定義不能性などの結果を，算術階層で統一的に解説するという資料
    - こちらも入門者向きにわかりやすく書かれており，特に不完全性定理を学ぶのに良いという話を聞くし，私もそう思う
3. 有川節夫（監修），西野哲朗，石坂裕毅，形式言語の理論 第2刷，丸善，1999
    - 極限学習については，上記を参考に

# $TM$と極限計算可能について
- 板書にて
- 参考
\url{https://www.kyoto-su.ac.jp/project/st/st14_05.html}


# UTM（万能チューリングマシン）

以下を満たすチューリングマシン$U$が存在する（証明は省略）．
\begin{block}{UTM}
\[
  \exists e \in \Sigma^* \  \forall \sigma \in \Sigma^* \{[U](<e,\sigma>) = [M](\sigma) \}
\]
\end{block}

ここで，$e$は$Mの$コード，$U$はコンパイラ（現実のコンパイラも1つのプログラム）と考えられ，（$M$の）コード$e$を利用して関数（プログラム）を作ると考えればよい．

上記のようなチューリングマシン$U$を万能チューリングマシンという．


# $TM$の解釈，自然数上の関数とTMについて

$\forall \sigma \in \Sigma^*$ に対して，チューリングマシン$M$の解釈$[M](\sigma)$を以下のように定義する

\begin{block}{定義： チューリングマシン$M$の解釈}
\[
  [M](\sigma) = \begin{cases}
  [M](\sigma)の値 & （[M](\sigma)\downarrow (停止する場合) ） \\
   未定義 & （[M](\sigma)\uparrow（停止しない場合） ）\\
  \end{cases}
\]
\end{block}

ところで，上記の定義では，チューリングマシンの計算結果は文字列となる．任意の$\Sigma$ において，$\Sigma^*$と$\mathbb{N}$は全単射が構成可能であるため，チューリングマシンを自然数上の部分関数とみなしても問題はない．

- 問題： 言語$L$全体（$\{L | L \subseteq \Sigma^* \}$）はどのくらいの大きさを持つだろうか？

# 計算可能な関数，決定可能（計算可能）な集合について
\begin{block}{定義: 部分関数 $f \subseteq \mathbb{N}^K -> \mathbb{N} $ が計算可能}
\[
  \forall (x_1, x_2,…,x_n) \in \mathbb{N}^k \ \exists M \in TM
\]
\[
  \{(x_1,x_2,…,x_n)\in dom(f) \land f(x_1,x_2,…,x_n)=y \ ならば
\]
\[
  [M](<x_1, x_2,…, x_n>)\downarrow =y\}
\]
\end{block}

関数$f$の値域が$\{0,1\}$の場合，$f$を特性関数（述語のようなもの）とみなせば，$f$が定義する集合$\mathbb{A} \subseteq \mathbb{N}$を定めることができる．

\begin{block}{定義：集合$\mathbb{A}$が決定可能（計算可能）}
\[
\mathbb{A}の特性関数が計算可能
\]
\end{block}
決定可能な集合の全体を$\mathbf{R}$で表す．

- 問題：計算可能な関数全体（$\{f|fは計算可能\}$）の大きさはどのくらいになるだろうか？

# 計算不能な関数，決定不能な問題

\begin{block}{定義: 部分関数 $f \subseteq \mathbb{N}^K \to \mathbb{N} $ が計算不能}
\[
fが計算可能ではない
\]
\end{block}
計算可能な関数全体はたかだか可算であるのに対して，$\mathbb{N}^{\mathbb{N}}$は非可算濃度存在する（$\{0,1\}^{\mathbb{N}}$の時点で非可算）ため，明らかに計算不能な関数は存在する．

\begin{block}{定義：集合$\mathbb{A}$が決定不能}
\[\mathbb{A}に対する計算可能な特性関数が存在しない\]
\end{block}

$Halt$（$<e,n>\in Halt$ \Leftrightarrow $[e](n)\downarrow$）も決定不能な集合の一つである．しかし，$Halt$は，**半分決定可能**であるという性質がある．それは，$Halt$の特性関数（以降，$\chi_{Halt}$とする．これは，計算不能な関数ではあるが，れっきとした自然数上の関数である）に関して，停止する場合に限っていえば対応する$TM$を構成することが可能だからである（単にシミュレートして，止まったら1を返せばよい）．


# 半決定可能
\begin{block}{定義：集合$\mathbb{A}$が半決定可能}
\[
\exists M \in TM \: \forall n \in \mathbb{N} \{ n \in \mathbb{A} \Leftrightarrow [M](n)\downarrow \}
\]
\end{block}
半決定可能な集合の全体を$\mathbf{RE}$で表す．
また，補集合が$\mathbf{RE}$となるような集合全体を$\mathbf{Co\mathchar`-RE}$で表す．つまり，$\mathbb{B} \in \mathbf{Co\mathchar`-RE}$ ならば，$\exists M \in TM\:  \forall n \in \mathbb{N} \{ n \not{\in} \mathbb{B} \Leftrightarrow [M](n)\downarrow \}$となる．明らかに，$Halt^C \in \mathbf{Co\mathchar`-RE}$である．また，明らかに$\mathbf{R} \subset \mathbf{RE}$，$\mathbf{R} \subset \mathbf{Co\mathchar`-RE}$である．

# ポストの定理
\begin{block}{ポストの定理}
\[
\mathbb{A} \in \mathbf{R} \Leftrightarrow (\mathbb{A} \in \mathbf{RE} \land \mathbb{A}^C \in \mathbf{RE})
\]
\end{block}
証明： $\mathbb{A}, \mathbb{A}^C$に対応するチューリングマシン2台を交互に動作させるチューリングマシンを考えれば明らか（また，$\mathbf{RE}$ を $\mathbf{Co\mathchar`-RE}$としても成り立つ）．


\begin{block}{系：$Halt$の性質}
$ Halt \in \mathbf{RE} \land Halt \not{\in} \mathbf{Co\mathchar`-RE} \land Halt^C \in \mathbf{Co\mathchar`-RE} \land Halt^C \not{\in} \mathbf{RE} $
\end{block}
証明：$Halt$の性質，ポストの定理より明らか．

つまり，$\mathbf{RE} \not{\subseteq} \mathbf{Co\mathchar`-RE}$かつ$\mathbf{Co\mathchar`-RE} \not{\subseteq} \mathbf{RE}$であり，$Halt$は真に$\mathbf{RE}$であり，$Halt^C$は真に$\mathbf{Co\mathchar`-RE}$である．

# ストリーム計算のモデル
- チューリングマシンは，「停止性」に重きをおいているが，現在の我々のコンピューティングを考えると，オンラインでのリアルタイム処理をはじめ，無限ループ（非停止）とはまた別の意味で，動き続ける原理が必要である．
- リアルタイム処理で要求される情報のように，計算用のテープとはまた別の無限の情報の放流（ストリーム）を用いた計算モデルを考える
  - また，入力されるストリームとは別に，出力のストリームも当然必要である
- ストリーム計算のモデルを用いると，神託つきTM，極限計算のモデルなども考えやすい

# ストリーム計算のイメージ（再）
- 黒板

# ストリーム計算の定義

\begin{block}{定義：始切片}
$\sigma,\tau \in \mathbb{A}^* $ のとき，$\sigma$ が $\tau$ の始切片であるとは，
\[ \exists \eta \in \mathbb{A}^* \{ \sigma\eta = \tau \} \]
が成り立つことであり，このとき，$\sigma \lesssim  \tau$と記す．
\end{block}

\begin{block}{定義：部分関数が単調}
$\phi \subseteq \mathbb{A}^* \to \mathbb {A}^* $ が単調とは以下が成り立つことである
\[ \sigma \lesssim \tau \ ならば，\ \phi(\sigma) \lesssim \phi(\tau) \]
\end{block}


# ストリーム計算の定義
\begin{block}{定義：}
部分単調関数 $\phi \subseteq \mathbb{A}^* \to \mathbb{A}^*$ が与えられたとき，新たな部分関数$\hat{\phi} \subseteq \mathbb{A}^{\mathbb{N}} \to \mathbb{A}^{\mathbb{N}}$を次のように定義する
\[ \hat{\phi}(X)(n)=m \Leftrightarrow \exists \sigma \lesssim X \{ \phi (\sigma) = m \}\]
\end{block}

\begin{block}{aaa}
定義：計算可能連続の定義
\end{block}


# ストリーム計算のイメージ
- 黒板

# オラクルマシンと神託つき$TM$

- オラクルマシンとはほげほげ
- 神託を使うことで，階層を考えることができる

# 神託チューリングマシン
黒板で

- 問題：＊＊＊＊は，オラクルマシンだと言われたりしているが，本当にそうだろうか？検討をしてみよう

# チューリング還元とチューリングジャンプと無限増大列

\begin{block}{チューリング還元}
関数$f,g$が与えられたとき，$f$が$g$にチューリング還元される（あるいは，$f$が$g$-相対計算可能である）とは，ある部分計算可能な$\Phi \subseteq A^* \to A^*$に対して，$f =  \Phi(g)$となることである．このとき，$f \leq_T g$とあらわす．また，$f \leq_T g$であり，$g \ \not{\leq_T} f$であるとき，$f <_T g$とあらわす．
\end{block}

\begin{block}{定理}
任意の自然数上の関数$g$に対して，$g <_{T} \chi_{Halt}^g$ が成り立つ
\end{block}
証明：$g \leq_T \chi_{Halt}^g$はほとんど明らか．$\chi_{Halt}^g \not{\leq_T} \ g$については，相対計算可能な神託チューリングマシンで再度停止性問題を考える．

上記の定理より，以下のような無限増大列を考えることができる．

\begin{block}{aaa}
\[ \emptyset <_T \emptyset^{'} <_T \emptyset^{''} <_T ... \]
\end{block}

ただし，「'」は，相対的な停止性問題を表す．つまり，$\emptyset^{''}$は$\chi_{Halt}^{\chi_{Halt}}$を表す．

# 相対的$\mathbf{R}$，$\mathbf{RE}$，$\mathbf{Co\mathchar`-RE}$
$\emptyset^n$における$\mathbf{R}$ を $\mathbf{R}^n$とあらわす．$\mathbf{RE}$，$\mathbf{Co\mathchar`-RE}$も同じようにそれぞれ$\mathbf{RE}^n$，$\mathbf{Co\mathchar`-RE}^n$と表す．このとき，同じように階層を考えることができる．

# 算術の論理式

\begin{block}{算術の項}
変項: $\bf {x,y,z}$\\
数項: $\bf{0, S(0), SS(0), SSS(0), ...}$\\
算術の項: 数項, 変項, $\bar{+}$, $\bar{\cdot}$ を用いて構成される項（$\bar{+}$, $\bar{\cdot}$ を使う際は，2変数関数として利用）
\end{block}

\begin{block}{算術の論理式}
算術の論理式: 算術の項と$\bar{=}, \bar{\lnot}, \bar{\land}, \bar{\forall}{\bf x}$から構成される論理式．\\
閉論理式: 自由変更を一つも含まない論理式（文）
\end{block}

**注意：**含意$\bar{\to}$，存在$\bar{\exists}$，限定量化$\bar{\forall} {\bf x} \bar{\leq} t$ 等は，$\bar{\lnot}, \bar{\land}, \bar{\forall}$を用いて定義可能である．

上記の定義だけでは，厳密なSyntaxを与えられていないが，そこは気持ちを感じ取って欲しい．

# 算術の論理式の意味論（算術の項の解釈）


算術の論理式に意味論（真偽）を割り当てる．その前に，算術の項に意味を割り当てる必要があるが，算術の項に対応する意味は，我々が通常解釈するところのその項の値である

\begin{block}{算術の項の解釈$\mathscr{F}_t$}
\[
  \mathscr{F}_t(\tau) = \begin{cases}
   0 & \tau \equiv {\bf 0} \\
   \mathscr{F}_t(\tau') +1 & \tau \equiv {\bf S}( \tau ') \\
   \mathscr{F}_t(a) + \mathscr{F}_t(b) & \tau \equiv  a\ \bar{+}\  b \\
   \mathscr{F}_t(a) \cdot \mathscr{F}_t(b) & \tau \equiv  a \  {\bar \cdot}\  b \\
  \end{cases}
\]
\end{block}


# 算術の論理式の意味論（算術の論理式の解釈）
\begin{block}{算術の論理式の解釈$\mathscr{F}$（閉論理式のみ）}
\[
  \mathscr{F}(a\ \bar{=}\ b)  = \begin{cases}
   True &  \mathscr{F}_t(a) = \mathscr{F}_t(b) \\
   False &  \mathscr{F}_t(a) \neq \mathscr{F}_t(b) \\
  \end{cases}
\]
\[
  \mathscr{F}(A\ \bar{\land}\ B)  = \begin{cases}
   True &  \mathscr{F}(A)\land \mathscr{F}(B) が真 \\
   False &  \mathscr{F}(A)\land \mathscr{F}(B)が偽 \\
  \end{cases}
\]
\[
  \mathscr{F}(\bar{\lnot} A)  = \begin{cases}
   True & \mathscr{F}(A) = False \\
   False &\mathscr{F}(A) = True \\
  \end{cases}
\]
\[
  \mathscr{F}(\bar{\forall}{\bf x}.A)  = \begin{cases}
   True &  \{\mathscr{F}(A[nに対応する数項/{\bf x}])| n \in \mathbb{N}\} = \{True\} \\
   False &  \{\mathscr{F}(A[nに対応する数項/{\bf x}])| n \in \mathbb{N}\} \neq \{True\} \\
  \end{cases}
\]
\end{block}
- 問題：$\bar{\exists} {\bf x}.A$，$\bar{\forall}{\bf x}\leq t.A$ の解釈を定義してみよう

# 標準モデルと算術の論理式が定義視する集合
\begin{block}{標準モデルにおいて真}
（上で定義した$\mathscr{F}$のもとで）$\mathscr{F}(\varphi) = True$であるとき，$\varphi$が標準モデルにおいて真であるといい，
$\mathbb{N} \models \varphi$ と記述する．
\end{block}


\begin{block}{論理式で定義される集合}
$A({\bf x})$を1変数論理式とする．このとき$A$が定める集合$\mathbb{A} \subseteq \mathbb{N}$を次のように定義する
\[
\mathscr{F}(A(自然数nに対する数項)) = True \Leftrightarrow n \in \mathbb{A}
\]
\end{block}

- 問題：$P({\bf x}): \bar{\exists}{\bf y}\{2{\bf y} \bar{=} {\bf x}\}$が$2\mathbb{N}$を定義することを確認せよ．

# 算術的階層（$\Delta_0$論理式，$\Sigma_{i}$論理式，$\Pi_{i}$論理式）
\begin{block}{定義：$\Delta_0$論理式，$\Sigma_{i}$論理式，$\Pi_{i}$論理式}
\begin{eqnarray*}
  &\Delta_0 論理式&： 量化子がある場合，限定量化のみの論理式\\
  &\Sigma_1 論理式&： \exists x_1...\exists x_n\varphi の形の論理式．ただし，n \geq 0 で，\varphi \in \Delta_0\\
  &\Pi_1 論理式&： \forall x_1...\forall x_n\varphi の形の論理式．ただし，n \geq 0 で，\varphi \in \Delta_0\\
  &\Sigma_{i+1} 論理式&： \exists x_1...\exists x_n\varphi の形の論理式．ただし，n \geq 0 で，\varphi \in \Pi_i\\
  &\Pi_{i+1} 論理式&： \forall x_1...\forall x_n\varphi の形の論理式．ただし，n \geq 0 で，\varphi \in \Sigma_i\\ 
\end{eqnarray*}
\end{block}

- $\Sigma$，$\Pi$に対しては，$i>0$

# 算術的階層（$\Sigma_{i}$集合，$\Pi_{i}$集合，$\Delta_{i}$集合）
\begin{block}{定義：$\Sigma_{i}$集合，$\Pi_{i}$集合，$\Delta_i$集合}
\begin{eqnarray*}
  &\Delta_0 集合&： \Delta_0 論理式によって定義可能な自然数の集合\\
  &\Sigma_{i} 集合&： \Sigma_i〃 \\
  &\Pi_{i} 集合&： \Pi_i〃 \\
  &\Delta_i 集合&： \Sigma_i \cap \Pi_i\\
\end{eqnarray*}
\end{block}

- $\Sigma$，$\Pi$に対しては，$i>0$
- $\Delta_i(\Sigma_i, \Pi_i)$集合全体のことを単に$\Delta_i(\Sigma_i, \Pi_i)$という．
- 包含関係があるのは自明だが，真に厳密な包含関係であることをお兄さんと見ていこう


# 算術的階層の厳密性

以下の命題が成り立つ．

- $\mathbf{R} = \Delta_1$
- $\mathbf{RE} = \Sigma_1$
- $\mathbf{Co\mathchar`-RE} = \Pi_1$
- $\mathbb{A} \in \Delta_i \Leftrightarrow \chi_{\mathbb{A}} <_T \emptyset^{i-1}$
- $\mathbf{R}^{i-1} = \Delta_i$
- $\mathbf{RE}^{i-1} = \Sigma_i$
- $\mathbf{Co\mathchar`-RE}^{i-1} = \Pi_i$

\begin{block}{定理：算術的階層は厳密である}
図
\end{block}
証明：


# 休憩
10分程度？

# 極限計算可能性
\begin{block}{$g: \mathbb{N} \to \mathbb{N}$ が極限計算可能（Limit computable）}
次を満たす計算可能関数$\tilde{g} :\mathbb{N} \to \mathbb{N}$ が存在するとき，関数$g$は極限計算可能であるという．
\[ g(k) =   \lim_{s \to \infty}\tilde{g}(k,s) \]
\end{block} 

# 極限補題
極限補題は，極限計算可能性とチューリング還元についての関係性を述べる
\begin{block}{極限補題}
\[
  gが極限計算可能 \Leftrightarrow g\preceq_{T} \chi{Halt}
\]
\end{block}

証明：がんばる 
\begin{block}{系}
\[
  \mathbb{A}が極限計算可能集合  \Leftrightarrow \mathbb{A} \in  \Delta_2
\]
\end{block}

証明：極限補題と，算術階層の定理より明らか

# 極限計算可能性に関して一言
- すばらしすぎてスライドに文字として書くと「「事態」」になるので書けない

# 極限学習（時間が余ったら）
- \url{https://qiita.com/lambda_x_x/items/af69e2b46c44120bf68e}
- 林先生の動画
