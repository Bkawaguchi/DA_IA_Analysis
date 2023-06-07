# DAアルゴリズムとボストン方式に関する比較分析プログラム
## 概要
DAアルゴリズムとボストン方式の二つのアルゴリズムによるマッチングの特性を比較するために, 主に「効率性」（「効用」と「第一希望にマッチした割合」とに分けられる）,「安定性」,「容易性」の三つの評価軸を設け, 様々な規模や選好分布, 応募可能な研究室数といったシフト・パラメターに応じたマッチングに対しこれらの値を導出する. また, それらシフト・パラメターを任意に変更し比較静学的な分析を可能とするプログラムである. 評価軸の概要について以下に説明する.    
※研究室配属問題を前提とした, 学生と研究室の学生提案マッチングを想定したものである.    

(1) 効率性 (efficiency)  
学生と研究室の総合的な効用を測る評価軸である. それぞれのマッチした相手の選好
順位に応じてボルダルールのように値を与えた「効用」（(相手の総数+1)-(マッチした相手が該当する希望順位)）と, 「第一希望にマッチした人数の割合」の二項目によって判断する. 前者について, 値が大きいほど効用が高く, プレイヤーがより望んだ相手とマッチできていることが分かる. また, 比較値においてマッチした学生の数で除し, 学生と研究室それぞれ1人(室)あたりどのくらい好ましい相手とマッチしているかの平均値として算出している.  

(2) 安定性 (stability)  
制度の持続可能性を測る. これには、マッチングによって生まれたブロッキング・ペア
の数を指標として用いる. この値が小さいほどマッチングの安定性が高く, 駆け落ちによるマッチングの崩壊が起こりにくい.  

(3) 容易性 (easiness)  
制度の実現しやすさを表す評価軸である. 具体的には, 学生が研究室にプロポーズし
た回数で算出する. これは, 面接や選考アンケートの手間として捉えてよい. したがって, この値が小さいほどマッチングにかかる手間が少なく済むということである.  

## 選好モデルについて
学生と研究室の選好を生成するモデルについて説明する.
学生と研究室が持つ選好について, より現実的なケースに近似させるため, 人気度や成績に影響を受けるような以下の選好モデルを作成した. それぞれ, 効用 U が高いほど選好順位が高くなる.  

学生の効用 : *Uij = αCVj + (1-α)PVij*  
研究室の効用 : *Uji = βSVi + (1-β)QVji*  

*Uij* = 学生 i の研究室 j に対する効用  
*CVj* = 研究室 j の人気度  
*PVij* = 学生 i の研究室 j に持つ好みの度合い  
*α* = 学生間の研究室に対する好みの相関パラメータ, 0〜1の間で設定(現実には0≦α<1であることが予想できる.)  

*Uji* = 研究室 j の学生 i に対する効用  
*SVi* = 学生 i の人気度  
*QVji* = 研究室 j の学生 i に持つ好みの度合い  
*β* = 研究室間の学生に対する好みの相関パラメータ, 0〜1の間で設定  

例：*α*を0.5に設定  
  *Uij* = 0.5(研究室jの人気度) + 0.5(学生iの研究室jに対する好みの度合い)  
    →学生の選好は研究室jの人気度と学生iの研究室jに対する個人的な好みの度合いを1/2ずつ反映して生成される.  
   
   *β*を0.7に設定  
   *Uji* = 0.7(学生iの人気度) + 0.3(研究室jの学生iに対する好みの度合い)  
   →研究室の選好は学生iの人気度を7/10反映し、個人的な好みの度合いは3/10だけ反映した選好を成す.  

*α*, *β*の値が大きいほど, 学生と研究室はそれぞれの人気度や成績等に影響を受けた選好を持つことになる. すなわち, *α*を 0 に設定すると学生の選好が一様となり, 学生同士の競争が起きにくくなる. 逆に0.9のような値を設定すると研究室の人気度が最大限に反映され, 似通った選好を持つ学生が極めて多くなる. この選好式を用い, 全てのi, jについて*CVj*, *SVi*及び*PVij*, *QVji*を [0, 1]区間の一様分布上でランダムに生成し, 各々の効用 *Uij*, *Uji* を得て選好を作成する. 

## 使用法
コード上のS(学生の人数), C(研究室の数), CP([]内に研究室1つあたりの定員を入力), T(試行回数), *α*, *β*, apply_num(学生が実際に応募できる研究室の数:現実には全ての研究室に対して応募できるようなケースは考えにくく, 応募先は数カ所に限られていると考えられるため.)に調べたい任意の数値を入力し実行する.  
※定員については, CPを無効にして123行目のcapacity = CPに, CPを削除し研究室数の要素を持ったリストを記述することで, 全研究室に個別の定員を設定したシミュレーションをすることもできる.

## 使用例


## あとがき
アルゴリズム部分については 川越敏司『基礎から学ぶマーケット・デザイン』(有斐閣, 2021)を参考にさせていただき, 改良と改変を加えています.
是非本書も手に取っていただきたく願います.
